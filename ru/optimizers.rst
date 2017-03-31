Пользовательские оптимизаторы
=============================
В большинстве распространенных функций в Zephir используются внутренние оптимизаторы. 'optimizer' работает как перехватчик 
для вызовов функций. 'optimizer' заменяет вызов функции в пользовательском пространстве PHP прямыми C-вызовами, 
которые выполняются быстрее и имеют более низкие накладные расходы, повышающие производительность.

Чтобы создать оптимизатор, вам нужно создать класс в каталоге 'optimizers', необходимо использовать следующее соглашение:

+--------------------+------------------------------+----------------------------------------+------------------+
| Функция в Zephir   | Название класса оптимизатора | Путь оптимизатора                      | Функция в C      |
+====================+==============================+========================================+==================+
| calculate_pi       | CalculatePiOptimizer         | optimizers/CalculatePiOptimizer.php    | my_calculate_pi  |
+--------------------+------------------------------+----------------------------------------+------------------+

Это основная структура для 'optimizer':

.. code-block:: php

    <?php

    class CalculatePiOptimizer extends OptimizerAbstract
    {

        public function optimize(array $expression, Call $call, CompilationContext $context)
        {
            //...
        }

    }

Реализация оптимизаторов в большой степени зависит от типа кода, который вы хотите сгенерировать. 
В нашем примере мы собираемся заменить вызов этой функции вызовом c-функции. 
В Zephir код, используемый для вызова этой функции:

.. code-block:: zephir

    let pi = calculate_pi(1000);

Таким образом, оптимизатор будет ожидать только один параметр, мы должны подтвердить это, чтобы избежать проблем позже:

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

Есть только что вызванные функции и они не возвращают никакого значения, наша функция возвращает значение, 
которое является вычисленным значением PI. Поэтому мы должны знать, что тип переменной, 
используемой для получения этого вычисленного значения, - ОК:

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
         * Обработка возвращаемого символа
         */
        $call->processExpectedReturn($context);

        $symbolVariable = $call->getSymbolVariable();
        if ($symbolVariable->isNotDouble()) {
            throw new CompilerException("Calculated PI values only can be stored in double variables", $expression);
        }

        //...
    }

Мы проверяем, будет ли возвращаемое значение храниться в типе переменной 'double', если не выбрано исключение компилятора.

Следующее, что нам нужно сделать, это обработать параметры, переданные функции:

.. code-block:: php

    <?php

    $resolvedParams = $call->getReadOnlyResolvedParams($expression['parameters'], $context, $expression);

Как хорошая практика в Zephir важна для создания функций, которые не изменяют их параметры, 
если вы изменяете переданные параметры, Zephir нужно будет распределить память для переданных констант, 
и вам придется использовать getResolvedParams вместо getReadOnlyResolvedParams.

Код, возвращаемый этими методами, является допустимым C-кодом, 
который может использоваться в принтере кода для генерации вызова c-функции:

.. code-block:: php

    <?php

    //Generate the C-code
    return new CompiledExpression('double', 'calculate_pi( ' . $resolvedParams[0] . ')', $expression);

Все оптимизаторы должны возвращать экземпляр CompiledExpression, это сообщит компилятору тип, 
возвращаемый кодом и связанным с ним C-кодом.

Полный код оптимизатора:

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
             * Обработка возвращаемого символа
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


