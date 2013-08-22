Arrays
======
Array manipulation in Zephir provides a way to use PHP arrays_.
An array is an implementation of a `hash table`_.

Creating Arrays
---------------
An array is created enclosing their elements in square brackets:

.. code-block:: javascript

	//Creating an empty array
	let elements = [];

	//Creating an array with elements
	let elements = [1, 3, 4];

	//Creating an array with elements of different types
	let elements = ["first", 2, true];

	//A multidimensional array
	let elements = [[0, 1], [4, 5], [2, 3]];

As PHP, hashes or dictionaries are supported:

.. code-block:: javascript

	//Creating a hash with string keys
	let elements = ["foo": "bar", "bar": "foo"];

	//Creating a hash with numeric keys
	let elements = [4: "bar", 8: "foo"];

	//Creating a hash with mixed string and numeric keys
	let elements = [4: "bar", 8: "foo"];

Updating arrays
---------------
Arrays are updated in the same way as PHP using square brackets:

.. code-block:: javascript

	//Updating an array with a string key
	let elements["foo"] = "bar";

	//Updating an array with a numeric key
	let elements[0] = "bar";

Appending elements
------------------
Elements can be appended at the end of the array as follows:

.. code-block:: javascript

	//Append an element to the array
	let elements[] = "bar";

Reading elements from arrays
----------------------------
Is possible to read array elements as follows:

.. code-block:: javascript

	//Getting an element using the string key "foo"
	let foo = elements["foo"];

	//Getting an element using the numeric key 0
	let foo = elements[0];

.. _arrays: http://www.php.net/manual/en/language.types.array.php
.. _`hash table`: http://en.wikipedia.org/wiki/Hash_table
