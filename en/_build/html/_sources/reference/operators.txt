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
| a === b  | Identical	              | TRUE if a is equal to b, and they are of the same type.          |
+----------+--------------------------+------------------------------------------------------------------+
| a != b   | Not equal	              | TRUE if a is not equal to b after type juggling.                 |
+----------+--------------------------+------------------------------------------------------------------+
| a <> b   | Not equal	              | TRUE if a is not equal to b after type juggling.                 |
+----------+--------------------------+------------------------------------------------------------------+
| a !== b  | Not identical            | TRUE if a is not equal to b, or they are not of the same type.   |
+----------+--------------------------+------------------------------------------------------------------+
| a < b	   | Less than                | TRUE if a is strictly less than b.                               |
+----------+--------------------------+------------------------------------------------------------------+
| a > b	   | Greater than             | TRUE if a is strictly greater than b.                            |
+----------+--------------------------+------------------------------------------------------------------+
| a <= b   | Less than or equal to    | TRUE if a is less than or equal to b.                            |
+----------+--------------------------+------------------------------------------------------------------+
| a >= b   | Greater than or equal to | TRUE if a is greater than or equal to b.                         |
+----------+--------------------------+------------------------------------------------------------------+

Learn more about comparison of dynamic variables in the `php manual`_.

.. _`php manual`: http://www.php.net/manual/en/language.operators.comparison.php
