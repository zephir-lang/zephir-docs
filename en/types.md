---
layout: default
language: 'en'
version: '0.10'
menu:
  - text:
      'Dynamic Types'
    url: '#dynamic-types'
    sub:
      - text:
            'Arrays'
        url: '#dynamic-types-arrays'
      - text:
            'Boolean'
        url: '#dynamic-types-boolean'
      - text:
            'Float/Double'
        url: '#dynamic-types-float-double'
      - text:
            'Integer'
        url: '#dynamic-types-integer'
      - text:
            'Integer overflow'
        url: '#dynamic-types-integer-overflow'
      - text:
            'Objects'
        url: '#dynamic-types-objects'
      - text:
            'String'
        url: '#dynamic-types-string'
  - text:
      'Static Types'
    url: '#static-types'
    sub:
      - text:
            'Boolean'
        url: '#static-types-boolean'
      - text:
            'Char/Unsigned Char'
        url: '#static-types-char-unsigned'
      - text:
            'Integer/Unsigned Integer'
        url: '#static-types-integer-unsigned'
      - text:
            'Long/Unsigned Long'
        url: '#static-types-long-unsigned'
      - text:
            'String'
        url: '#static-types-string'
---
# Types
Zephir is both dynamically and statically typed. In this chapter we highlight the supported types and their behaviors.

<a name='dynamic-types'></a>
## Dynamic Types
Dynamic variables are exactly like the ones in PHP. They can be assigned and reassigned to different types without restriction.

A dynamic variable must be declared with the keyword `var`. The behavior is nearly the same as in PHP:

```zephir
var a, b, c;
```
    
##### Initialize variables

```zephir
let a = "hello", b = false;
```
    
##### Change their values
    
```zephir
let a = "hello", b = false;
let a = 10, b = "140";
```
    
##### Perform operations
    
```zephir
let c = a + b;
```

They can have eight types:

| Type              | Description                                                                  |
|-------------------|------------------------------------------------------------------------------|
| `array`           | An array is an ordered map. A map is a type that associates values to keys.  |
| `boolean`         | A boolean expresses a truth value. It can be either `true` or `false`.       |
| `float`/`double`  | Floating point numbers. The size of a float is platform-dependent.           |
| `integer`         | Integer numbers. The size of an integer is platform-dependent.               |
| `null`            | The special NULL value represents a variable with no value.                  |
| `object`          | Object abstraction like in PHP.                                              |
| `resource`        | A resource holds a reference to an external resource.                        |
| `string`          | A string is series of characters, where a character is the same as a byte.   |

Check more info about these types in the [PHP manual](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>
### Array
The array implementation in Zephir is basically the same as in PHP: ordered maps optimized for several different uses; it can be treated as an array, list (vector), hash table (an implementation of a map), dictionary, collection, stack, queue, and probably more. As array values can be other arrays, trees and multidimensional arrays are also possible.

The syntax to define arrays is slightly different than in PHP:

##### Square braces must be used to define arrays

```zephir
let myArray = [1, 2, 3];
```
    
##### Double colon must be used to define hashes' keys

```zephir
let myHash = ["first": 1, "second": 2, "third": 3];
```

Only long and string values can be used as keys:

```zephir
let myHash = [0: "first", 1: true, 2: null];
let myHash = ["first": 7.0, "second": "some string", "third": false];
```

<a name='dynamic-types-boolean'></a>
### Boolean
A boolean expresses a truth value. It can be either `true` or `false`:

```zephir
var a = false, b = true;
```

<a name='dynamic-types-float-double'></a>
### Float/Double
Floating-point numbers (also known as "floats", "doubles", or "real numbers"). Floating-point literals are expressions with one or more digits, followed by a period (.), followed by one or more digits. The size of a float is platform-dependent, although a maximum of ~1.8e308 with a precision of roughly 14 decimal digits is a common value (the 64 bit IEEE format).

```zephir
var number = 5.0, b = 0.014;
```

Floating point numbers have limited precision. Although it depends on the system, Zephir uses the same IEEE 754 double precision format used by PHP, which will give a maximum relative error due to rounding in the order of 1.11e-16.

<a name='dynamic-types-integer'></a>
### Integer
Integer numbers. The size of an integer is platform-dependent, although a maximum value of about two billion is the usual value (that's 32 bits signed). 64-bit platforms usually have a maximum value of about 9E18. PHP does not support unsigned integers so Zephir has this restriction too:

```zephir
var a = 5, b = 10050;
```

<a name='dynamic-types-integer-overflow'></a>
### Integer overflow
Contrary to PHP, Zephir does not automatically check for integer overflows. Like in C, if you are doing operations that may return a big number, you should use types such as `unsigned long` or `float` to store them:

```zephir
unsigned long my_number = 2147483648;
```

<a name='dynamic-types-objects'></a>
### Object
Zephir allows to instantiate, manipulate, call methods, read class constants, etc from PHP objects:

```zephir
let myObject = new \stdClass(),
    myObject->someProperty = "my value";
```

<a name='dynamic-types-string'></a>
### String
A `string` is series of characters, where a character is the same as a byte. As PHP, Zephir only supports a 256-character set, and hence does not offer native Unicode support.

```zephir
var today = "friday";
```

In Zephir, string literals can only be specified using double quotes (like in C or Go). Single quotes are reserved for `char` data type.

The following escape sequences are supported in strings:

| Sequence    | Description      |
|-------------|------------------|
| `\t`        | Horizontal tab   |
| `\n`        | Line feed        |
| `\r`        | Carriage return  |
| `\\`        | Backslash        |
| `\"`        | double-quote     |

```zephir
var today    = "\tfriday\n\r",
    tomorrow = "\tsaturday";
```

In Zephir, strings don't support variable parsing like in PHP; you need to use concatenation instead:

```zephir
var name = "peter";

echo "hello: " . name;
```

<a name='static-types'></a>
## Static Types
Static typing allows the developer to declare and use some variable types available in C. Variables can't change their type once they're declared as static types. However, they allow the compiler to do a better optimization job. The following types are supported:

| Type                  | Description                                                                     |
|-----------------------|---------------------------------------------------------------------------------|
| `array`               | A structure that can be used as hash, map, dictionary, collection, stack, etc.  |
| `boolean`             | A boolean expresses a truth value. It can be either `true` or `false`.          |
| `char`                | Smallest addressable unit of the machine that can contain basic character set.  |
| `float`/`double`      | Double precision floating-point type. The size is platform-dependent.           |
| `integer`             | Signed integers. At least 16 bits in size.                                      |
| `long`                | Long signed integer type. At least 32 bits in size.                             |
| `string`              | A string is a series of characters, where a character is the same as a byte.    |
| `unsigned char`       | Same size as `char`, but guaranteed to be unsigned.                             |
| `unsigned integer`    | Unsigned integers. At least 16 bits in size.                                    |
| `unsigned long`       | Same as `long`, but unsigned.                                                   |

<a name='static-types-boolean'></a>
### Boolean
A `boolean` expresses a truth value. It can be either `true` or `false`. Contrary to the dynamic behavior detailed above, static `boolean` types remain `boolean` (`true` or `false`) no mater what value is assigned to them:

```zephir
boolean a;
let a = true;
```

##### automatically casted to `true`

```zephir
let a = 100;
```
    
##### automatically casted to `false`

```zephir
let a = 0;
```

##### throws a compiler exception
    
```zephir
let a = "hello";
```
    
<a name='static-types-char-unsigned'></a>
### Char/Unsigned Char
`char` variables are the smallest addressable unit of the machine that can contain the basic character set (generally 8 bits). A `char` variable can be used to store any character in a string:

```zephir
char ch, string name = "peter";
```
    
##### stores 't'

```zephir
let ch = name[2];
```
    
##### `char` literals must be enclosed in single quotes
    
```zephir
let ch = 'Z';
```zephir

<a name='static-types-integer-unsigned'></a>
### Integer/Unsigned Integer
`integer` values are like the `integer` member in dynamic values. Values assigned to integer variables remain integer:

```zephir
int a;

let a = 50,
    a = -70;
```

##### automatically casted to 100

```zephir
let a = 100.25;
```
    
##### automatically casted to 0

```zephir
let a = null;
```
    
##### automatically casted to 0
    
```zephir
let a = false;
```
    
##### throws a compiler exception

```zephir
let a = "hello";
```

`unsigned integer` variables are like `integer` but they don't have sign, this means you can't store negative numbers in these sort of variables:

```zephir
uint a;

let a = 50;
```
    
##### automatically casted to 70

```zephir
let a = -70;
```
    
##### automatically casted to 100

```zephir
let a = 100.25;
```
    
##### automatically casted to 0

```zephir
let a = null;
```
    
##### automatically casted to 0

```zephir
let a = false;
```

##### throws a compiler exception

```zephir
let a = "hello";
```

`unsigned integer` variables are twice bigger than standard `integer`. Assigning `unsigned integer` to standard (signed) `integer` may result in loss of data:

##### potential loss of data for `b`

```zephir
uint a, int b;

let a = 2147483648,
    b = a;
```

<a name='static-types-long-unsigned'></a>
### Long/Unsigned Long
`long` variables are twice bigger than `integer` variables, thus they can store bigger numbers. As with `integer`, values assigned to `long` variables are automatically casted to this type:

```zephir
long a;

let a = 50,
    a = -70;
```
        
##### automatically casted to 100
    
```zephir
let a = 100.25;
```
    
##### automatically casted to 0

```zephir
    let a = null;
```
    
##### automatically casted to 0
    
```zephir
let a = false;
```
    
##### throws a compiler exception

```zephir
let a = "hello";
```

`unsigned long` are like `long` but they are not signed, this means you can't store negative numbers in these sort of variables:

```zephir
ulong a;

let a = 50;
```

##### automatically casted to 70
    
```zephir
let  a = -70;
```
    
##### automatically casted to 100

```zephir
let a = 100.25;
```
    
##### automatically casted to 0

```zephir
let a = null;
```
    
##### automatically casted to 0
    
```zephir
let a = false;
```
    
##### throws a compiler exception

```zephir
let a = "hello";
```

`unsigned long` variables are twice bigger than standard `long`; assigning `unsigned long` to standard (signed) `long` may result in loss of data:

##### potential loss of data for `b`

```zephir
ulong a, long b;

let a = 4294967296,
    b = a;
```

<a name='static-types-string'></a>
### String
A string is series of characters, where a character is the same as a byte. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

When a variable is declared `string` it never changes its type:

```zephir
string a;

let a = "";
```
    
##### string literals must be enclosed in double quotes

```zephir
let  a = "hello";
```
    
##### converted to string "A"

```zephir
let a = 'A';
```
    
##### automatically casted to ""

```zephir
let a = null;
```
