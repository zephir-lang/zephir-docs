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

.. code-block:: bash

	public function math(var a, var b)
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

.. code-block:: bash

	public function math(int a, int b)
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

static-type-inference-second-pass
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This enables a second type inference pass which improves the work done by the first static type inference pass.

 'optimizations' => array(
            'static-type-inference'             => true,
            'static-type-inference-second-pass' => true,
            'local-context-pass'                => true,
            'constant-folding'                  => true,
            'static-constant-class-folding'     => true,
            'call-gatherer-pass'                => true,
            'check-invalid-reads'               => false
        ),