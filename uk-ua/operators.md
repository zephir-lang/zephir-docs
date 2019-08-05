---
layout: default
language: 'en'
version: '0.12'
---

# Operators

Zephir's operators are similar to the ones in PHP, and also inherit some of their behaviors.

<a name='arithmetic-operators'></a>

## Arithmetic Operators

The following operators are supported:

| Operation      | Example |
| -------------- | ------- |
| Negation       | `-a`    |
| Addition       | `a + b` |
| Subtraction    | `a - b` |
| Multiplication | `a * b` |
| Division       | `a / b` |
| Modulus        | `a % b` |

<a name='bitwise-operators'></a>

## Bitwise Operators

The following operators are supported:

| Operation          | Example        |
| ------------------ | -------------- |
| And                | `a & b`    |
| Or (inclusive or)  | `a | b`        |
| Xor (exclusive or) | `a ^ b`        |
| Not                | `~a`           |
| Shift left         | `a << b` |
| Shift right        | `a >> b` |


Example:

```zephir
if a & SOME_FLAG {
    echo "has some flag";
}
```

Learn more about comparison of dynamic variables in the [php manual](http://www.php.net/manual/en/language.operators.comparison.php).

<a name='comparison-operators'></a>

## Comparison Operators

Comparison operators depend on the type of variables compared. For example, if both compared operands are dynamic variables, the behavior is the same as in PHP:

| Example        | Operation                | Опис                                                             |
| -------------- | ------------------------ | ---------------------------------------------------------------- |
| `a == b`       | Equal                    | `true` if a is equal to b after type juggling.                   |
| `a === b`      | Identical                | `true` if a is equal to b, and they are of the same type.        |
| `a != b`       | Not equal                | `true` if a is not equal to b after type juggling.               |
| `a <> b` | Not equal                | `true` if a is not equal to b after type juggling.               |
| `a !== b`      | Not identical            | `true` if a is not equal to b, or they are not of the same type. |
| `a < b`     | Less than                | `true` if a is strictly less than b.                             |
| `a > b`     | Greater than             | `true` if a is strictly greater than b.                          |
| `a <= b`    | Less than or equal to    | `true` if a is less than or equal to b.                          |
| `a >= b`    | Greater than or equal to | `true` if a is greater than or equal to b.                       |


Example:

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

<a name='logical-operators'></a>

## Logical Operators

The following operators are supported:

| Operation | Example          |
| --------- | ---------------- |
| And       | `a && b` |
| Or        | `a || b`         |
| Not       | `!a`             |


Example:

```zephir
if a && b || !c {
    return -1;
}
return 1;
```

<a name='tenary-operator'></a>

## Ternary Operator

Zephir supports the ternary operator available in C or PHP:

```zephir
let b = a == 1 ? "x" : "y"; // b is set to "x" if a is equal to 1, otherwise "y" is assigned as the value
```

<a name='special-operators'></a>

## Special Operators

The following operators are supported:

<a name='special-operators-empty'></a>

### Empty

This operator allows checking whether an expression is empty. 'Empty' means the expression is `null`, is an empty string, or an empty array:

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

<a name='special-operators-fetch'></a>

### Fetch

'Fetch' is an operator that reduces a common operation in PHP into a single instruction:

```php
<?php

if (isset($myArray[$key])) {
    $value = $myArray[$key];
    echo $value;
}
```

In Zephir, you can write the same code as:

```zephir
if fetch value, myArray[key] {
    echo value;
}
```

'Fetch' only returns `true` if the 'key' is a valid item in the array, and only in that case is 'value' populated.

<a name='special-operators-isset'></a>

### Isset

This operator checks whether a property or index has been defined in an array or object:

```zephir
let someArray = ["a": 1, "b": 2, "c": 3];
if isset someArray["b"] { // check if the array has an index "b"
    echo "yes, it has an index 'b'\n";
}
```

Using `isset` as a return expression:

```zephir
return isset this->{someProperty};
```

Note that `isset` in Zephir works more like PHP's function [array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php), `isset` in Zephir returns true even if the array index or property is null.

<a name='special-operators-typeof'></a>

### Typeof

This operator checks a variable's type. 'typeof' can be used with a comparison operator:

```zephir
if (typeof str == "string") { // or !=
    echo str;
}
```

It can also work like the PHP function `gettype`.

```zephir
return typeof str;
```

**Be careful**, if you want to check whether an object is 'callable', you always have to use `typeof` as a comparison operator, not a function.

<a name='special-operators-type-hints'></a>

### Type Hints

Zephir always tries to check whether an object implements methods and properties called/accessed on a variable that is inferred to be an object:

```zephir
let o = new MyObject();

// Zephir checks if "myMethod" is implemented on MyObject
o->myMethod();
```

However, due to the dynamism inherited from PHP, sometimes it is not easy to know the class of an object, so Zephir can't produce error reports effectively. A type hint tells the compiler which class is related to a dynamic variable, allowing the compiler to perform more compilation checks:

```zephir
// Tell the compiler that "o"
// is an instance of class MyClass
let o = <MyClass> this->_myObject;
o->myMethod();
```

These "type hints" are weak. This means the program does not check if the value is in fact an instance of the specified class, nor whether it implements the specified interface. If you want it to check this every time in execution, use a strict type:

```zephir
// Always check if the property is an instance
// of MyClass before the assignment
let o = <MyClass!> this->_myObject;
o->myMethod();
```

<a name='special-operators-branch-prediction-hints'></a>

### Branch Prediction Hints

What is branch prediction? Check this [article](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) or refer to the [Wikipedia article](https://en.wikipedia.org/wiki/Branch_predictor). In environments where performance is very important, it may be useful to introduce these hints.

Consider the following example:

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

The authors of the above code know in advance that the condition that throws the exception is unlikely to happen. This means that, 99.9% of the time, our method executes that condition, but it is probably never evaluated as true. For the processor, this could be hard to know, so we could introduce a hint there:

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