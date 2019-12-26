---
layout: default
language: 'ru-ru'
version: '0.10'
---

# Пользовательские оптимизаторы
В большинстве распространенных функций в Zephir используются внутренние оптимизаторы. Оптимизатор работает как перехватчик для вызовов функций. Оптимизатор заменяет вызов функции в пользовательском пространстве PHP прямыми C-вызовами, которые выполняются быстрее и имеют более низкие накладные расходы, повышающие производительность.

Чтобы создать оптимизатор, вам нужно создать класс в каталоге «optimizers», необходимо использовать следующее соглашение (вы можете настроить название этого каталога в файле `config.json`; см. ниже). Следующие соглашения по именованию являются обязательными:

| Функция в Zephir | Название класса оптимизатора | Путь оптимизатора                     | Функция в Си      |
| ---------------- | ---------------------------- | ------------------------------------- | ----------------- |
| `calculate_pi`   | `CalculatePiOptimizer`       | `optimizers/CalculatePiOptimizer.php` | `my_calculate_pi` |

Обратите внимание, что оптимизатор написан на языке PHP, а не Zephir. Он используется во время компиляции для программной генерации соответствующего Си-кода для вызова вашего расширения. Оптимизатор ответственен за проверку, что аргументы и возвращаемые типы соответствуют тому, что необходимо Си-функции, помогая компилятору Zephir генерировать корректный Си-код.

Вот базовая структура для 'оптимизатора':

```php
<?php

namespace Zephir\Optimizers\FunctionCall;

use Zephir\Call;
use Zephir\CompilationContext;
use Zephir\Compiler\CompilerException;
use Zephir\Optimizers\OptimizerAbstract;

class CalculatePiOptimizer extends OptimizerAbstract
{
    public function optimize(array $expression, Call $call, CompilationContext $context)
    {
        //...

    }
}
```

Реализация оптимизаторов в значительной степени зависит от типа кода, который вы хотите сгенерировать. В нашем примере мы заменим вызов этой функции вызовом Си-функции. В Zephir, код, используемый для вызова этой функции, выглядит так:

```zephir
let pi = calculate_pi(1000);
```

Таким образом, оптимизатор будет ожидать только один параметр, нам необходимо проверить это, чтобы избежать проблем позже:

```php
<?php

public function optimize(array $expression, Call $call, CompilationContext $context)
{

    if (!isset($expression['parameters'])) {
        throw new CompilerException("'calculate_pi' requires one parameter", $expression);
    }

    if (count($expression['parameters']) > 1) {
        throw new CompilerException("'calculate_pi' requires one parameter", $expression);
    }

    //...
}
```

Существуют функции, которые вызываются, но не возвращают никакого значения. Однако наша функция возвращает вычисленное число PI. Поэтому нам нужно проверить, что переменная, используемая для вычисления значения представляет допустимый тип данных:

```php
<?php

public function optimize(array $expression, Call $call, CompilationContext $context)
{

    if (!isset($expression['parameters'])) {
        throw new CompilerException("'calculate_pi' requires one parameter", $expression);
    }

    if (count($expression['parameters']) > 1) {
        throw new CompilerException("'calculate_pi' requires one parameter", $expression);
    }

    /**
     * Обработка возвращаемого символа
     */
    $call->processExpectedReturn($context);

    $symbolVariable = $call->getSymbolVariable();
    if (!$symbolVariable->isDouble()) {
        throw new CompilerException("Calculated PI values only can be stored in double variables", $expression);
    }

    //...
}
```

Здесь мы проверяем, будет ли возвращенное значение сохранено в переменной типа `double`; если нет, то компилятором выбрасывается исключение.

The next thing we need to do is process the parameters passed to the function:

```php
<?php

$resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);
```

A good practice with Zephir is to create functions that don't modify their parameters. If you are changing the parameters passed, Zephir will need to allocate memory for them, and you have to use `getResolvedParams` instead of `getReadOnlyResolvedParams`.

Code returned by these methods is valid C code that can be used in the code printer to generate the C function call:

```php
<?php

// Generate the C-code
return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);
```

All optimizers must return a CompiledExpression instance. This will tell the compiler the type returned by the code, and its related C-code.

The complete optimizer code is:

```php
<?php

namespace Zephir\Optimizers\FunctionCall;

use Zephir\Call;
use Zephir\CompilationContext;
use Zephir\CompiledExpression;
use Zephir\Compiler\CompilerException;
use Zephir\Optimizers\OptimizerAbstract;

class CalculatePiOptimizer extends OptimizerAbstract
{
    public function optimize(array $expression, Call $call, CompilationContext $context)
    {
        if (!isset($expression['parameters'])) {
            throw new CompilerException("'calculate_pi' requires one parameter", $expression);
        }

        if (count($expression['parameters']) > 1) {
            throw new CompilerException("'calculate_pi' requires one parameter", $expression);
        }

        /**
         * Process the expected symbol to be returned
         */
        $call->processExpectedReturn($context);

        $symbolVariable = $call->getSymbolVariable();
        if (!$symbolVariable->isDouble()) {
            throw new CompilerException("Calculated PI values only can be stored in double variables", $expression);
        }

        $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);

        return new CompiledExpression('double', 'my_calculate_pi(' . $resolvedParams[0] . ')', $expression);
    }
}
```

The code that implements the function `my_calculate_pi` is written in C, and must be compiled along with the extension.

This code must be placed in the `ext/` directory wherever you find appropriate; just check that those files do not conflict with the files generated by Zephir.

This file must contain the Zend Engine headers, and the C implementation of the function:

```c
#ifdef HAVE_CONFIG_H
#include "config.h"
#endif

#include "php.h"
#include "php_ext.h"

double my_calculate_pi(zval *accuracy) {
    return 0.0;
}
```

This file must be added at a special section in the [config.json](/{{ page.version }}/{{ page.language }}/config) file:

```json
"extra-sources": [
    "utils/pi.c"
]
```

Lastly you will have to specify where Zephir can find your optimizer by using the `optimizer-dirs` configuration option.

```json
"optimizer-dirs": [
    "optimizers"
]
```

Check the complete source code of this example [here](https://github.com/phalcon/zephir-samples/tree/master/ext-optimizers)
