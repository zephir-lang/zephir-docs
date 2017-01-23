自定义优化
=================
Zephir用到的许多函数是使用内部的优化器实现的。优化器像是函数调用的转接器。优化器替换掉了PHP中的函数调用，而采用扩展提供的C版本的函数，这样可以获得更好的执行性能。

要创建优化器，你必须先在optimizers目录中创建一个类，惯例如下:

+--------------------+----------------------+-------------------------------------+------------------+
| Zephir中的函数名   | 优化器类名           | 优化器目录                          | C中的函数名      |
+====================+======================+=====================================+==================+
| calculate_pi       | CalculatePiOptimizer | optimizers/CalculatePiOptimizer.php | my_calculate_pi  |
+--------------------+----------------------+-------------------------------------+------------------+

下面是优化器的基本结构:

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

优化器的实现高度的依赖于你要生成哪类的代码。在我们例子中，我们将会使用一个C版本的函数来代替PI的值计算。Zephir中要调用这个函数的代码如下:

.. code-block:: zephir

    let pi = calculate_pi(1000);

因为优化器只需要一个参数，我们要先在函数中进行参数的正确性检查:

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

有些函数是不返回值的，我们写的这个函数返回一个计算出的PI值。所以我们还要注意到我们的结果也要是正确的类型：

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

下面我们还要检查一下返回值类型是否保存为double型，如果不是的话则会抛出一个异常。

下面要做的即是处理传递给函数的参数:

.. code-block:: php

    <?php

    $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);

一个比较实用的建议是创建一个有只读参数的函数，如果你修改了传递过去的参数，Zephir会为其常量分配内存，这时你不得不使用getResolvedParams
来代替getReadOnlyResolvedParams。

这些方法的返回值通常是可以被用在代码生成器中的可以产生C函数的合法的C代码:

.. code-block:: php

    <?php

    //生成C代码
    return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);

优化器必须返回一个CompiledExpression实例，这样做主要用来告诉编译器生成的返回值类型与相关的C代码。

完整版本的优化器如下:

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

代码中实现的my_calculate_pi函数是使用C实现的，且必须和扩展一起编译。

上述代码必须放在ext/目录下，这样你可就可以很容易的找到了，要确保新生成的代码不和Zephir生成的文件相冲突。

这个文件必须包含Zend Engine头文件的引用与其C版本函数的实现:

.. code-block:: c

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif

    #include "php.h"
    #include "php_ext.h"

    double my_calculate_pi(zval *accuracy) {
        return 0.0;
    }

且这个文件必须包含在 :doc:`config.json <config>` 文件中:

.. code-block:: javascript

    "extra-sources": [
        "utils/pi.c"
    ]

是查看完整版本的例子可以点击这里 `here <https://github.com/phalcon/zephir-samples/tree/master/ext-optimizers>`。

