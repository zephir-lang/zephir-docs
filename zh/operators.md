# 运算符

Zephir的操作符与PHP中的操作符类似，并且继承了它们的一些行为。

<a name='arithmetic-operators'></a>

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

<a name='bitwise-operators'></a>

## Bitwise Operators

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

## Comparison Operators

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

## Logical Operators

支持一下操作符

| 操作 | 示例               |
| -- | ---------------- |
| 与  | `a && b` |
| 或  | `a || b`         |
| 非  | `!a`             |

示例︰

    if a && b || !c {
        return -1;
    }
    return 1;
    

<a name='tenary-operator'></a>

## 三元运算符

Zephir支持C或PHP中的三进制运算符:

    let b = a == 1 ? “x”:“y”; //如果a = 1， // b设为“x”，否则赋值为“y”
    

<a name='special-operators'></a>

## 特殊运算符

支持一下操作符

<a name='special-operators-empty'></a>

### Empty

这个运算符允许检查表达式是否为空。 ‘Empty’表示表达式为`null`，可以是空字符串或空数组:

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

Fetch操作符将PHP中的一个常见操作简化为一条指令:

    <?php
    
    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }
    

在Zephir中，您可以编写与以下代码相同的代码:

    if fetch value, myArray[key] {
        echo value;
    }
    

'Fetch'只返回`true`，只有在'key'是数组中的有效项的情况下进行'value'填充。

<a name='special-operators-isset'></a>

### Isset

这个操作符检查是否在数组或对象中定义了属性或索引:

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // check if the array has an index "b"
        echo "yes, it has an index 'b'\n";
    }
    

使用`isset`作为返回表达式:

    return isset this->{someProperty};
    

注意，在Zephir中`isset` </code>更像PHP的函数[array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php)，在Zephir中`isset</0>即使数组索引或属性为空也返回true。</p>

<p>

<a name='special-operators-typeof'></a>

</p>

<h3>Typeof</h3>

<p>这个操作符检查变量的类型。 'typeof'可与比较运算符一起使用:</p>

<pre><code>if (typeof str == "string") { // or !=
    echo str;
}
`</pre> 

它也可以像PHP函数`gettype`那样工作。

    return typeof str;
    

**坑: **，如果你想检查一个对象是否“callable”，你总是必须使用`typeof`作为比较运算符，而不是函数。

<a name='special-operators-type-hints'></a>

### 类型提示

Zephir总是试图检查一个对象是否实现了方法和属性，这些方法和属性在一个被推断为对象的变量上被调用/访问:

    let o = new MyObject();
    
    // Zephir checks if "myMethod" is implemented on MyObject
    o->myMethod();
    

但是，由于继承自PHP的动态性，有时很难知道对象的类，所以Zephir无法有效地生成错误报告。 A type hint tells the compiler which class is related to a dynamic variable, allowing the compiler to perform more compilation checks:

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

### Branch Prediction Hints

What is branch prediction? Check this [article](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) or refer to the [Wikipedia article](https://en.wikipedia.org/wiki/Branch_predictor). In environments where performance is very important, it may be useful to introduce these hints.

Consider the following example:

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