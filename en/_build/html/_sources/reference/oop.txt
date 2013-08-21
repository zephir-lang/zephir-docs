Classes and Objects
===================
Zephir promotes object-oriented programming, this is why you can only export methods
and classes in extensions, also you will see that most of the time, runtime errors raise
exceptions instead of fatal errors or warnings.

Classes
^^^^^^^
Every Zephir file must implement a class or an interface (and just once). A class structure
is very similar to a PHP class:

.. code-block:: javascript

    namespace Test;

    /**
     * This is a sample class
     */
    class MyClass
    {

    }



Methods
^^^^^^^

Methods implements the usual visibility modifiers available in PHP:

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        public function myPublicMethod()
        {
            // ...
        }

        protected function myProtectedMethod()
        {
            // ...
        }

        private function myPrivateMethod()
        {
            // ...
        }
    }
