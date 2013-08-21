Basic Syntax
============

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

.. code-block:: php

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
As in C or PHP, Zephir requires instructions to be terminated with a semicolon at the end of each statement.

.. code-block:: javascript

	echo "hello";

Comments
--------
Zephir supports only supports 'C'/'C++', these are one line comments with // and multi line comments with /* ... \*/:

.. code-block:: c

	//this is one line comment

	/*
	 * multi-line comment
	 */
