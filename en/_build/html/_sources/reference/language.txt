Basic Syntax
============
In this chapter, we’ll discuss organization of files and namespaces, variable declarations,
miscellaneous syntax conventions, and a few other concepts.

Organizing Code in Files and Namespaces
---------------------------------------
In PHP, you can place code in any file without a specific structure. In Zephir every file must contain
a class (and just one class). Every class must have a namespace and the directory structure must match
the names of classes and namespaces used.

For example given the following structure the classes in each file must be:

.. code-block:: sh

	mylibrary/
		router/
			exception.zep # MyLibrary\Router\Exception
		router.zep # MyLibrary\Router

Class in mylibrary/router.zep:

.. code-block:: javascript

	namespace MyLibrary;

	class Router
	{

	}

Class in mylibrary/router/exception.zep:

.. code-block:: php

	namespace MyLibrary\Router;

	class Router extends Exception
	{

	}

Zephir will raise a compiler exception if a file or class is not located at the expected file or viceversa.

Instruction separation
----------------------
You may have already noticed that there were very few semicolons in the code examples in the previous chapter.
You can use semicolons to separate statements and expressions, as in Java, C/C++, PHP, and similar languages:

.. code-block:: javascript

	myObject->myMethod(1, 2, 3); echo "world";

Comments
--------
Zephir supports 'C'/'C++' comments, these are one line comments with // and multi line comments with /* ... \*/:

.. code-block:: c

	// this is one line comment

	/**
	 * multi-line comment
	 */

In most languages comments are simply text ignored by the compiler/interpreter. In Zephir,
multi-line comments are also used as docblocks and they're exported to the generated code,
so they're part of the language!.

The compiler would throw an exception if a docblock is not located where is expected.

Variable Declarations
---------------------
In Zephir, all variables used in a given scope must be declared, this process gives important information
to the compiler to perform optimizations and validations:

.. code-block:: javascript

	//Declaring variables for the same type	in the same instruction
	var a, b c;

	//Declaring each variable in different lines
	var a;
	var b;
	var c;

Variables can optionally have an initial compatible default value, you can assign a new value to a variable
as often as you want:

.. code-block:: javascript

	//Declaring variables with default values
	var a = "hello", b = 0, c = 1.0;
	int d = 50; bool some = true;

