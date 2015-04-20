Static Analysis
===============
Zephir's compiler provides static analysis of the compiled code.
The idea behind this feature is to help the developer to find potential problems and
avoid unexpected behaviors.

Conditional Unassigned Variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Static Analysis of assignments tries to identify if a variable is used before it's assigned:

.. code-block:: zephir

	class Utils
	{
 		public function someMethod(b)
 		{
   			string a; char c;

			if b == 10 {
				let a = "hello";
			}

			//a could be unitialized here
			for c in a {
				echo c, PHP_EOL;
			}
		}
	}

The above example illustrates a common situation. The variable “a” is assigned only when “b”
is equal to 10, then it’s required to use the value of this variable but it could be uninitialized.
Zephir detects this and automatically initializes the variable to an empty string and generates
a warning alerting the developer:

.. code-block:: html

	Warning: Variable 'a' was assigned for the first time in conditional branch,
 	consider initialize it in its declaration in
	/home/scott/test/test/utils.zep on 21 [conditional-initialization]

		for c in a {

Finding such errors is sometimes tricky, however static analysis helps the programmer
to find bugs in advance.

Dead Code Elimination
^^^^^^^^^^^^^^^^^^^^^
Zephir informs the developer about unreacheable branches in the code and performs
dead code elimination which means eliminate all those code from the generated binary as
this cannot be executed:

.. code-block:: zephir

	class Utils
	{
 		public function someMethod(b)
 		{
   			if false {
				// This is never executed
				echo "hello";
			}
		}
	}
