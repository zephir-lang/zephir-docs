---
layout: default
language: 'zh-cn'
version: '0.12'
---

# 运算符

This article describes Zephir's operators. For precedence, you can check the [Operator Precedence](/{{ page.version }}/{{ page.language }}/operator-precedence) article.

Zephir's operators are similar to the ones in PHP, and inherit some of their behaviors.

<a id='arithmetic-operators'></a>

## 算术运算符

支持一下操作符

| 操作 | 示例      |
| -- | ------- |
| 求反 | `-a`    |
| 加  | `a + b` |
| 相减 | `a - b` |
| 乘  | `a * b` |
| 除  | `a / b` |
| 取模 | `a % b` |

<a id='bitwise-operators'></a>

## 按位运算符

支持一下操作符

| 操作                 | 示例             |
| ------------------ | -------------- |
| 与                  | `a & b`    |
| 或(包括)              | `a | b`        |
| Xor (exclusive or) | `a ^ b`        |
| 非                  | `~a`           |
| 左移                 | `a << b` |
| 右移                 | `a >> b` |


示例︰

```zephir
if a & SOME_FLAG {
    echo "has some flag";
}
```

Learn more about comparison of dynamic variables in the [php manual](https://www.php.net/manual/en/language.operators.comparison.php).

<a id='comparison-operators'></a>

## 比较运算符

比较运算符取决于比较变量的类型。 例如，如果两个比较操作数都是动态变量，其行为与PHP相同:

| 示例             | 操作    | 说明                        |
| -------------- | ----- | ------------------------- |
| `a == b`       | 等于    | `true`如果a在去除类型后等于b。       |
| `a === b`      | 完全相同的 | `true`如果a等于b，它们是相同类型的。    |
| `a != b`       | 不等于   | `true`如果a在去除类型后不等于b。      |
| `a <> b` | 不等于   | `true`如果a在去除类型后不等于b。      |
| `a !== b`      | 不一致   | `true`如果a不等于b，或者它们不是同一类型。 |
| `a < b`     | 小于    | 如果a严格小于b，则`true`。         |
| `a > b`     | 大于    | 如果a严格大于b，则`true`。         |
| `a <= b`    | 小于或等于 | `true`如果a小于或等于b。          |
| `a >= b`    | 大于或等于 | `true`如果a大于或等于b。          |


示例︰

```zephir
if a == b {
    return 0;
} else {
    if a < b {
        return -1;
    } else {
        return 1;
    }
}
```

<a id='logical-operators'></a>

## 逻辑运算符

支持一下操作符

| 操作 | 示例               |
| -- | ---------------- |
| 与  | `a && b` |
| 或  | `a || b`         |
| 非  | `!a`             |


示例︰

```zephir
if a && b || !c {
    return -1;
}
return 1;
```

<a id='tenary-operator'></a>

## 三元运算符

Zephir支持C或PHP中的三元运算符:

```zephir
let b = a == 1 ? “x”:“y”; //如果a = 1， // b设为“x”，否则赋值为“y”
```

<a id='special-operators'></a>

## 特殊运算符

支持一下操作符

<a id='special-operators-empty'></a>

### Empty

这个运算符允许检查表达式是否为空。 ‘Empty’表示表达式为`null`，可以是空字符串或空数组:

```zephir
let someVar = "";
if empty someVar {
    echo "is empty!";
}

let someVar = "hello";
if !empty someVar {
    echo "is not empty!";
}
```

<a id='special-operators-fetch'></a>

### Fetch

Fetch操作符将PHP中的一个常见操作简化为一条指令:

```php
<?php

if (isset($myArray[$key])) {
    $value = $myArray[$key];
    echo $value;
}
```

在Zephir中，您可以编写与以下代码相同的代码:

```zephir
if fetch value, myArray[key] {
    echo value;
}
```

'Fetch'只返回`true`，只有在'key'是数组中的有效项的情况下进行'value'填充。

<a id='special-operators-isset'></a>

### Isset

这个操作符检查是否在数组或对象中定义了属性或索引:

```zephir
let someArray = ["a": 1, "b": 2, "c": 3];
if isset someArray["b"] { // check if the array has an index "b"
    echo "yes, it has an index 'b'\n";
}
```

使用`isset`作为返回表达式:

```zephir
return isset this->{someProperty};
```

Note that `isset` in Zephir works more like PHP's function [array_key_exists](https://www.php.net/manual/en/function.array-key-exists.php), `isset` in Zephir returns true even if the array index or property is null.

<a id='special-operators-typeof'></a>

### Typeof

这个操作符检查变量的类型。 'typeof'可与比较运算符一起使用:

```zephir
if (typeof str == "string") { // or !=
    echo str;
}
```

它也可以像PHP函数`gettype`那样工作。

```zephir
return typeof str;
```

**Be careful**, if you want to check whether an object is 'callable', you always have to use `typeof` as a comparison operator, not a function.

<a id='special-operators-type-hints'></a>

### 类型提示

Zephir总是试图检查一个对象是否实现了方法和属性，这些方法和属性在一个被推断为对象的变量上被调用/访问:

```zephir
let o = new MyObject();

// Zephir checks if "myMethod" is implemented on MyObject
o->myMethod();
```

但是，由于继承自PHP的动态性，有时很难知道对象的类，所以Zephir无法有效地生成错误报告。 类型提示告诉编译器哪个类与动态变量相关，允许编译器执行更多的编译检查:

```zephir
// Tell the compiler that "o"
// is an instance of class MyClass
let o = <MyClass> this->_myObject;
o->myMethod();
```

这些 "类型提示" 很弱。 这意味着程序不检查该值是否实际上是指定类的实例, 也不检查它是否实现了指定的接口。 如果希望它每次执行时都检查此问题, 请使用严格的类型:

```zephir
// 始终检查属性是否为实例
// 在使用前检查
let o = <MyClass!> this->_myObject;
o->myMethod();
```

<a id='special-operators-branch-prediction-hints'></a>

### 分支预测提示

什么是分支预测？ Check this [article](https://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) or refer to the [Wikipedia article](https://en.wikipedia.org/wiki/Branch_predictor). 在性能非常重要的环境中, 引入这些提示可能会很有用。

请考虑下面的示例:

```zephir
let allPaths = [];
for path in this->_paths {
    if path->isAllowed() == false {
        throw new App\Exception("Some error message here");
    } else {
        let allPaths[] = path;
    }
}
```

上述代码的作者事先知道, 引发异常的条件不太可能发生。 这意味着, 99.9% 的时间, 我们的方法执行该条件, 但它可能永远不会被评估为 true。 对于处理器, 这可能很难知道, 因此我们可以在那里引入一个提示:

```zephir
let allPaths = [];
for path in this->_paths {
    if unlikely path->isAllowed() == false {
        throw new App\Exception("Some error message here");
    } else {
        let allPaths[] = path;
    }
}
```