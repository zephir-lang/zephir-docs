闭包
----
Zephir中我们可以使用闭包或匿名函数，这些方法是与PHP相兼容的且可以直接返回给PHP端来使用:

.. code-block:: zephir

    namespace MyLibrary;

    class Functional
    {

        public function map(array! data)
        {
            return function(number) {
                return number * number;
            };
        }
    }

Zephir中的闭包可以直接在Zephir中使用，也可以作为参数传给其他函数或方法:

.. code-block:: zephir

    namespace MyLibrary;

    class Functional
    {

        public function map(array! data)
        {
            return data->map(function(number) {
                return number * number;
            });
        }
    }

可以用如下简写来定义闭包（lambda表达式）:

.. code-block:: zephir

    namespace MyLibrary;

    class Functional
    {

        public function map(array! data)
        {
            return data->map( number => number * number );
        }
    }
