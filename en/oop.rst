Classes and Objects
===================
Zephir promotes object-oriented programming, this is why you can only export methods
and classes in extensions, also you will see that most of the time, runtime errors raise
exceptions instead of fatal errors or warnings.

Classes
-------
Every Zephir file must implement a class or an interface (and just once). A class structure
is very similar to a PHP class:

.. code-block:: zephir

    namespace Test;

    /**
     * This is a sample class
     */
    class MyClass
    {

    }

Class Modifiers
^^^^^^^^^^^^^^^
The following class modifiers are supported:

*Final*: If a class has this modifier it cannot be extended

.. code-block:: zephir

    namespace Test;

    /**
     * This class cannot be extended by another class
     */
    final class MyClass
    {

    }

*Abstract*: If a class has this modifier it cannot be instantiated

.. code-block:: zephir

    namespace Test;

    /**
     * This class cannot be instantiated
     */
    abstract class MyClass
    {

    }

Implementing Methods
--------------------
The "function" keyword introduces a method. Methods implements the usual visibility modifiers available
in PHP, explicity set a visibility modifier is mandatory in Zephir:

.. code-block:: zephir

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

.. code-block:: zephir

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

Supported Visibilities
^^^^^^^^^^^^^^^^^^^^^^

* Public: Methods marked as "public" are exported to the PHP extension, this means that public methods are visible to the PHP code as well to the extension itself.

* Protected: Methods marked as "protected" are exported to the PHP extension, this means that protected methods are visible to the PHP code as well to the extension itself. However, protected methods can only be called in the scope of the class or in classes that inherit them.

* Private: Methods marked as "private" are not exported to the PHP extension, this means that private methods are only visible to the class where they're implemented.

Supported Modifiers
^^^^^^^^^^^^^^^^^^^

* Deprecated: Methods marked as "deprecated" throwing an E_DEPRECATED error when they are called.

Getter/Setter shortcuts
^^^^^^^^^^^^^^^^^^^^^^^
Like in C#, you can use get/set/toString shortcuts in Zephir, this feature allows to easily write setters and getters for properties without explictly
implementing those methods as such.

For example, without shortcuts we could find code like:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {
        protected myProperty;

        protected someProperty = 10;

        public function setMyProperty(myProperty)
        {
            this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }

        public function setSomeProperty(someProperty)
        {
            this->someProperty = someProperty;
        }

        public function getSomeProperty()
        {
            return this->someProperty;
        }

        public function __toString()
        {
            return this->myProperty;
        }

     }

You can write the same code using shortcuts as follows:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        protected myProperty {
            set, get, toString
        };

        protected someProperty = 10 {
            set, get
        };

    }

When the code is compiled those methods are exported as real methods but you don’t have to write them one by one.

Return Type Hints
^^^^^^^^^^^^^^^^^
Methods in classes and interfaces can have return type hints, these will provide useful extra information to the compiler
to inform you about errors in your application. Consider the following example:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        public function getSomeData() -> string
        {
            // this will throw a compiler exception
            // since the returned value (boolean) does not match
            // the expected returned type string
            return false;
        }

        public function getSomeOther() -> <App\MyInterface>
        {
            // this will throw a compiler exception
            // if the returned object does not implement
            // the expected interface App\MyInterface
            return new App\MyObject;
        }

        public function process()
        {
            var myObject;

            // the type-hint will tell the compiler that
            // myObject is an instance of a class
            // that implement App\MyInterface
            let myObject = this->getSomeOther();

            // the compiler will check if App\MyInterface
            // implements a method called "someMethod"
            echo myObject->someMethod();
        }

    }

A method can have more than one return type. When multiple types are defined, the operator | must be used to separate those types.

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        public function getSomeData(a) -> string|bool
        {
            if a == false {
                return false;
            }
            return "error";
        }

    }

Return Type: Void
^^^^^^^^^^^^^^^^^
Methods can also be marked as ‘void’. This means that a method is not allowed to return any data:

.. code-block:: zephir

    public function setConnection(connection) -> void
    {
        let this->_connection = connection;
    }

Why is this useful? Because the compiler can detect if the program is expecting a returning value from these methods and produce a compiler exception:

.. code-block:: zephir

    let myDb = db->setConnection(connection);
    myDb->execute("SELECT * FROM robots"); // this will produce an exception

Strict/Flexible Parameter Data-Types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In Zephir, you can specify the data type of each parameter of a method. By default, these data-types are flexible,
this means that if a value with wrong (but compatible) data-type is passed, Zephir will try to transparently
convert it to the expected one:

.. code-block:: zephir

    public function filterText(string text, boolean escape=false)
    {
        //...
    }

Above method will work with the following calls:

.. code-block:: php

    <?php

    $o->filterText(1111, 1); // OK
    $o->filterText("some text", null); // OK
    $o->filterText(null, true); // OK
    $o->filterText("some text", true); // OK
    $o->filterText(array(1, 2, 3), true); // FAIL

However, passing a wrong type could be often lead to bugs, a bad use of a specific API would produce unexpected results.
You can disallow the automatic conversion by setting the parameter with a strict data-type:

.. code-block:: zephir

    public function filterText(string! text, boolean escape=false)
    {
        //...
    }

Now, most of the calls with a wrong type will cause an exception due to the invalid data types passed:

.. code-block:: php

    <?php

    $o->filterText(1111, 1); // FAIL
    $o->filterText("some text", null); // OK
    $o->filterText(null, true); // FAIL
    $o->filterText("some text", true); // OK
    $o->filterText(array(1, 2, 3), true); // FAIL

By specifying what parameters are strict and what must be flexible, a developer can set the specific behavior he/she really wants.

Read-Only Parameters
^^^^^^^^^^^^^^^^^^^^
Using the keyword 'const' you can mark parameters as read-only, this helps to respect `const-correctness <http://en.wikipedia.org/wiki/Const-correctness>`_.
Parameters marked with this attribute cannot be modified inside the method:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        // "a" is read-only
        public function getSomeData(const string a)
        {
            // this will throw a compiler exception
            let a = "hello";
        }
    }

When a parameter is declared as read-only the compiler can make safe assumptions and perform further optimizations over these variables.

Implementing Properties
-----------------------
Class member variables are called "properties". By default, they act as PHP properties.
Properties are exported to the PHP extension and are visibles from PHP code.
Properties implement the usual visibility modifiers available in PHP, explicity set
a visibility modifier is mandatory in Zephir:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        public myProperty1;

        protected myProperty2;

        private myProperty3;

    }

Within class methods non-static properties may be accessed by using -> (Object Operator): this->property
(where property is the name of the property):

.. code-block:: zephir

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

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected myProperty1 = null;
        protected myProperty2 = false;
        protected myProperty3 = 2.0;
        protected myProperty4 = 5;
        protected myProperty5 = "my value";

    }

Updating Properties
^^^^^^^^^^^^^^^^^^^
Properties can be updated by accesing them using the '->' operator:

.. code-block:: zephir

    let this->myProperty = 100;

Zephir checks that properties do exist when a program is accesing them, if a property is not declared you will get a compiler exception:

.. code-block:: php

    CompilerException: Property '_optionsx' is not defined on class 'App\MyClass' in /Users/scott/utils/app/myclass.zep on line 62

          this->_optionsx = options;
          ------------^

If you want to avoid this compiler validation or just create a property dynamically, you can enclose the property name using string quotes:

.. code-block:: zephir

    let this->{"myProperty"} = 100;

You can also use a simple variable to update a property, the property name will be taken from the variable:

.. code-block:: zephir

    let someProperty = "myProperty";
    let this->{someProperty} = 100;

Reading Properties
^^^^^^^^^^^^^^^^^^
Properties can be read by accesing them using the '->' operator:

.. code-block:: zephir

    echo this->myProperty;

As when updating, properties can be dynamically read this way:

.. code-block:: zephir

    //Avoid compiler check or read a dynamic user defined property
    echo this->{"myProperty"};

    //Read using a variable name
    let someProperty = "myProperty";
    echo this->{someProperty}

Class Constants
---------------
Class may contain class constants that remain the same and unchangeable once the extension is compiled.
Class constants are exported to the PHP extension allowing them to be used from PHP.

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;

    }

Class constants can be accessed using the class name and the static operator (::):

.. code-block:: zephir

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

.. code-block:: zephir

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

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected static function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public static function someMethod(c, d)
        {
            return self::_someHiddenMethod(c, d);
        }

    }

You can call methods in a dynamic manner as follows:

.. code-block:: zephir

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
            return this->adapter->{methodName}();
        }

    }
