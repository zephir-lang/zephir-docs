Calling Functions
-----------------
PHP has a rich library of functions you can use in your extensions.
To call a PHP function, you can just refer its name in the Zephir code.

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(var text)
        {
            if strlen(text) != 0 {
                return base64_encode(text);
            }
            return false;
        }

    }

You can call also functions that are expected to exist in the PHP userland but they
aren't built-in with PHP:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(var text)
        {
            if strlen(text) != 0 {
                if function_exists("my_custom_encoder") {
                    return my_custom_encoder($text);
                } else {
                    return base64_encode(text);
                }
            }
            return false;
        }

    }

Note that all PHP functions only receive and return dynamic variables, if you pass a static typed
variable as a parameter, some temporary dynamic variable will be used as a bridge in order to call them:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            if strlen(text) != 0 {
                // an implicit dynamic variable is created to
                // pass the static typed 'text' as parameter
                return base64_encode(text);
            }
            return false;
        }

    }

Similarly, functions return dynamic values that cannot be directly assigned to static
variables without the appropriate cast:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(string text)
        {
            string encoded = "";

            if strlen(text) != 0 {
                let encoded = (string) base64_encode(text);
                return '(' . encoded . ')';
            }
            return false;
        }

    }

Sometimes, we would need to call functions in a dynamic way, you can call them as follows:

.. code-block:: zephir

    namespace MyLibrary;

    class Encoder
    {

        public function encode(var callback, string text)
        {
            return {callback}(text);
        }

    }

Using optimizers
^^^^^^^^^^^^^^^^
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
            throw new CompilerException("'calculate_pi' requires one parameter");
        }

        if (count($expression['parameters']) < 2) {
            throw new CompilerException("'calculate_pi' requires one parameter");
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
            throw new CompilerException("'calculate_pi' requires one parameter");
        }

        if (count($expression['parameters']) < 2) {
            throw new CompilerException("'calculate_pi' requires one parameter");
        }

        /**
         * Process the expected symbol to be returned
         */
        $call->processExpectedReturn($context);

        $symbolVariable = $call->getSymbolVariable();
        if ($symbolVariable->isNotDouble()) {
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

    class CalculatePiOptimizer extends OptimizerAbstract
    {

        public function optimize(array $expression, Call $call, CompilationContext $context)
        {

            if (!isset($expression['parameters'])) {
                throw new CompilerException("'calculate_pi' requires one parameter");
            }

            if (count($expression['parameters']) < 2) {
                throw new CompilerException("'calculate_pi' requires one parameter");
            }

            /**
             * Process the expected symbol to be returned
             */
            $call->processExpectedReturn($context);

            $symbolVariable = $call->getSymbolVariable();
            if ($symbolVariable->isNotDouble()) {
                throw new CompilerException("Calculated PI values only can be stored in double variables", $expression);
            }

            $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);

            return new CompiledExpression('double', 'my_calculate_pi( ' . $resolvedParams[0] . ')', $expression);
        }

    }


