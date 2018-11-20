# 自定义优化器

Zephir 中最常见的函数使用内部优化器。 "优化器" 的工作方式类似于函数调用的拦截器。 一个“优化器”取代了对PHP代码块中通常定义的函数调用的直接C调用，后者更快，开销更低，从而提高了性能。

要创建优化器，您必须在“优化器”目录中创建一个类(您可以在`config.json`中配置该目录的名称; 见下文)。 必须使用以下命名约定:

| 在 Zephir的作用    | 优化器类名                  | 优化器路径                                 | C 中的函数            |
| -------------- | ---------------------- | ------------------------------------- | ----------------- |
| `calculate_pi` | `CalculatePiOptimizer` | `optimizers/CalculatePiOptimizer.php` | `my_calculate_pi` |

请注意, 优化器是用 PHP 编写的, 而不是 Zephir编写的。 在编译过程中, 它用于以编程方式为您的扩展调用生成适当的 c 代码。 它负责检查参数和返回类型是否与 c 函数实际需要的内容相匹配, 从而防止 Zephir 生成无效的 c 代码。

这是 "优化器" 的基本结构:

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
    

优化器的实现在很大程度上取决于要生成的代码类型。 在我们的示例中, 我们将用对 c 函数的调用来替换对此函数的调用。 在 Zephir 中, 用于调用此函数的代码是:

    let pi = calculate_pi(1000);
    

因此, 优化器只需要一个参数, 我们必须验证这一点, 以避免以后出现问题:

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
    

There are functions that are just called and don't return any value. Our function returns a value that is the calculated PI value. So we need to check that the type of the variable used to receive this calculated value is OK:

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
    

We're checking if the value returned will be stored in a variable of type `double`; if not, a compiler exception is thrown.

The next thing we need to do is process the parameters passed to the function:

    <?php
    
    $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);
    

A good practice with Zephir is to create functions that don't modify their parameters. If you are changing the parameters passed, Zephir will need to allocate memory for them, and you have to use `getResolvedParams` instead of `getReadOnlyResolvedParams`.

Code returned by these methods is valid C code that can be used in the code printer to generate the C function call:

    <?php
    
    // Generate the C-code
    return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);
    

All optimizers must return a CompiledExpression instance. This will tell the compiler the type returned by the code, and its related C-code.

The complete optimizer code is:

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
    

The code that implements the function `my_calculate_pi` is written in C, and must be compiled along with the extension.

This code must be placed in the `ext/` directory wherever you find appropriate; just check that those files do not conflict with the files generated by Zephir.

This file must contain the Zend Engine headers, and the C implementation of the function:

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif
    
    #include "php.h"
    #include "php_ext.h"
    
    double my_calculate_pi(zval *accuracy) {
        return 0.0;
    }
    

This file must be added at a special section in the [config.json](/[[language]]/[[version]]/config) file:

    "extra-sources": [
        "utils/pi.c"
    ]
    

Lastly you will have to specify where Zephir can find your optimizer by using the `optimizer-dirs` configuration option.

    "optimizer-dirs": [
        "optimizers"
    ]
    

Check the complete source code of this example [here](https://github.com/phalcon/zephir-samples/tree/master/ext-optimizers)