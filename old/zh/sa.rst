静态分析
===============
Zephir的编译器会对编译的代码进行静态分析，这会帮助开发者找出潜在的问题并避免一些不当的行为。

因条件表达式而导致的未赋值变量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
对赋值的静态分析会检测出一个变量在使用之前是否被赋值了。

.. code-block:: zephir

	class Utils
	{
 		public function someMethod(b)
 		{
   			string a; char c;

			if b == 10 {
				let a = "hello";
			}

			//这里不能保证a会被初始化
			for c in a {
				echo c, PHP_EOL;
			}
		}
	}

上述例子展示了一种常见的场景。其中a被赋值只发生在b的值是10时，如果b不为10则a是未初始化状态。这时Zephir的编译器会检测到
这种情况然后使用一个空的字符串来初始化a并向开发者发出一个警告信息:

.. code-block:: html

	Warning: Variable 'a' was assigned for the first time in conditional branch,
 	consider initialize it in its declaration in
	/home/scott/test/test/utils.zep on 21 [conditional-initialization]

		for c in a {

有些找出这些问题真的是比较困难，这时静态分析就会预先为我们找到潜在的bug。

无用代码删除
^^^^^^^^^^^^^^^^^^^^^
当Zephir检测到无用代码时会向开发者发出警告并从生成的二进制文件中删除掉无用的代码(不会被执行的代码):

.. code-block:: zephir

	class Utils
	{
 		public function someMethod(b)
 		{
   			if false {
				// 这里的代码不会被执行
				echo "hello";
			}
		}
	}
