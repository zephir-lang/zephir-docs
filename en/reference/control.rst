Control Structures
==================
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
'while' denotes a loop that iterates as long as its given condition evaluates as true:

.. code-block:: javascript

	let counter = 5;
	while counter {
	    let counter -= 1;
	}

In addition to 'while', 'loop' can be used to create infinite loops:

.. code-block:: javascript

	let n = 40;
	loop {
	    let n -= 2;
	    if n % 5 == 0 { break; }
	    echo x, "\n";
	}
