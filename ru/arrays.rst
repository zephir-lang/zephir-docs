Массивы
=======
Работа с массивом в Zephir работает так же как и в PHP arrays_.
Массивы реализованы через `hash table`_.

Объявление переменных массива
-----------------------------
Переменная тип массив может быть обьявлена с помощью ключевых слов 'var' or 'array':

.. code-block:: zephir

	var a = []; // array variable, its type can be changed
	array b = []; // array variable, its type cannot be changed across execution

Создание массивов
-----------------
An array is created enclosing their elements in square brackets:

.. code-block:: zephir

	//Creating an empty array
	let elements = [];

	//Creating an array with elements
	let elements = [1, 3, 4];

	//Creating an array with elements of different types
	let elements = ["first", 2, true];

	//A multidimensional array
	let elements = [[0, 1], [4, 5], [2, 3]];

As PHP, hashes or dictionaries are supported:

.. code-block:: zephir

	//Creating a hash with string keys
	let elements = ["foo": "bar", "bar": "foo"];

	//Creating a hash with numeric keys
	let elements = [4: "bar", 8: "foo"];

	//Creating a hash with mixed string and numeric keys
	let elements = [4: "bar", "foo": 8];

Обновление массивов
-------------------
Массивы обновляются так же, как в PHP, используя квадратные скобки:

.. code-block:: zephir

	//Updating an array with a string key
	let elements["foo"] = "bar";

	//Updating an array with a numeric key
	let elements[0] = "bar";

	//Updating multi-dimensional array
	let elements[0]["foo"] = "bar";
	let elements["foo"][0] = "bar";

Добавление элементов
--------------------
Элементы могут быть добавлены в конце массива следующим образом:

.. code-block:: zephir

	//Append an element to the array
	let elements[] = "bar";

Чтение элементов из массивов
----------------------------
Можно прочитать элементы массива следующим образом:

.. code-block:: zephir

	//Getting an element using the string key "foo"
	let foo = elements["foo"];

	//Getting an element using the numeric key 0
	let foo = elements[0];

.. _arrays: http://www.php.net/manual/en/language.types.array.php
.. _`hash table`: http://en.wikipedia.org/wiki/Hash_table
