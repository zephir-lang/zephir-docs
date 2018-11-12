# 类型

Zephir是动态类型和静态类型的。 在本章中，我们将重点介绍支持的类型及其行为。

<a name='dynamic-types'></a>

## 动态类型

动态变量与PHP中的变量完全相同。 它们可以不受限制地分配和重新分配到不同的类型。

动态变量必须声明为关键字`var`。 其行为与PHP中几乎相同:

    var a, b, c;
    

##### 初始化变量

    let a = "hello", b = false;
    

##### 改变变量

    let a = "hello", b = false;
    let a = 10, b = "140";
    

##### 做一些处理

    let c = a + b;
    

它们有八种类型

| 类型               | 说明                                                                         |
| ---------------- | -------------------------------------------------------------------------- |
| `array`          | 数组是有序映射。 映射是将值与键关联的类型。                                                     |
| `boolean`        | 布尔值表示真值。 It can be either `true` or `false`.                               |
| `float`/`double` | 浮点数 浮点的大小依赖于平台。                                                            |
| `integer`        | 整数 整数的大小与平台有关。                                                             |
| `null`           | 特殊的空值表示一个没有值的变量。                                                           |
| `object`         | 对象抽象和PHP的类似。                                                               |
| `resource`       | 资源，对外部资源的引用。                                                               |
| `string`         | A string is series of characters, where a character is the same as a byte. |

查看[PHP手册](http://www.php.net/manual/en/language.types.php)中关于这些类型的更多信息。

<a name='dynamic-types-arrays'></a>

### Array

Zephir中的数组实现与PHP中的基本相同: 为几种不同用途优化的有序映射; 它可以被视为数组、列表(向量)、哈希表(映射的实现)、字典、集合、堆栈、队列，甚至可能更多。 由于数组值可以是其他数组，树和多维数组也是可能的。

定义数组的语法与PHP略有不同:

##### 必须使用方形大括号来定义数组

    let myArray = [1, 2, 3];
    

##### 必须使用双冒号来定义哈希键

    let myHash = ["first": 1, "second": 2, "third": 3];
    

只能使用长值和字符串值作为键:

    let myHash = [0: "first", 1: true, 2: null];
    let myHash = ["first": 7.0, "second": "some string", "third": false];
    

<a name='dynamic-types-boolean'></a>

### Boolean

布尔值表示真值。 It can be either `true` or `false`:

    var a = false, b = true;
    

<a name='dynamic-types-float-double'></a>

### Float/Double

Floating-point numbers (also known as "floats", "doubles", or "real numbers"). Floating-point literals are expressions with one or more digits, followed by a period (.), followed by one or more digits. The size of a float is platform-dependent, although a maximum of ~1.8e308 with a precision of roughly 14 decimal digits is a common value (the 64 bit IEEE format).

    var number = 5.0, b = 0.014;
    

Floating point numbers have limited precision. Although it depends on the system, Zephir uses the same IEEE 754 double precision format used by PHP, which will give a maximum relative error due to rounding in the order of 1.11e-16.

<a name='dynamic-types-integer'></a>

### Integer

整数 The size of an integer is platform-dependent, although a maximum value of about two billion is the usual value (that's 32 bits signed). 64-bit platforms usually have a maximum value of about 9E18. PHP does not support unsigned integers so Zephir has this restriction too:

    var a = 5, b = 10050;
    

<a name='dynamic-types-integer-overflow'></a>

### Integer overflow

Contrary to PHP, Zephir does not automatically check for integer overflows. Like in C, if you are doing operations that may return a big number, you should use types such as `unsigned long` or `float` to store them:

    unsigned long my_number = 2147483648;
    

<a name='dynamic-types-objects'></a>

### Object

Zephir allows to instantiate, manipulate, call methods, read class constants, etc from PHP objects:

    let myObject = new \stdClass(),
        myObject->someProperty = "my value";
    

<a name='dynamic-types-string'></a>

### String

A `string` is series of characters, where a character is the same as a byte. As PHP, Zephir only supports a 256-character set, and hence does not offer native Unicode support.

    var today = "friday";
    

In Zephir, string literals can only be specified using double quotes (like in C or Go). Single quotes are reserved for `char` data type.

The following escape sequences are supported in strings:

| Sequence | 说明              |
| -------- | --------------- |
| `\t`    | Horizontal tab  |
| `\n`    | Line feed       |
| `\r`    | Carriage return |
| `\`   | Backslash       |
| `\"`    | double-quote    |

    var today    = "\tfriday\n\r",
        tomorrow = "\tsaturday";
    

In Zephir, strings don't support variable parsing like in PHP; you need to use concatenation instead:

    var name = "peter";
    
    echo "hello: " . name;
    

<a name='static-types'></a>

## Static Types

Static typing allows the developer to declare and use some variable types available in C. Variables can't change their type once they're declared as static types. However, they allow the compiler to do a better optimization job. The following types are supported:

| 类型                 | 说明                                                                             |
| ------------------ | ------------------------------------------------------------------------------ |
| `array`            | A structure that can be used as hash, map, dictionary, collection, stack, etc. |
| `boolean`          | 布尔值表示真值。 It can be either `true` or `false`.                                   |
| `char`             | Smallest addressable unit of the machine that can contain basic character set. |
| `float`/`double`   | Double precision floating-point type. The size is platform-dependent.          |
| `integer`          | Signed integers. At least 16 bits in size.                                     |
| `long`             | Long signed integer type. At least 32 bits in size.                            |
| `string`           | A string is a series of characters, where a character is the same as a byte.   |
| `unsigned char`    | Same size as `char`, but guaranteed to be unsigned.                            |
| `unsigned integer` | Unsigned integers. At least 16 bits in size.                                   |
| `unsigned long`    | Same as `long`, but unsigned.                                                  |

<a name='static-types-boolean'></a>

### Boolean

A `boolean` expresses a truth value. It can be either `true` or `false`. Contrary to the dynamic behavior detailed above, static `boolean` types remain `boolean` (`true` or `false`) no mater what value is assigned to them:

    boolean a;
    let a = true;
    

##### automatically casted to `true`

    let a = 100;
    

##### automatically casted to `false`

    let a = 0;
    

##### throws a compiler exception

    let a = "hello";
    

<a name='static-types-char-unsigned'></a>

### Char/Unsigned Char

`char` variables are the smallest addressable unit of the machine that can contain the basic character set (generally 8 bits). A `char` variable can be used to store any character in a string:

    char ch, string name = "peter";
    

##### stores 't'

    let ch = name[2];
    

##### `char` literals must be enclosed in single quotes

    let ch = 'Z';
    

<a name='static-types-integer-unsigned'></a>

### Integer/Unsigned Integer

`integer` values are like the `integer` member in dynamic values. Values assigned to integer variables remain integer:

    int a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

`unsigned integer` variables are like `integer` but they don't have sign, this means you can't store negative numbers in these sort of variables:

    uint a;
    
    let a = 50;
    

##### automatically casted to 70

    let a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

`unsigned integer` variables are twice bigger than standard `integer`. Assigning `unsigned integer` to standard (signed) `integer` may result in loss of data:

##### potential loss of data for `b`

    uint a, int b;
    
    let a = 2147483648,
        b = a;
    

<a name='static-types-long-unsigned'></a>

### Long/Unsigned Long

`long` variables are twice bigger than `integer` variables, thus they can store bigger numbers. As with `integer`, values assigned to `long` variables are automatically casted to this type:

    long a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

`unsigned long` are like `long` but they are not signed, this means you can't store negative numbers in these sort of variables:

    ulong a;
    
    let a = 50;
    

##### automatically casted to 70

    let  a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### throws a compiler exception

    let a = "hello";
    

`unsigned long` variables are twice bigger than standard `long`; assigning `unsigned long` to standard (signed) `long` may result in loss of data:

##### potential loss of data for `b`

    ulong a, long b;
    
    let a = 4294967296,
        b = a;
    

<a name='static-types-string'></a>

### String

A string is series of characters, where a character is the same as a byte. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

When a variable is declared `string` it never changes its type:

    string a;
    
    let a = "";
    

##### string literals must be enclosed in double quotes

    let  a = "hello";
    

##### converted to string "A"

    let a = 'A';
    

##### automatically casted to ""

    let a = null;