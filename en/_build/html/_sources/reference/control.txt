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

For Loop
^^^^^^^^
A 'for' is a control structure that allows to traverse arrays or strings:

.. code-block:: javascript

    for item in ['a', 'b', 'c', 'd'] {
        echo item, "\n";
    }

Keys in hashes can be obtained in the following way:

.. code-block:: javascript

    let items = ['a': 1, 'b': 2, 'c': 3, 'd': 4];

    for key, value in items {
        echo key, ' ', value, "\n";
    }

A 'for' loop can also be instructed to traverse an array or string in reverse order:

.. code-block:: javascript

    let items = [1, 2, 3, 4, 5];

    for value in reverse items {
        echo value, "\n";
    }

A 'for' can be used to traverse string variables:

.. code-block:: javascript

    string language = "zephir", char ch;

    for ch in language {
        echo "[", ch "]";
    }

In reverse order:

.. code-block:: javascript

    string language = "zephir", char ch;

    for ch in reverse language {
        echo "[", ch "]";
    }

A standard 'for' that traverses a range of integer values can be written as follows:

.. code-block:: javascript

    for i in range(1, 10) {
        echo i, "\n";
    }

