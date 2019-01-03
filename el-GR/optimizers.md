---
layout: default
language: 'el-GR'
version: '0.11'
---
# Custom optimizers

Most common functions in Zephir use internal optimizers. An 'optimizer' works like an interceptor for function calls. An 'optimizer' replaces calls to a function normally defined in the PHP userland, by direct C calls, which are faster and have a lower overhead, improving performance.

To create an optimizer, you have to create a class in the 'optimizers' directory (you can configure this directory's name in `config.json`; see below). The following naming convention must be used:

| Function in Zephir | Optimizer Class Name   | Optimizer Path                        | Function in C     |
| ------------------ | ---------------------- | ------------------------------------- | ----------------- |
| `calculate_pi`     | `CalculatePiOptimizer` | `optimizers/CalculatePiOptimizer.php` | `my_calculate_pi` |

Note that an optimizer is written in PHP, not Zephir. It is used during compilation to programmatically generate the appropriate C code for your extension to call. It is responsible for checking that arguments and return types match what the C function actually requires, preventing Zephir from generating invalid C code.

This is the basic structure for an 'optimizer':

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
    

Implementation of optimizers highly depends on the kind of code you want to generate. In our example, we're going to replace the call to this function by a call to a C function. In Zephir, the code used to call this function is:

    let pi = calculate_pi(1000);
    

So, the optimizer will expect just one parameter, we have to validate that to avoid problems later:

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