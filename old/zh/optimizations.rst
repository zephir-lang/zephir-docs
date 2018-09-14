Optimizations
优化
=============
因为Zephir语言相对C语言来讲较高级一些，所以C编译器可能无法对其代码进行足够的优化。

由于Zehpir语言是静态编译的，因此有对代码在编译时进行优化的可能，通过这些优化可以减少执行时间与内存占用。

我们通过在编译时传递以-f开头的编译优化参数进行优化。

.. code-block:: bash

    zephir -fstatic-type-inference -flocal-context-pass

在编译时可以传递-fno-来关闭一些警告信息:

.. code-block:: bash

    zephir -fno-static-type-inference -fno-call-gatherer-pass

Zephir的编译器支持下列优化参数:

static-type-inference
^^^^^^^^^^^^^^^^^^^^^
这个编译参数非常重要，因为他可以让编译器查找动态变量以尽可能的使其编译为静态或基本变量（如int等）这样可以提升应用的性能。

下面的代码使用一些动态变量来进行数学计算:

.. code-block:: zephir

	public function someCalculations(var a, var b)
	{
		var i = 0, t = 1;

		while i < 100 {
			if i % 3 == 0 {
				continue;
			}
			let t += (a - i), i++;
		}

		return i + b;
	}

变量a,b,i在其中完成了数学计算，这些变量可以被Zephir转换成基本变量，这会带来性能上提升。转换过后的代码如下:

.. code-block:: zephir

	public function someCalculations(int a, int b)
	{
		int i = 0, t = 1;

		while i < 100 {
			if i % 3 == 0 {
				continue;
			}
			let t += (a - i), i++;
		}

		return i + b;
	}

如果禁用了这个扩展，所有的变量会保持原样不变，编译器不会做出如上优化。

static-type-inference-second-pass
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
这个优化选项会打开第二类型推断途经，由于其会收集基于第一静态推断途经的数据，所以会加大编译的工作量。

local-context-pass
^^^^^^^^^^^^^^^^^^
这个编译参数会把堆中的数据移到栈中。这样通过减少内存的间接访问从而提升性能。

constant-folding
^^^^^^^^^^^^^^^^
常量合并即是在编译时对常量表达式进行合并。下面是打开这个优化选择时的例子:

.. code-block:: zephir

	public function getValue()
	{
		return (86400 * 30) / 12;
	}

上述代码会被转换成:

.. code-block:: zephir

	public function getValue()
	{
		return 216000;
	}

static-constant-class-folding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
这个优化选项可以把类的常量替换成其代码的值:

.. code-block:: zephir

	class MyClass
	{

		const SOME_CONSTANT = 100;

		public function getValue()
		{
			return self::SOME_CONSTANT;
		}
	}

上述代码被转换为:

.. code-block:: zephir

	class MyClass
	{

		const SOME_CONSTANT = 100;

		public function getValue()
		{
			return 100;
		}
	}

call-gatherer-pass
^^^^^^^^^^^^^^^^^^
这个参数打开后编译器会计算一个函数或方法在被调用方法中被调用的次数。
这个优化选项可以让编译器引入内联缓存来避免方法或函数的查找从而提升性能。

优化后:

.. code-block:: zephir

	class MyClass extends OtherClass
	{

		public function getValue()
		{
			this->someMethod();
            this->someMethod(); // 这个方法调用会执行的比较快
		}
	}
