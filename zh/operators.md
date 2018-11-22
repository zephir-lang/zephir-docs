# 运算符

Zephir的操作符与PHP中的操作符类似，并且继承了它们的一些行为。

<a name='arithmetic-operators'></a>

## Arithmetic Operators

支持一下操作符

| 操作 | 示例      |
| -- | ------- |
| 求反 | `-a`    |
| 加  | `a + b` |
| 相减 | `a - b` |
| 乘  | `a * b` |
| 除  | `a / b` |
| 取模 | `a % b` |

<a name='bitwise-operators'></a>

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

    if a & SOME_FLAG {
        echo "has some flag";
    }
    

了解[php手册](http://www.php.net/manual/en/language.operators.comparison.php)中动态变量的比较。

<a name='comparison-operators'></a>

## 比较运算符

比较运算符取决于比较变量的类型。 例如，如果两个比较操作数都是动态变量，其行为与PHP相同:

| 示例             | 操作                       | 说明                                         |
| -------------- | ------------------------ | ------------------------------------------ |
| `a == b`       | 等于                       | `true`如果a在去除类型后等于b。                        |
| `a === b`      | 完全相同的                    | `true`如果a等于b，它们是相同类型的。                     |
| `a != b`       | 不等于                      | `true`如果a在去除类型后不等于b。                       |
| `a <> b` | 不等于                      | `true`如果a在去除类型后不等于b。                       |
| `a !== b`      | 不一致                      | `true`如果a不等于b，或者它们不是同一类型。                  |
| `a < b`     | 小于                       | 如果a严格小于b，则`true`。                          |
| `a > b`     | 大于                       | 如果a严格大于b，则`true`。                          |
| `a <= b`    | 小于或等于                    | `true` if a is less than or equal to b.    |
| `a >= b`    | Greater than or equal to | `true` if a is greater than or equal to b. |

示例︰

    if a == b {
        return 0;
    } else {
        if a < b {
            return -1;
        } else {
            return 1;
        }
    }
    

<a name='logical-operators'></a>

## 逻辑运算符

支持一下操作符

| 操作 | 示例               |
| -- | ---------------- |
| 与  | `a && b` |
| Or | `a || b`         |
| 非  | `!a`             |

示例︰

    if a && b || !c {
        return -1;
    }
    return 1;
    

<a name='tenary-operator'></a>

## 三元运算符

Zephir supports the ternary operator available in C or PHP:

    let b = a == 1 ? "x" : "y"; // b is set to "x" if a is equal to 1, otherwise "y" is assigned as the value
    

<a name='special-operators'></a>

## Special Operators

支持一下操作符

<a name='special-operators-empty'></a>

### Empty

This operator allows checking whether an expression is empty. 'Empty' means the expression is `null`, is an empty string, or an empty array:

    let someVar = "";
    if empty someVar {
        echo "is empty!";
    }
    
    let someVar = "hello";
    if !empty someVar {
        echo "is not empty!";
    }
    

<a name='special-operators-fetch'></a>

### Fetch

'Fetch' is an operator that reduces a common operation in PHP into a single instruction:

    <?php
    
    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }
    

In Zephir, you can write the same code as:

    if fetch value, myArray[key] {
        echo value;
    }
    

'Fetch' only returns `true` if the 'key' is a valid item in the array, and only in that case is 'value' populated.

<a name='special-operators-isset'></a>

### Isset

This operator checks whether a property or index has been defined in an array or object:

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // check if the array has an index "b"
        echo "yes, it has an index 'b'\n";
    }
    

Using `isset` as a return expression:

    return isset this->{someProperty};
    

Note that `isset` in Zephir works more like PHP's function [array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php), `isset` in Zephir returns true even if the array index or property is null.

<a name='special-operators-typeof'></a>

### Typeof

This operator checks a variable's type. 'typeof' can be used with a comparison operator:

    if (typeof str == "string") { // or !=
        echo str;
    }
    

It can also work like the PHP function `gettype`.

    return typeof str;
    

**Be careful**, if you want to check whether an object is 'callable', you always have to use `typeof` as a comparison operator, not a function.

<a name='special-operators-type-hints'></a>

### 类型提示

Zephir always tries to check whether an object implements methods and properties called/accessed on a variable that is inferred to be an object:

    let o = new MyObject();
    
    // Zephir checks if "myMethod" is implemented on MyObject
    o->myMethod();
    

However, due to the dynamism inherited from PHP, sometimes it is not easy to know the class of an object, so Zephir can't produce error reports effectively. A type hint tells the compiler which class is related to a dynamic variable, allowing the compiler to perform more compilation checks:

    // Tell the compiler that "o"
    // is an instance of class MyClass
    let o = <MyClass> this->_myObject;
    o->myMethod();
    

These "type hints" are weak. This means the program does not check if the value is in fact an instance of the specified class, nor whether it implements the specified interface. If you want it to check this every time in execution, use a strict type:

    // Always check if the property is an instance
    // of MyClass before the assignment
    let o = <MyClass!> this->_myObject;
    o->myMethod();
    

<a name='special-operators-branch-prediction-hints'></a>

### 分支预测提示

What is branch prediction? Check this [article](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) or refer to the [Wikipedia article](https://en.wikipedia.org/wiki/Branch_predictor). In environments where performance is very important, it may be useful to introduce these hints.

请考虑下面的示例:

    let allPaths = [];
    for path in this->_paths {
        if path->isAllowed() == false {
            throw new App\Exception("Some error message here");
        } else {
            let allPaths[] = path;
        }
    }
    

The authors of the above code know in advance that the condition that throws the exception is unlikely to happen. This means that, 99.9% of the time, our method executes that condition, but it is probably never evaluated as true. For the processor, this could be hard to know, so we could introduce a hint there:

    let allPaths = [];
    for path in this->_paths {
        if unlikely path->isAllowed() == false {
            throw new App\Exception("Some error message here");
        } else {
            let allPaths[] = path;
        }
    }