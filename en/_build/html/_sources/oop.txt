Classes and Objects
===================
Zephir promotes object-oriented programming, this is why you can only export methods
and classes in extensions, also you will see that most of the time, runtime errors raise
exceptions instead of fatal errors or warnings.

Classes
-------
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

Implementing Methods
--------------------
The "function" keyword introduces a method. Methods implements the usual visibility modifiers available
in PHP, explicity set a visibility modifier is mandatory in Zephir:

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

Implementing Properties
-----------------------
Class member variables are called "properties". By default, they act as PHP properties.
Properties are exported to the PHP extension and are visibles from PHP code.
Properties implement the usual visibility modifiers available in PHP, explicity set
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

        public function setMyProperty(var myProperty)
        {
            let this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }

    }

Properties can have literal compatible default values. These values must be able to be evaluated at
compile time and must not depend on run-time information in order to be evaluated:

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        protected myProperty1 = null;
        protected myProperty2 = false;
        protected myProperty3 = 2.0;
        protected myProperty4 = 5;
        protected myProperty5 = "my value";

    }

Class Constants
---------------
Class may contain class constants that remain the same and unchangeable once the extension is compiled.
Class constants are exported to the PHP extension allowing them to be used from PHP.

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;

    }

Class constants can be accessed using the class name and the static operator (::):

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;

        public function someMethod()
        {
            return MyClass::MYCONSTANT1;
        }

    }

Calling Methods
---------------
Methods can be called using the object operator (->) as in PHP:

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        protected function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public function someMethod(c, d)
        {
            return this->_someHiddenMethod(c, d);
        }

    }

Static methods must be called using the static operator (::):

.. code-block:: javascript

    namespace Test;

    class MyClass
    {

        protected static function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public static function someMethod(c, d)
        {
            return self::someHiddenMethod(c, d);
        }

    }

You can call methods in a dynamic manner as follows:

.. code-block:: javascript

    namespace Test;

    class MyClass
    {
        protected adapter;

        public function setAdapter(var adapter)
        {
            let this->adapter = adapter;
        }

        public function someMethod(var methodName)
        {
            return $this->adapter->{methodName}();
        }

    }
