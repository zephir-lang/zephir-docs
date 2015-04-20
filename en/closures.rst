Closures
--------
You can use closures or anonymous functions in Zephir, these are PHP compatible and
can be returned to the PHP userland:

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

It also can be executed directly within Zephir and passed as parameter to other functions/methods:

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

A short syntax is available to define closures:

.. code-block:: zephir

    namespace MyLibrary;

    class Functional
    {

        public function map(array! data)
        {
            return number => number * number;
        }
    }
