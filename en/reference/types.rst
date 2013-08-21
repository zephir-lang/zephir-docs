Types
=====
Zephir is both dynamic and static typed. In this chapter we highlight the supported types and
its behavior:

Dynamic Type
------------
Dynamic variables are exactly like the ones in PHP, they can be assigned and reassigned to
different types without restriction.

A dynamic variable must be declared with the keyword 'var', the behavior is nearly the same as in PHP:

.. code-block:: javascript

    var a, b, c;

    // Initialize variables
    let a = "hello", b = false;

    // Change their values
    let a = 10, b = "140";

    // Perform operations between them
    let c = a + b;

They can have eight types:

+---------------+---------------------------------------------------------------------------+
| Type          | Description                                                               |
+---------------+---------------------------------------------------------------------------+
| boolean       | A boolean expresses a truth value. It can be either 'true' or 'false'.    |
+---------------+---------------------------------------------------------------------------+
| integer       | Integer numbers. The size of an integer is platform-dependent.            |
+---------------+---------------------------------------------------------------------------+
| float/double  | Floating point numbers. The size of a float is platform-dependent.        |
+---------------+---------------------------------------------------------------------------+
| string        | A string is series of characters, where a character is the same as a byte.|
+---------------+---------------------------------------------------------------------------+
| array         | An array is an ordered map. A map is a type that associates values to keys|
+---------------+---------------------------------------------------------------------------+
| object        | Object abstraction like in PHP                                            |
+---------------+---------------------------------------------------------------------------+
| resource      | A resource holds a reference to an external resource                      |
+---------------+---------------------------------------------------------------------------+
| null          | The special NULL value represents a variable with no value                |
+---------------+---------------------------------------------------------------------------+

Check more info about these types in the `PHP manual`_

Boolean
^^^^^^^
A boolean expresses a truth value. It can be either 'true' or 'false':

.. code-block:: javascript

    var a = false, b = true;

Integer
^^^^^^^
Integer numbers. The size of an integer is platform-dependent, although a maximum value of about two
billion is the usual value (that's 32 bits signed). 64-bit platforms usually have a maximum value of about 9E18.
PHP does not support unsigned integers so Zephir has this restriction too:

.. code-block:: javascript

    var a = 5, b = 10050;

Integer overflow
^^^^^^^^^^^^^^^^
Contrary to PHP, Zephir does not automatically checks for integer overflows, like in C if you are
doing operations that may return a big number you can use types such as 'unsigned long' or 'float'
to store them:

.. code-block:: javascript

    unsigned long my_number = 2147483648;

Float/Double
^^^^^^^^^^^^
Floating point numbers (also known as "floats", "doubles", or "real numbers"). The size of a float is
platform-dependent, although a maximum of ~1.8e308 with a
precision of roughly 14 decimal digits is a common value (the 64 bit IEEE format).

.. code-block:: javascript

    var number = 5.0, b = 0.014;

Floating point numbers have limited precision. Although it depends on the system,
as PHP, Zephir uses the IEEE 754 double precision format, which will give a maximum
relative error due to rounding in the order of 1.11e-16.

String
^^^^^^
A string is series of characters, where a character is the same as a byte. As PHP, Zephir only supports
a 256-character set, and hence does not offer native Unicode support.

.. code-block:: javascript

    var today = "friday";

In Zephir, string literals can only be specified using double quotes (like in C), single quotes are reserved
for chars.

The following escape sequences are supported in strings:

+---------------+---------------------------------------------------------------------------+
| Sequence      | Description                                                               |
+---------------+---------------------------------------------------------------------------+
| \\t           | Horizontal tab                                                            |
+---------------+---------------------------------------------------------------------------+
| \\n           | Line feed                                                                 |
+---------------+---------------------------------------------------------------------------+
| \\r           | Carriage return                                                           |
+---------------+---------------------------------------------------------------------------+
| \\\\          | Backslash                                                                 |
+---------------+---------------------------------------------------------------------------+
| \\"           | double-quote                                                              |
+---------------+---------------------------------------------------------------------------+

.. code-block:: javascript

    var today = "\tfriday\n\r",
        tomorrow = "\tsaturday";

In Zephir, strings don't support variable parsing like in PHP, you can use concatenation instead:

.. code-block:: javascript

    var name = "peter";

    echo "hello: " . name;

Array
^^^^^
The array implementation in Zephir is basically the same as in PHP: Ordered maps optimized for
several different uses; it can be treated as an array, list (vector), hash table (an implementation of a map),
dictionary, collection, stack, queue, and probably more. As array values can be other arrays, trees and
multidimensional arrays are also possible.

The syntax to define arrays is slightly different than in PHP:

.. code-block:: javascript

    //Square braces must be used to define arrays
    let myArray = [1, 2, 3];

    //Double colon must be used to define hashes' keys
    let myHash = ['first': 1, 'second': 2, 'third': 3];

Only long and string values can be used as keys:

.. code-block:: javascript

    let myHash = [0: "first", 1: true, 2: null];
    let myHash = ['first': 7.0, 'second': "some string", 'third': false];

Static Types
------------
Static typing allows the developer to declare and use some variable types available in C.
Variables can't change their type once they're declared as dynamic types. However, they allow
the compiler to do a better optimization job. The following types are supported:

+------------------+---------------------------------------------------------------------------------+
| Type             | Description                                                                     |
+------------------+---------------------------------------------------------------------------------+
| boolean          | A boolean expresses a truth value. It can be either 'true' or 'false'.          |
+------------------+---------------------------------------------------------------------------------+
| integer          | Signed integers. At least 16 bits in size.                                      |
+------------------+---------------------------------------------------------------------------------+
| unsigned integer | Unsigned integers. At least 16 bits in size.                                    |
+------------------+---------------------------------------------------------------------------------+
| char             | Smallest addressable unit of the machine that can contain basic character set.  |
+------------------+---------------------------------------------------------------------------------+
| unsigned char    | Same size as char, but guaranteed to be unsigned.                               |
+------------------+---------------------------------------------------------------------------------+
| long             | Long signed integer type. At least 32 bits in size.                             |
+------------------+---------------------------------------------------------------------------------+
| unsigned long    | Same as long, but unsigned.                                                     |
+------------------+---------------------------------------------------------------------------------+
| float/double     | Double precision floating-point type. The size is platform-dependent.           |
+------------------+---------------------------------------------------------------------------------+
| string           | A string is series of characters, where a character is the same as a byte.      |
+------------------+---------------------------------------------------------------------------------+


.. _`PHP manual`: http://www.php.net/manual/en/language.types.php