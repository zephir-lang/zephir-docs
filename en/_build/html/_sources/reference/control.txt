Control Structures
------------------
Zephir implements a simplified set of control structures present in similar languages like C, PHP etc.

Conditionals
------------
'if' statements evaluates an expression executing this trace if the evaluation is true.
Braces are compulsory, an 'if' can have an optional 'else' clause, and multiple 'if'/'else'
constructs can be chained together:

.. code-block:: javascript

	if false {
	    echo "false?";
	} else if true {
	    echo "true!";
	} else {
	    echo "neither true nor false";
	}

Parentheses in the evaluated expression are optional:

.. code-block:: javascript

	if a < 0 { return -1; } else if a > 0 { return 1; }

Loops
-----
