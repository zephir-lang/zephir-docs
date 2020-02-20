---
layout: default
language: 'ru-ru'
version: '0.12'
---

# Настраиваемые оптимизаторы

Most common functions in Zephir use internal optimizers. An 'optimizer' works like an interceptor for function calls. An 'optimizer' replaces calls to a function normally defined in the PHP userland, by direct C calls, which are faster and have a lower overhead, improving performance.

To create an optimizer, you have to create a class in the 'optimizers' directory (you can configure this directory's name in `config.json`; see below). The following naming convention must be used:

| Функция в Zephir | Optimizer Class Name   | Путь оптимизатора                     | Функция в C       |
| ---------------- | ---------------------- | ------------------------------------- | ----------------- |
| `calculate_pi`   | `CalculatePiOptimizer` | `optimizers/CalculatePiOptimizer.php` | `my_calculate_pi` |


Note that an optimizer is written in PHP, not Zephir. It is used during compilation to programmatically generate the appropriate C code for your extension to call. It is responsible for checking that arguments and return types match what the C function actually requires, preventing Zephir from generating invalid C code.

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

Реализация оптимизаторов в значительной степени зависит от типа кода, который вы хотите сгенерировать. In our example, we're going to replace the call to this function by a call to a C function. В Zephir, код, используемый для вызова этой функции, выглядит так:

```zephir
let pi = calculate_pi(1000);
```

So, the optimizer will expect just one parameter, we have to validate that to avoid problems later:

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

Существуют функции, которые вызываются, но не возвращают никакого значения. Наша функция возвращает вычисленное число PI. So we need to check that the type of the variable used to receive this calculated value is OK:

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
     * Process the expected symbol to be returned
     */
    $call->processExpectedReturn($context);

    $symbolVariable = $call->getSymbolVariable();
    if (!$symbolVariable->isDouble()) {
        throw new CompilerException("Calculated PI values only can be stored in double variables", $expression);
    }

    //...
}
```

We're checking if the value returned will be stored in a variable of type `double`; if not, a compiler exception is thrown.

Следующее, что нам нужно сделать, это обработать параметры, переданные функции:

```php
<?php

$resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);
```

Хорошей практикой в Zephir считается создание функций, которые не изменяют параметры. If you are changing the parameters passed, Zephir will need to allocate memory for them, and you have to use `getResolvedParams` instead of `getReadOnlyResolvedParams`.

Code returned by these methods is valid C code that can be used in the code printer to generate the C function call:

```php
<?php

// Generate the C-code
return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);
```

Все оптимизаторы должны возвращать экземпляр CompiledExpression. This will tell the compiler the type returned by the code, and its related C-code.

Полный код оптимизатора:

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

        return new CompiledExpression('double', 'my_calculate_pi(' . $resolvedParams[0] .  ')', $expression);
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

Этот файл должен быть добавлен в специальную секцию файла [config.json](/{{ page.version }}/{{ page.language }}/config):

```json
"extra-sources": [
    "utils/pi.c"
]
```

Наконец, вы должны указать, где Zephir может найти ваш оптимизатор при помощи опции конфигурации `optimizer-dirs`.

```json
"optimizer-dirs": [
    "optimizers"
]
```

Check the complete source code of this example [here](https://github.com/zephir-lang/zephir-samples/tree/master/ext-optimizers)