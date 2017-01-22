数组
======
Zephir为我们提供了一种使用PHP arrays_ 的方式。
数组是由 `hash table`_ 实现的。

声明数组变量
-------------------------
数组变量中以使用关键字var或array进行声明:

.. code-block:: zephir

	var a = []; // 数组变量（动态），其类型可以修改
	array b = []; // 数组变量（静态），其类型在运行中不可修改

创建数组
---------------
数组的创建需要使用方括号包起来:

.. code-block:: zephir

	//创建一个空的数组
	let elements = [];

	//创建有初始值的数组
	let elements = [1, 3, 4];

	//创建一个有不同类型元素组成的数组
	let elements = ["first", 2, true];

	//多维数组
	let elements = [[0, 1], [4, 5], [2, 3]];

像PHP中一样Zephir数组支持hash或字典:

.. code-block:: zephir

	//使用字符串作为键创建hash
	let elements = ["foo": "bar", "bar": "foo"];

	//使用数字作为键创建hash
	let elements = [4: "bar", 8: "foo"];

	//使用字符串和数字作为键来创建hash
	let elements = [4: "bar", "foo": 8];

修改数组
---------------
我们可以像修改PHP数组一样使用方括号的形式来修改Zephir中的数组:

.. code-block:: zephir

	//修改以字符串为键的数组
	let elements["foo"] = "bar";

	//修改以数字为键的数组
	let elements[0] = "bar";

	//修改多维数组
	let elements[0]["foo"] = "bar";
	let elements["foo"][0] = "bar";

追加数组元素
------------------
可以以如下形式追加元素到数组的末尾:

.. code-block:: zephir

	//追加元素到数组的末尾
	let elements[] = "bar";

从数组中读取元素
----------------------------
以如下形式从数组中读取元素：

.. code-block:: zephir

	//读取键为foo的数组元素
	let foo = elements["foo"];

	//读取键为0的数组元素
	let foo = elements[0];

.. _arrays: http://www.php.net/manual/en/language.types.array.php
.. _`hash table`: http://en.wikipedia.org/wiki/Hash_table
