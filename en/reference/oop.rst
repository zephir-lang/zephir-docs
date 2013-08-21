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
Methods implements the usual visibility modifiers available in PHP, explicity set
a visibility modifier is mandatory in Zephir:

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

Methods can receive required and optional parameters:

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        /**
         * All parameters are required
         */
        public function doSum1(a, b)
        {
            return a + b;
        }

        /**
         * Just 'a' is required, 'b' is optional and it has a default value
         */
        public function doSum2(a, b=3)
        {
            return a + b;
        }

        /**
         * Both parameters are optional
         */
        public function doSum3(a=1, b=2)
        {
            return a + b;
        }

        /**
         * Parameters are required and their values must be integer
         */
        public function doSum4(int a, int b)
        {
            return a + b;
        }

        /**
         * Static typed with default values
         */
        public function doSum4(int a=4, int b=2)
        {
            return a + b;
        }

    }

Public Visibility
^^^^^^^^^^^^^^^^^
Methods marked as "public" are exported to the PHP extension, this means that public methods
are visible to the PHP code as well to the extension itself.

Protected Visibility
^^^^^^^^^^^^^^^^^^^^
Methods marked as "protected" are exported to the PHP extension, this means that protected methods
are visible to the PHP code as well to the extension itself. However, protected methods can only
be called in the scope of the class or in classes that inherit them.

Private Visibility
^^^^^^^^^^^^^^^^^^
Methods marked as "private" are not exported to the PHP extension, this means that private methods
are only visible to the class where they're implemented.

Properties
^^^^^^^^^^
Class member variables are called "properties". By default, they act as PHP properties.
Properties implements the usual visibility modifiers available in PHP, explicity set
a visibility modifier is mandatory in Zephir:

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        public myProperty1;

        protected myProperty2;

        private myProperty3;

    }

Within class methods non-static properties may be accessed by using -> (Object Operator): this->property
(where property is the name of the property):

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        protected myProperty;

        public function setMyProperty(myProperty)
        {
            this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }

    }

