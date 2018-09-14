基本语法
============
这一章中我们讨论一下如何组织文件、命名空间、变量定义、语法相关的东西还有一些别的概念等。

代码在文件与命名空间中的组织
---------------------------------------
PHP中，代码可以放在任何文件任何地方没有特定的要求。Zephir中每个文件中只能有一个类每个类也只能在一个文件中。
每个类必须一个命名空间且文件夹与命名空间等要对应。

如下面的例子，文件夹与命名空间等是相照应的:

.. code-block:: sh

	mylibrary/
		router/
			exception.zep # MyLibrary\Router\Exception
		router.zep # MyLibrary\Router

mylibrary/router.zep中的类:

.. code-block:: zephir

	namespace MyLibrary;

	class Router
	{

	}

mylibrary/router/exception.zep中的类:

.. code-block:: zephir

	namespace MyLibrary\Router;

	class Exception extends \Exception
	{

	}

Zephir中如果出现类所在位置不对等情况时则会在编译时出现异常。

指令分割
----------------------
在上面的例子中我们注意到Zephir代码中的分号。Zephir中我们可以像Java、C/C++中一样使用分号来区分语句。

.. code-block:: zephir

	myObject->myMethod(1, 2, 3); echo "world";

注释
--------
Zephir支持与C/C++一样的注释，单选注释使用//，多行注释使用/* ... \*/:

.. code-block:: c

	// 单行注释

	/**
	 * 多行注释（更确切的说是多行注释中的文档注释）
	 */

多数语言中，注释会被编译器或解释器直接忽略。在Zephir中，多行注释（文档注释）也可用来做文档块标记，他们被用来生成代码，
所以他们是语言的一部分。

如果文档注释所在的位置不对编译器会抛出一个异常。

变量声明
---------------------
Zephir中变量是需要定义的。这样做给编译器带来了的好处是可以进行代码优化与验证等。变量不能重名且不能是保留字。

.. code-block:: zephir

	//在一行指令中声明同种类型的变量
	var a, b, c;

	//每行声明一个变量
	var a;
	var b;
	var c;

变量在声明时可以赋初值。

.. code-block:: zephir

	//声明变量时赋初值
	var a = "hello", b = 0, c = 1.0;
	int d = 50; bool some = true;

变量名是大小写敏感的，下面是不同的变量:

.. code-block:: zephir

	//不同的变量
	var somevalue, someValue, SomeValue;

变量的作用域
--------------
方法中定义的变量都是局部变量。

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

超全局变量
-------------
Zephir不支持全局变量，访问PHP代码中开发者定义的全局变量是不允许的。但可以访问PHP定义的超全局变量如:

.. code-block:: zephir

	//从_POST中取值
	let price = _POST["price"];

	//从_SERVER中取值
	let requestMethod = _SERVER["REQUEST_METHOD"];

本地符号表
------------------
每个PHP中的方法或上下文都有一个符号表，这个符号表中我们可以动态的修改变量:

.. code-block:: php

	<?php

	$b = 100;
	$a = "b";
	echo $$a; // 打印输出 100

Zephir不支持这个特性，因为Zephir代码被编译成了更底层的变量，这样就没办法知道指定的上下文中是存在某个变量。如果我们想在
PHP的符号表中创建一个变量可以使用如下的语法（这里可能有bug我测试了几个结果出错）:

.. code-block:: zephir

	//Set variable $name in PHP
	let {"name"} = "hello";

	//Set variable $price in PHP
	let name = "price";
	let {name} = 10.2;
