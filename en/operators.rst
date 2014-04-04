Operators
=========
Zephir's set of operators are similar to the ones PHP and also inherits some of their behaviors.

Arithmetic Operators
--------------------
The following operators are supported:

+-------------------+-----------------------------------------------------+
| Operation         | Example                                             |
+-------------------+-----------------------------------------------------+
| Negation          | -a                                                  |
+-------------------+-----------------------------------------------------+
| Addition          | a + b                                               |
+-------------------+-----------------------------------------------------+
| Substraction      | a - b                                               |
+-------------------+-----------------------------------------------------+
| Multiplication    | a * b                                               |
+-------------------+-----------------------------------------------------+
| Division          | a / b                                               |
+-------------------+-----------------------------------------------------+
| Modulus           | a % b                                               |
+-------------------+-----------------------------------------------------+

Comparison Operators
--------------------
Comparison operators depend on the type of variables compared, for example, if both
compared operands are dynamic variables the behavior is the same as in PHP:

+----------+--------------------------+------------------------------------------------------------------+
| a == b   | Equal                    | TRUE if a is equal to b after type juggling.                     |
+----------+--------------------------+------------------------------------------------------------------+
| a === b  | Identical                | TRUE if a is equal to b, and they are of the same type.          |
+----------+--------------------------+------------------------------------------------------------------+
| a != b   | Not equal                | TRUE if a is not equal to b after type juggling.                 |
+----------+--------------------------+------------------------------------------------------------------+
| a <> b   | Not equal                | TRUE if a is not equal to b after type juggling.                 |
+----------+--------------------------+------------------------------------------------------------------+
| a !== b  | Not identical            | TRUE if a is not equal to b, or they are not of the same type.   |
+----------+--------------------------+------------------------------------------------------------------+
| a < b    | Less than                | TRUE if a is strictly less than b.                               |
+----------+--------------------------+------------------------------------------------------------------+
| a > b    | Greater than             | TRUE if a is strictly greater than b.                            |
+----------+--------------------------+------------------------------------------------------------------+
| a <= b   | Less than or equal to    | TRUE if a is less than or equal to b.                            |
+----------+--------------------------+------------------------------------------------------------------+
| a >= b   | Greater than or equal to | TRUE if a is greater than or equal to b.                         |
+----------+--------------------------+------------------------------------------------------------------+

Example:

.. code-block:: zephir

    if a == b {
        return 0;
    } else {
        if a < b {
            return -1;
        } else {
            return 1;
        }
    }

Logical Operators
-----------------
The following operators are supported:

+-------------------+-----------------------------------------------------+
| Operation         | Example                                             |
+-------------------+-----------------------------------------------------+
| And               | a && b                                              |
+-------------------+-----------------------------------------------------+
| Or                | a || b                                              |
+-------------------+-----------------------------------------------------+
| Not               | !a                                                  |
+-------------------+-----------------------------------------------------+

Example:

.. code-block:: zephir

    if a && b || !c {
        return -1;
    }
    return 1;

Bitwise Operators
-----------------
The following operators are supported:

+---------------------+------------------------------------------------------+
| Operation           | Example                                              |
+---------------------+------------------------------------------------------+
| And                 | a & b                                                |
+---------------------+------------------------------------------------------+
| Or (inclusive or)   | a | b                                                |
+---------------------+------------------------------------------------------+
| Xor (exclusive or)  | a ^ b                                                |
+---------------------+------------------------------------------------------+
| Not                 | ~a                                                   |
+---------------------+------------------------------------------------------+
| Shift left          | a << b                                               |
+---------------------+------------------------------------------------------+
| Shift right         | a >> b                                               |
+---------------------+------------------------------------------------------+

Example:

.. code-block:: zephir

    if a & SOME_FLAG {
        echo "has some flag";
    }

Learn more about comparison of dynamic variables in the `php manual`_.

Ternary Operator
----------------
Zephir supports the ternary operator available in C or PHP:

.. code-block:: zephir

    let b = a == 1 ? "x" : "y"; // b is assigned with "x" if a is equals to 1 otherwise "y" is assigned as value

Special Operators
-----------------
The following operators are supported:

Empty
^^^^^
This operator allows to check whether an expression is empty. 'Empty' means the expression is null, is an empty string or an empty array:

.. code-block:: zephir

    let someVar = "";
    if empty someVar {
        echo "is empty!";
    }

    let someVar = "hello";
    if !empty someVar {
        echo "is not empty!";
    }

Isset
^^^^^
This operator checks whether a property or index has been defined in an array or object:

.. code-block:: zephir

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // check if the array has an index "b"
        echo "yes, it has an index 'b'\n";
    }

Using 'isset' as return expression:

.. code-block:: zephir

    return isset this->{someProperty};

Note that 'isset' in Zephir works more like PHP's function array_key_exists_, 'isset' in Zephir returns true even if the array index or property is null.

Fetch
^^^^^
'Fetch' is an operator that reduce a common operation in PHP into a single instruction:

.. code-block:: php

    <?php

    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }

In Zephir, you can write the same code as:

.. code-block:: zephir

    if fetch value, myArray[key] {
        echo value;
    }

'Fetch' only returns true if the 'key' is a valid item in the array, only in that case, 'value' is populated.

Typeof
^^^^^^
This operator check variable type.
'Typeof' could works like comparison operator:

.. code-block:: zephir

    if (typeof str == "string") { // or !=
        echo str;
    }

Also typeof can works like PHP function 'gettype'.

.. code-block:: zephir

    return typeof str;
    
**Be careful**, if you want to check is object 'callable' you allways have to use 'typeof' as comparison operator.

Type Hints
^^^^^^^^^^
Zephir always tries to check whether an object implements methods and properties called/accessed on a variable that is inferred to be an object:

.. code-block:: zephir

    let o = new MyObject();

    // Zephir checks if "myMethod" is implemented on MyObject
    o->myMethod();

However, due to the dynamism inherited from PHP, sometimes it is not easy to know the class of an object so Zephir can not produce errors reports effectively.
A type hint tells the compiler which class is related to a dynamic variable allowing the compiler to perform more compilation checks:

.. code-block:: zephir

    // Tell the compiler that "o"
    // is an instance of class MyClass
    let o = <MyClass> this->_myObject;
    o->myMethod();

Branch Prediction Hints
^^^^^^^^^^^^^^^^^^^^^^^
What is branch prediction? Check this `article out`_. In environments where performance is very important, it may be useful to introduce these hints.

Consider the following example:

.. code-block:: zephir

    let allPaths = [];
    for path in this->_paths {
        if path->isAllowed() == false {
            throw new App\Exception("error!!");
        } else {
            let allPaths[] = path;
        }
    }

The authors of the above code, know in advance that the condition that throws the exception is unlikely to happen. This means that 99.9% of the time, our method executes that condition, but it is probably never evaluated as true. For the processor, this could be hard to know, so we could introduce a hint there:

.. code-block:: zephir

    let allPaths = [];
    for path in this->_paths {
        if unlikely path->isAllowed() == false {
            throw new App\Exception("error!!");
        } else {
            let allPaths[] = path;
        }
    }

.. _`array_key_exists`: http://www.php.net/manual/en/function.array-key-exists.php
.. _`php manual`: http://www.php.net/manual/en/language.operators.comparison.php
.. _`article out`: http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/
