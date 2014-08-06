Control Structures
==================
Zephir implements a simplified set of control structures present in similar languages like C, PHP etc.

Conditionals
------------

If Statement
^^^^^^^^^^^^
'if' statements evaluates an expression executing this trace if the evaluation is true.
Braces are compulsory, an 'if' can have an optional 'else' clause, multiple 'if'/'else'
constructs can be chained together:

.. code-block:: zephir

    if false {
        echo "false?";
    } else {
        if true {
            echo "true!";
        } else {
            echo "neither true nor false";
        }
    }

Parentheses in the evaluated expression are optional:

.. code-block:: zephir

    if a < 0 { return -1; } else { if a > 0 { return 1; } }

Switch Statement
^^^^^^^^^^^^^^^^
A 'switch' evalutes an expression against a series of predefined literal values executing the corresponding
'case' block or falling back to the 'default' block case:

.. code-block:: zephir

    switch count(items) {
        case 1:
        case 3:
            echo "odd items";
            break;
        case 2:
        case 4:
            echo "even items";
            break;
        default:
            echo "unknown items";
    }

Loops
-----

While Statement
^^^^^^^^^^^^^^^
'while' denotes a loop that iterates as long as its given condition evaluates as true:

.. code-block:: zephir

    let counter = 5;
    while counter {
        let counter -= 1;
    }

Loop Statement
^^^^^^^^^^^^^^
In addition to 'while', 'loop' can be used to create infinite loops:

.. code-block:: zephir

    let n = 40;
    loop {
        let n -= 2;
        if n % 5 == 0 { break; }
        echo x, "\n";
    }

For Statement
^^^^^^^^^^^^^
A 'for' is a control structure that allows to traverse arrays or strings:

.. code-block:: zephir

    for item in ["a", "b", "c", "d"] {
        echo item, "\n";
    }

Keys in hashes can be obtained in the following way:

.. code-block:: zephir

    let items = ["a": 1, "b": 2, "c": 3, "d": 4];

    for key, value in items {
        echo key, " ", value, "\n";
    }

A 'for' loop can also be instructed to traverse an array or string in reverse order:

.. code-block:: zephir

    let items = [1, 2, 3, 4, 5];

    for value in reverse items {
        echo value, "\n";
    }

A 'for' can be used to traverse string variables:

.. code-block:: zephir

    string language = "zephir"; char ch;

    for ch in language {
        echo "[", ch ,"]";
    }

In reverse order:

.. code-block:: zephir

    string language = "zephir"; char ch;

    for ch in reverse language {
        echo "[", ch ,"]";
    }

A standard 'for' that traverses a range of integer values can be written as follows:

.. code-block:: zephir

    for i in range(1, 10) {
        echo i, "\n";
    }

Break Statement
^^^^^^^^^^^^^^^
'break' ends execution of the current 'while', 'for' or 'loop' statements:

.. code-block:: zephir

    for item in ["a", "b", "c", "d"] {
        if item == "c" {
            break; // exit the for
        }
        echo item, "\n";
    }

Continue Statement
^^^^^^^^^^^^^^^^^^
'continue' is used within looping structures to skip the rest of the current loop iteration and
continue execution at the condition evaluation and then the beginning of the next iteration.

.. code-block:: zephir

    let a = 5;
    while a > 0 {
        let a--;
        if a == 3 {
            continue;
        }
        echo a, "\n";
    }

Require
-------
The 'require' statement dynamically includes and evaluates a specified PHP file. Note that files
included via Zephir are interpreted by Zend Engine as normal PHP files. 'require' does not allow to
include other zephir files in runtime.

.. code-block:: zephir

    if file_exists(path) {
        require path;
    }

Let
---
'Let' statement is used to mutate variables, properties and arrays. Variables are by default inmutable and this instruction makes them mutable:

.. code-block:: zephir

    let name = "Tony";           // simple variable
    let this->name = "Tony";     // object property
    let data["name"] = "Tony";   // array index
    let self::_name = "Tony";    // static property

Also this instruction must be used to increment/decrement variables:

.. code-block:: zephir

    let number++;           // increment simple variable
    let number--;           // decrement simple variable
    let this->number++;     // increment object property
    let this->number--;     // decrement object property

Multiple mutations can be performed in a single 'let' operation:

.. code-block:: zephir

    let price = 1.00, realPrice = price, status = false;


