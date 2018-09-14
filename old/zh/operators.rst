运算符
=========
Zephir中去处符与PHP中的运算符行为近似。

算术运算符
--------------------
Zephir中支持如下算术运算符:

+--------+--------+
| 运算符 | 例子   |
+--------+--------+
| 负号   | -a     |
+--------+--------+
| 加     | a + b  |
+--------+--------+
| 减     | a - b  |
+--------+--------+
| 乘     | a * b  |
+--------+--------+
| 除     | a / b  |
+--------+--------+
| 取模   | a % b  |
+--------+--------+

比较运算符
--------------------
比较运算符依赖于变量的类型，例如，如果两个变量都是动态变量则其表现同于PHP中的比较运算符:

+----------+----------+----------------------------------+
| a == b   | 相等     | 相等时为真                       |
+----------+----------+----------------------------------+
| a === b  | 全等     | 相等且类型相同时为真             |
+----------+----------+----------------------------------+
| a != b   | 不等     | 不等时为真                       |
+----------+----------+----------------------------------+
| a <> b   | 不等     | 不等时为真                       |
+----------+----------+----------------------------------+
| a !== b  | 不全等   | 值不等或值相等但不同类型时不为真 |
+----------+----------+----------------------------------+
| a < b    | 小于     | a小于b时为真                     |
+----------+----------+----------------------------------+
| a > b    | 大于     | a大于b时为真                     |
+----------+----------+----------------------------------+
| a <= b   | 小于等于 | a小于等于b时为真                 |
+----------+----------+----------------------------------+
| a >= b   | 大于等于 | a大于等于b时为真                 |
+----------+----------+----------------------------------+

例子:

.. code-block:: zephir

    if a == b {
        return 0;
    } else {
        if a < b {
            return -1;
        } else {
            return 1;
        }
    }

逻辑操作符
-----------------
Zephir中支持如下逻辑操作符:

+--------+--------+
| 运算符 | 例子   |
+--------+--------+
| 逻辑与 | a && b |
+--------+--------+
| 逻辑或 | a || b |
+--------+--------+
| 非     | !a     |
+--------+--------+

例子:

.. code-block:: zephir

    if a && b || !c {
        return -1;
    }
    return 1;

位操作符
-----------------
Zephir中支持下列位操作符:

+----------+--------+
| 运算符   | 例子   |
+----------+--------+
| 按位与   | a & b  |
+----------+--------+
| 按位或   | a | b  |
+----------+--------+
| 按位异或 | a ^ b  |
+----------+--------+
| 按位取反 | ~a     |
+----------+--------+
| 左移     | a << b |
+----------+--------+
| 右移     | a >> b |
+----------+--------+

例子:

.. code-block:: zephir

    if a & SOME_FLAG {
        echo "has some flag";
    }

要了解更多动态类型对比可以参见 `php manual`_.

三目运算符
----------------
Zephir像C和PHP一样支持三目运算符:

.. code-block:: zephir

    let b = a == 1 ? "x" : "y"; // a等于1时设置为"x"反之设置为"y"

特殊运算符
-----------------
Zephir支持如下的特殊运算符:

empty运算符
^^^^^^^^^
这个运算符会检查一个表达式是否为空。空意味着null，一般为空字符串或空数组等:

.. code-block:: zephir

    let someVar = "";
    if empty someVar {
        echo "is empty!";
    }

    let someVar = "hello";
    if !empty someVar {
        echo "is not empty!";
    }

Isset运算符
^^^^^^^^^^^
这个运算符会检查一个属性或索引是否在类或数组中定义了:

.. code-block:: zephir

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // 检查数组中是否有键b
        echo "yes, it has an index 'b'\n";
    }

在函数的返回在值中使用isset运算符:

.. code-block:: zephir

    return isset this->{someProperty};

注意: Zephir中的isset操作符的行为类似PHP中的 array_key_exists_, 即即使数组或对象中的元素为null也会返回真值。

Fetch运算符
^^^^^^^^^^
fetch运算符可以把一组取值操作精简成一条指令:

.. code-block:: php

    <?php

    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }

Zephir中，可以直接这样写

.. code-block:: zephir

    if fetch value, myArray[key] {
        echo value;
    }

fetch只在数组中有合法的键时才返回其值。

Typeof运算符
^^^^^^^^^^^^
这个运算符用来检查变量的类型。
typeof操作符用起来像是比较运算符：

.. code-block:: zephir

    if (typeof str == "string") { // or !=
        echo str;
    }

typeof运算符的作用等同于PHP中的gettype函数。

.. code-block:: zephir

    return typeof str;

**特别注意**, 当你想检查一个对象是否是可调用时，你最好使用typeof来进行对比。 

类型暗示
^^^^^^^^^^
Zephir总是在调用方法或访问属性时检查其是否实现了该方法或是有该属性:

.. code-block:: zephir

    let o = new MyObject();

    //Zephir会检查对象o的类是否有myMethod方法。
    o->myMethod();

由于Zephir从PHP继承了动态机制，因此有些要找出对象的类是不容易的而且容易出错。这时类型暗示即可起到作用了，Zephir的编译器会根据暗示在编译期执行更多的检查:

.. code-block:: zephir

    //告诉编译器o是MyClass的对象
    let o = <MyClass> this->_myObject;
    o->myMethod();

当然这里是弱的类型暗示，这意味着程序并不检查该对象是否真的是暗示的类的对象或是其接口的实现者。如果想要在执行期进行类型检查的话:

.. code-block:: zephir

    // 在执行赋值之会进行检查
    let o = <MyClass!> this->_myObject;
    o->myMethod();

分支预测暗示
^^^^^^^^^^^^^^^^^^^^^^^
什么是分支暗示？参见 `article out`_ 或 `Wikipedia article`_。 性能要求比较重要的地方使用分支暗示比较好。

考虑如下的代码:

.. code-block:: zephir

    let allPaths = [];
    for path in this->_paths {
        if path->isAllowed() == false {
            throw new App\Exception("Some error message here");
        } else {
            let allPaths[] = path;
        }
    }

代码作者肯定知道上面的异常情况是极少发生的。这就意思着99.9%的情况下是述异常不会发生。但对处理器来说是不知道，怎样让处理器知道呢，
这里我们引入了一个关键字:

.. code-block:: zephir

    let allPaths = [];
    for path in this->_paths {
        if unlikely path->isAllowed() == false {
            throw new App\Exception("Some error message here");
        } else {
            let allPaths[] = path;
        }
    }

.. _`array_key_exists`: http://www.php.net/manual/en/function.array-key-exists.php
.. _`php manual`: http://www.php.net/manual/en/language.operators.comparison.php
.. _`article out`: http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/
.. _`Wikipedia article`: https://en.wikipedia.org/wiki/Branch_predictor
