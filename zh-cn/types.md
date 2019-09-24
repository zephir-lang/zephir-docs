---
layout: default
language: 'zh-cn'
version: '0.10'
---

# 类型

Zephir是动态类型和静态类型的。 在本章中，我们将重点介绍支持的类型及其行为。

<a name='dynamic-types'></a>

## 动态类型

动态变量与PHP中的变量完全相同。 它们可以不受限制地分配和重新分配到不同的类型。

动态变量必须声明为关键字`var`。 其行为与PHP中几乎相同:

```zephir
var a, b, c;
```

##### 初始化变量

```zephir
let a = "hello", b = false;
```

##### 改变变量

```zephir
let a = "hello", b = false;
let a = 10, b = "140";
```

##### 做一些处理

```zephir
let c = a + b;
```

它们有八种类型

| 类型               | 说明                           |
| ---------------- | ---------------------------- |
| `array`          | 数组是有序映射。 映射是将值与键关联的类型。       |
| `boolean`        | 布尔值表示真值。 它可以是`true`或`false`。 |
| `float`/`double` | 浮点数 浮点的大小依赖于平台。              |
| `integer`        | 整数 整数的大小与平台有关。               |
| `null`           | 特殊的空值表示一个没有值的变量。             |
| `object`         | 对象抽象和PHP的类似。                 |
| `resource`       | 资源，对外部资源的引用。                 |
| `string`         | 一个string是一系列字符，其中字符与字节相同。    |

查看[PHP手册](http://www.php.net/manual/en/language.types.php)中关于这些类型的更多信息。

<a name='dynamic-types-arrays'></a>

### Array

Zephir中的数组实现与PHP中的基本相同: 为几种不同用途优化的有序映射; 它可以被视为数组、列表(向量)、哈希表(映射的实现)、字典、集合、堆栈、队列，甚至可能更多。 由于数组值可以是其他数组，树和多维数组也是可能的。

定义数组的语法与PHP略有不同:

##### 必须使用方形大括号来定义数组

```zephir
let myArray = [1, 2, 3];
```

##### 必须使用双冒号来定义哈希键

```zephir
let myHash = ["first": 1, "second": 2, "third": 3];
```

只能使用长值和字符串值作为键:

```zephir
let myHash = [0: "first", 1: true, 2: null];
let myHash = ["first": 7.0, "second": "some string", "third": false];
```

<a name='dynamic-types-boolean'></a>

### Boolean

布尔值表示真值。 它可以是 `true`, 也可以是 `false`:

```zephir
var a = false, b = true;
```

<a name='dynamic-types-float-double'></a>

### 浮点型/双精度浮点

浮点数(也称为浮点数、双数或实数)。 浮点文字是具有一个或多个数字的表达式, 后跟句点 (.), 后跟一个或多个数字。 浮点的大小取决于平台, 但精度约为14位十进制数字的最大值为 ~ 1.8 e308 是一个通用值 (64位 IEEE 格式)。

```zephir
var number = 5.0, b = 0.014;
```

浮点数的精度有限。 虽然这取决于系统, 但 Zephir 使用 php 使用的相同 IEEE 754 双精度格式, 这将由于舍入的1.11e-16 的顺序而产生最大的相对误差。

<a name='dynamic-types-integer'></a>

### Integer

整数 一个整数的大小依赖于平台，尽管通常的值是20亿左右(32位)。 64位平台的最大值通常在9E18左右。 PHP不支持无符号整数，所以Zephir也有这个限制:

```zephir
var a = 5, b = 10050;
```

<a name='dynamic-types-integer-overflow'></a>

### Integer overflow

与PHP相反，Zephir不会自动检查整数溢出。 像在C中一样，如果你做的操作可能会返回一个大的数字，你应该使用`unsigned long`或`float`来存储它们:

```zephir
unsigned long my_number = 2147483648;
```

<a name='dynamic-types-objects'></a>

### 对象

Zephir允许从PHP对象实例化、操作、调用方法、读取类常量等:

```zephir
let myObject = new \stdClass(),
    myObject->someProperty = "my value";
```

<a name='dynamic-types-string'></a>

### String

一个`string`是一系列字符，其中字符与字节相同。 与PHP一样，Zephir只支持256个字符集，因此不提供本地Unicode支持。

```zephir
var today = "friday";
```

在Zephir中，字符串文字只能使用双引号指定(类似在C或Go中)。 单引号用于`char`数据类型。

字符串中支持下列转义序列:

| 序列     | 说明    |
| ------ | ----- |
| `\t`  | 水平制表符 |
| `\n`  | 换行    |
| `\r`  | 回车    |
| `\` | 反斜线   |
| `\"`  | 双引号   |

```zephir
var today    = "\tfriday\n\r",
    tomorrow = "\tsaturday";
```

在 Zephir中, 字符串不支持像 php 中那样的变量解析; 您需要改为使用串联:

```zephir
var name = "peter";

echo "hello: " . name;
```

<a name='static-types'></a>

## 静态类型

静态类型允许开发人员声明和使用 c. 变量中可用的某些变量类型, 一旦它们被声明为静态类型, 就不能更改它们的类型。 但是，它们允许编译器做更好的优化工作。 支持以下类型:

| 类型                 | 说明                           |
| ------------------ | ---------------------------- |
| `array`            | 可以用作散列、映射、字典、集合、堆栈等的结构。      |
| `boolean`          | 布尔值表示真值。 它可以是`true`或`false`。 |
| `char`             | 能包含基本字符集的机器的最小可寻址单元。         |
| `float`/`double`   | 双精度浮点型。 大小依赖于平台。             |
| `integer`          | 带符号的整形 至少16位的大小。             |
| `long`             | 长有符号整数类型。 至少32位。             |
| `string`           | 字符串是一系列字符，其中字符与字节相同。         |
| `unsigned char`    | 与`char`大小相同，但保证是无符号的。        |
| `unsigned integer` | 无符号整数 至少16位的大小。              |
| `unsigned long`    | 与`long`相同，但无符号。              |

<a name='static-types-boolean'></a>

### Boolean

一个`boolean`表示一个真假。 它可以是`true`或`false`。 与上面详细描述的动态行为相反，静态`布尔值`类型仍然是`布尔值` (`true`或`false`)，不管赋予它们什么值:

```zephir
boolean a;
let a = true;
```

##### 自动转换为`true`

```zephir
let a = 100;
```

##### 自动转换为`false`

```zephir
let a = 0;
```

##### 抛出编译器异常

```zephir
let a = "hello";
```

<a name='static-types-char-unsigned'></a>

### 字符/无符号字符

`char`变量是机器中能够包含基本字符集(通常是8位) 的最小可寻址单元。 `char`变量是机器中能够包含基本字符集(通常是8位) 的最小可寻址单元。

```zephir
char ch, string name = "peter";
```

##### stores 't'

```zephir
let ch = name[2];
```

##### `char`文字必须用单引号括起来

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

##### 自动转换为100

```zephir
let a = 100.25;
```

##### 自动转换为0

```zephir
let a = null;
```

##### 自动转换为0

```zephir
let a = false;
```

##### 抛出编译器异常

```zephir
let a = "hello";
```

`unsigned integer`变量类似于`integer`但它们没有符号，这意味着你不能在这些变量中存储负数:

```zephir
uint a;

let a = 50;
```

##### 自动转换为70

```zephir
let a = -70;
```

##### 自动转换为100

```zephir
let a = 100.25;
```

##### 自动转换为0

```zephir
let a = null;
```

##### 自动转换为0

```zephir
let a = false;
```

##### 抛出编译器异常

```zephir
let a = "hello";
```

< 0>unsigned integer</0 > 变量比标准 `integer` 大两倍。 将 `unsigned integer</0 > 分配给标准 (有符号) <code>integer` 可能会导致数据丢失:

##### `b` 的数据可能丢失

```zephir
uint a, int b;

let a = 2147483648,
    b = a;
```

<a name='static-types-long-unsigned'></a>

### 长值/无符号长值

`long` 变量比 `integer` 变量大两倍, 因此它们可以存储较大的数字。 与 `integer` 一样, 分配给 `long` 变量的值将自动转换为此类型:

```zephir
long a;

let a = 50,
    a = -70;
```

##### 自动转换为100

```zephir
let a = 100.25;
```

##### 自动转换为0

```zephir
    let a = null;
```

##### 自动转换为0

```zephir
let a = false;
```

##### 抛出编译器异常

```zephir
let a = "hello";
```

< 0>unsigned long</0 > 类似 `long` 但它们没有符号, 这意味着您不能将负数存储在以下类型的变量中:

```zephir
ulong a;

let a = 50;
```

##### 自动转换为70

```zephir
let  a = -70;
```

##### 自动转换为100

```zephir
let a = 100.25;
```

##### 自动转换为0

```zephir
let a = null;
```

##### 自动转换为0

```zephir
let a = false;
```

##### 抛出编译器异常

```zephir
let a = "hello";
```

< 0>unsigned long </0 > 变量比标准 `long` 大两倍; 将 `unsigned long</0 > 分配给标准 (有符号) <code>long` 可能会导致数据丢失:

##### `b` 的数据可能丢失

```zephir
ulong a, long b;

let a = 4294967296,
    b = a;
```

<a name='static-types-string'></a>

### String

一个string是一系列字符，其中字符与字节相同。 与PHP一样，Zephir只支持256个字符集，因此不提供本地Unicode支持。

当一个变量被声明为`string`时，它永远不会改变它的类型:

```zephir
string a;

let a = "";
```

##### string 文字必须用双引号括起来

```zephir
let  a = "hello";
```

##### 转换为字符串“A”

```zephir
let a = 'A';
```

##### 自动转换为""

```zephir
let a = null;
```
