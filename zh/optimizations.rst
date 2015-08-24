Optimizations
=============
Because the code in Zephir is sometimes very high-level, a C compiler might not be in the ability to optimize this code enough.

Zephir, thanks to its AOT compiler, is able to optimize the code at compile time potentially improving its execution time
or reducing the memory required by the program.

You can enable optimizations by passing its name prefixed by -f:

.. code-block:: bash

    zephir -fstatic-type-inference -flocal-context-pass

Warnings can be disabled by passing its name prefixed by -fno-:

.. code-block:: bash

    zephir -fno-static-type-inference -fno-call-gatherer-pass

The following optimizations are supported:

static-type-inference
^^^^^^^^^^^^^^^^^^^^^
This compilation pass is very important, since it looks for dynamic variables that can potentially
transformed into static/primitive types which are better optimized by the underlying compiler.

The following code use a set dynamic variables to perform some mathematical calculations:

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

Variables 'a', 'b' and 'i' are used all of them in mathematical operations and can thus be transformed
into static variables taking advantage of other compilation passes. After this pass, the compiler
automatically rewrites this code to:

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

By disabling this compilation pass, all variables will maintain the type which they were originally declared
without optimization.

static-type-inference-second-pass
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This enables a second type inference pass which improves the work done based on the data gathered by
the first static type inference pass.

local-context-pass
^^^^^^^^^^^^^^^^^^
This compilation pass moves variables that will be allocated in the heap to the stack.

constant-folding
^^^^^^^^^^^^^^^^
Constant folding is the process of simplifying constant expressions at compile time. The following
code is simplified when this optimization is enabled:

.. code-block:: zephir

	public function getValue()
	{
		return (86400 * 30) / 12;
	}

Is transformed into:

.. code-block:: zephir

	public function getValue()
	{
		return 216000;
	}

static-constant-class-folding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This optimization replaces values at class constants in compile time:

.. code-block:: zephir

	class MyClass
	{

		const SOME_CONSTANT = 100;

		public function getValue()
		{
			return self::SOME_CONSTANT;
		}

	}

Is transformed into:

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
This pass counts how many times a function or method is called within the same method.
This allow the compiler to introduce inline caches to avoid method or function lookups.

