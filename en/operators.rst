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

Learn more about comparison of dynamic variables in the `php manual`_.

Special Operators
-----------------
The following operators are supported:

Empty
^^^^^
This operators check if an expression is empty. 'Empty' means the expression is null, an empty string or an empty array:

.. code-block:: javascript

    let someVar = "";
    if empty someVar {
        echo "is empty!";
    } 

    let someVar = "hello";
    if !emptyVar someVar {
        echo "is not empty!";
    } 

Isset
^^^^^
This checks if a property or index is defined in an array or object:

.. code-block:: javascript

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // check if the array has an index "b"
        echo "yes, it has an index 'b'\n";
    }

Using 'isset' as return expression:

.. code-block:: javascript

    return isset this->{someProperty};  

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

.. code-block:: javascript

    if fetch value, myArray[key] {
        echo value;
    }

'Fetch' only returns true if the 'key' is a valid item in the array, only in that case, 'value' is populated. 

Type Hints
^^^^^^^^^^
Zephir always tries to check whether an object implements methods and properties called/accessed on a variable that is inferred to be an object:

.. code-block:: javascript

    let o = new MyObject();

    // Zephir checks if "myMethod" is implemented on MyObject
    o->myMethod(); 

However, due to the dynamism inherited from PHP, sometimes it is not easy to know the class of an object so Zephir can not produce errors reports effectively. 
A “type hint” tells the compiler which class is related to a dynamic variable allowing the compiler to perform more compilation checks:

.. code-block:: javascript

    // Tell the compiler that "o"
    // is an instance of class MyClass
    let o = <MyClass> this->_myObject; 
    o->myMethod();    

.. _`php manual`: http://www.php.net/manual/en/language.operators.comparison.php
