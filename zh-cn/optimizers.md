* * *

layout: default language: 'en' version: '0.10'

* * *

# 自定义优化器

Zephir 中最常见的函数使用内部优化器。 "优化器" 的工作方式类似于函数调用的拦截器。 一个“优化器”取代了对PHP代码块中通常定义的函数调用的直接C调用，后者更快，开销更低，从而提高了性能。

要创建优化器，您必须在“优化器”目录中创建一个类(您可以在`config.json`中配置该目录的名称; 见下文)。 必须使用以下命名约定:

| 在 Zephir的作用    | 优化器类名                  | 优化器路径                                 | C 中的函数            |
| -------------- | ---------------------- | ------------------------------------- | ----------------- |
| `calculate_pi` | `CalculatePiOptimizer` | `optimizers/CalculatePiOptimizer.php` | `my_calculate_pi` |

请注意, 优化器是用 PHP 编写的, 而不是 Zephir编写的。 在编译过程中, 它用于以编程方式为您的扩展调用生成适当的 c 代码。 它负责检查参数和返回类型是否与 c 函数实际需要的内容相匹配, 从而防止 Zephir 生成无效的 c 代码。

这是 "优化器" 的基本结构:

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

优化器的实现在很大程度上取决于要生成的代码类型。 在我们的示例中, 我们将用对 c 函数的调用来替换对此函数的调用。 在 Zephir 中, 用于调用此函数的代码是:

```zephir
let pi = calculate_pi(1000);
```

因此, 优化器只需要一个参数, 我们必须验证这一点, 以避免以后出现问题:

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

有一些函数只是调用, 不返回任何值。 我们的函数返回一个值, 该值是计算出的 pi 值。 因此, 我们需要检查用于接收此计算值的变量的类型是否为 "确定":

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

我们正在检查返回的值是否将存储在 `double` 类型的变量中; 否则, 将引发编译器异常。

接下来我们需要做的是处理传递给函数的参数:

```php
<?php

$resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);
```

Zephir 的一个好做法是创建不修改其参数的函数。 如果要更改传递的参数, Zephir 将需要为其分配内存, 并且必须使用 `getResolvedParams` 而不是 `getReadOnlyResolvedParams`。

这些方法返回的代码是有效的 c 代码, 可在代码打印机中用于生成 c 函数调用:

```php
<?php

// Generate the C-code
return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);
```

所有优化器都必须返回一个编译表达式实例。 这将告诉编译器代码返回的类型及其相关的c代码。

完整的优化器代码是:

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

实现函数`my_calculate_pi`的代码是用C编写的，必须与扩展一起编译。

这段代码必须放在`ext/`目录中任何您认为合适的位置; 检查这些文件是否与Zephir生成的文件冲突。

这个文件必须包含Zend Engine标头，以及函数的C实现:

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

This file must be added at a special section in the [config.json](/0.10/en/config) file:

```json
"extra-sources": [
    "utils/pi.c"
]
```

最后, 您必须使用 `optimizer-dirs` 配置选项指定 Zephir 在何处可以找到优化器。

```json
"optimizer-dirs": [
    "optimizers"
]
```

检查此示例的完整源代码 [here](https://github.com/phalcon/zephir-samples/tree/master/ext-optimizers)