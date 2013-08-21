Basic Syntax
============
In this chapter, we’ll discuss organization of files and namespaces, variable declarations,
miscellaneous syntax conventions, and a few other concepts.

File and Directory structure
----------------------------
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

	//this is one line comment

	/*
	 * multi-line comment
	 */


You might wonder why you don’t need parentheses (...) around the evaluated expressions