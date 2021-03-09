---
layout: default
language: 'ru-ru'
version: '0.10'
---

# Пользовательские оптимизаторы
В большинстве распространенных функций в Zephir используются внутренние оптимизаторы. Оптимизатор работает как перехватчик для вызовов функций. Оптимизатор заменяет вызов функции в пользовательском пространстве PHP прямыми Си-вызовами, которые выполняются быстрее и имеют более низкие накладные расходы, повышающие производительность.

Чтобы создать оптимизатор, вам нужно создать класс в каталоге «optimizers», необходимо использовать следующее соглашение (вы можете настроить название этого каталога в файле `config.json`; см. ниже). Следующие соглашения по именованию являются обязательными:

| Функция в Zephir | Название класса оптимизатора | Путь оптимизатора                     | Функция в Си      |
| ---------------- | ---------------------------- | ------------------------------------- | ----------------- |
| `calculate_pi`   | `CalculatePiOptimizer`       | `optimizers/CalculatePiOptimizer.php` | `my_calculate_pi` |

Обратите внимание, что оптимизатор написан на языке PHP, а не Zephir. Он используется во время компиляции для программной генерации соответствующего Си-кода для вызова вашего расширения. Оптимизатор ответственен за проверку, что аргументы и возвращаемые типы соответствуют тому, что необходимо Си-функции, помогая компилятору Zephir генерировать корректный Си-код.

Вот базовая структура для оптимизатора:

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

Следующее, что нам нужно сделать, это обработать параметры, переданные функции:

```php
<?php

$resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);
```

Хорошей практикой в Zephir считается создание функций, которые не изменяют параметры. Если вы меняете переданные параметры, Zephir нужно будет выделить для них память, и в этом случае вы должны будете использовать `getResolvedParams` вместо `getReadOnlyResolvedParams`.

Код, возвращаемый этими методами, является валидным Си-кодом, который может быть использован при создании кода для генерации вызова Си-функции:

```php
<?php

// Генерируем Си-код
return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);
```

Все оптимизаторы должны возвращать экземпляр CompiledExpression. Это сообщит компилятору тип возвращенного кода и соответствующий ему Си-код.

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
         * Обработка возвращаемого символа
         */
        $call->processExpectedReturn($context);

        $symbolVariable = $call->getSymbolVariable();
        if (!$symbolVariable->isDouble()) {
            throw new CompilerException("Calculated PI values only can be stored in double variables", $expression);
        }

        $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);

        return new CompiledExpression('double', 'my_calculate_pi(' . $resolvedParams[0] .  ')', $expression);
```

Код, реализующий функцию `my_calculate_pi` написан на Си, и должен быть скомпилирован вместе с расширением.

Этот код должен быть помещен в каталог `ext/`, в любую поддиректорию, на ваше усмотрение. Однако убедитесь, что имя файла с Си-кодом не конфликтует с файлами, генерируемыми Zephir.

Файл представленный ниже должен содержать Zend Engine заголовки и реализацию Си-функции:

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
