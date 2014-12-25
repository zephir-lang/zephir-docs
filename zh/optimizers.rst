Custom optimizers
=================
Most common functions in Zephir use internal optimizers. An 'optimizer' works like an interceptor for function calls.
An 'optimizer' replaces the call for the function in the PHP userland by direct C-calls which are faster and have a lower
overhead improving performance.

To create an optimizer you have to create a class in the 'optimizers' directory, the following convention must be used:

+--------------------+----------------------------+----------------------------------------------------------+------------------+
| Function in Zephir | Optimizer Class Name       | Optimizer Path                                           | Function in C    |
+====================+============================+==========================================================+==================+
| calculate_pi       | CalculatePiOptimizer       | optimizers/CalculatePiOptimizer.php                      | my_calculate_pi  |
+--------------------+----------------------------+----------------------------------------------------------+------------------+

This is the basic structure for an 'optimizer':

.. code-block:: php

    <?php

    namespace Zephir\Optimizers\FunctionCall;

    use Zephir\Call;
    use Zephir\CompilerException;
    use Zephir\CompilationContext;
    use Zephir\Optimizers\OptimizerAbstract;

    class CalculatePiOptimizer extends OptimizerAbstract
    {

        public function optimize(array $expression, Call $call, CompilationContext $context)
        {
            //...
        }

    }

Implementation of optimizers highly depends on the kind of code you want to generate. In our example, we're going to replace the call to this
function by a call to a c-function. In Zephir, the code used to call this function is:

.. code-block:: zephir

    let pi = calculate_pi(1000);

So, the optimizer will expect just one parameter, we have to validate that to avoid problems later:

.. code-block:: php

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

There are functions that are just called and they don't return any value, our function returns a value that is the calculated PI value. So we need
to be aware that the type of the variable used to received this calculated value is OK:

.. code-block:: php

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

We're checking if the value returned will be stored in a variable type 'double', if not a compiler exception is thrown.

The next thing we need to do is process the parameters passed to the function:

.. code-block:: php

    <?php

    $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);

As a good practice with Zephir is important to create functions that don't modify their parameters, if you are changing the parameters
passed, Zephir will need to allocate memory for constants passed and you have to use getResolvedParams instead of getReadOnlyResolvedParams.

Code returned by these methods is valid C-code that can be used in the code printer to generate the c-function call:

.. code-block:: php

    <?php

    //Generate the C-code
    return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);

All optimizers must return a CompiledExpression instance, this will tell the compiler the type returned by the code and its related C-code.

The complete optimizer code is:

.. code-block:: php

    <?php

    namespace Zephir\Optimizers\FunctionCall;

    use Zephir\Call;
    use Zephir\CompilerException;
    use Zephir\CompilationContext;
    use Zephir\CompiledExpression;
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

The code that implements the function "my_calculate_pi" is written in C and must be compiled along with the extension.

This code must be placed in the ext/ directory where you find appropiate, just check that those files do not conflict with the
files generated by Zephir.

This file must contain the Zend Engine headers and C implementation of the function:

.. code-block:: c

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif

    #include "php.h"
    #include "php_ext.h"

    double my_calculate_pi(zval *accuracy) {
        return 0.0;
    }

This file must be added at a special section in the :doc:`config.json <config>` file:

.. code-block:: javascript

    "extra-sources": [
        "utils/pi.c"
    ]

Check the complete source code of this example `here <https://github.com/phalcon/zephir-samples/tree/master/ext-optimizers>`.


