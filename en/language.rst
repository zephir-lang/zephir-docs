Basic Syntax
============
In this chapter, we’ll discuss the organization of files and namespaces, variable declarations, miscellaneous syntax
conventions, and a few other general concepts.

Organizing Code in Files and Namespaces
---------------------------------------
In PHP, you can place code in any file, without a specific structure. In Zephir, every file must contain a class (and just
one class). Every class must have a namespace, and the directory structure must match the names of the classes and namespaces
used. (This is similar to PSR-4 autoloading conventions, except it's enforced by the language itself.)

For example, given the following structure, the classes in each file must be:

.. code-block:: sh

	mylibrary/
		router/
			exception.zep # MyLibrary\Router\Exception
		router.zep # MyLibrary\Router

Class in mylibrary/router.zep:

.. code-block:: zephir

	namespace MyLibrary;

	class Router
	{

	}

Class in mylibrary/router/exception.zep:

.. code-block:: zephir

	namespace MyLibrary\Router;

	class Exception extends \Exception
	{

	}

Zephir will raise a compiler exception if a file or class is not located in the expected file, or vice versa.

Instruction separation
----------------------
You may have already noticed that there were very few semicolons in the code examples in the previous chapter. You can use
semicolons to separate statements and expressions, as in Java, C/C++, PHP, and similar languages:

.. code-block:: zephir

	myObject->myMethod(1, 2, 3); echo "world";

Comments
--------
Zephir supports 'C'/'C++' comments. These are one line comments with :code:`// ...`, and multi line comments with
:code:`/* ... */`:

.. code-block:: c

	// this is a one line comment

	/**
	 * multi-line comment
	 */

In most languages, comments are simply text ignored by the compiler/interpreter. In Zephir, multi-line comments are also used
as docblocks, and they're exported to the generated code, so they're part of the language!

If a docblock is not located where it is expected, the compiler will throw an exception.

Variable Declarations
---------------------
In Zephir, all variables used in a given scope must be declared. This gives important information to the compiler to perform
optimizations and validations. Variables must be unique identifiers, and they cannot be reserved words.

.. code-block:: zephir

	//Declaring variables for the same type	in the same instruction
	var a, b, c;

	//Declaring each variable in separate lines
	var a;
	var b;
	var c;

Variables can optionally have an initial compatible default value:

.. code-block:: zephir

	//Declaring variables with default values
	var a = "hello", b = 0, c = 1.0;
	int d = 50; bool some = true;

Variable names are case-sensitive, the following variables are different:

.. code-block:: zephir

	//Different variables
	var somevalue, someValue, SomeValue;

Variable Scope
--------------
All variables declared are locally scoped to the method where they were declared:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        public function someMethod1()
        {
            int a = 1, b = 2;
            return a + b;
        }

        public function someMethod2()
        {
            int a = 3, b = 4;
            return a + b;
        }

    }

Super Globals
-------------
Zephir doesn't support global variables - accessing global variables from the PHP userland is not allowed. However, you can
access PHP's *super*-globals as follows:

.. code-block:: zephir

	//Getting a value from _POST
	let price = _POST["price"];

	//Read a value from _SERVER
	let requestMethod = _SERVER["REQUEST_METHOD"];

Local Symbol Table
------------------
Every method or context in PHP has a symbol table that allows you to write variables in a very dynamic way:

.. code-block:: php

	<?php

	$b = 100;
	$a = "b";
	echo $$a; // prints 100

Zephir does not implement this feature, since all variables are compiled down to low-level variables, and there is no way to
know which variables exist in a specific context. If you want to create a variable in the current PHP symbol table, you can
use the following syntax:

.. code-block:: zephir

	//Set variable $name in PHP
	let {"name"} = "hello";

	//Set variable $price in PHP
	let name = "price";
	let {name} = 10.2;
