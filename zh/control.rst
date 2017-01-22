控制结构
==================
Zephir中的控制语句与C和PHP等语言的非常类似。

条件语句
------------

If语句
^^^^^^^^^^^^
'if'语句内的代码其条件为真时执行。花括号是必须要写的，'if'语句有一个可选的'else'语句，多个'if'/'else'控制语句
可以进行连写。

.. code-block:: zephir

    if false {
        echo "false?";
    } else {
        if true {
            echo "true!";
        } else {
            echo "neither true nor false";
        }
    }

我们也可以使用'elseif':

.. code-block:: zephir

    if a > 100 {
        echo "to big";
    } elseif a < 0 {
        echo "to small";
    } elseif a == 50 {
        echo "perfect!";
    } else {
        echo "ok";
    }

小括号在条件语句中是可选择的:

.. code-block:: zephir

    if a < 0 { return -1; } else { if a > 0 { return 1; } }

分支语句
^^^^^^^^^^^^^^^^
'switch'语句对表达式进行评估之后与'case'后的预定义值相匹配,若匹配成功则执行相应的操作，
未匹配成功的值则会由'default'处理(其处理方式与php一致):

.. code-block:: zephir

    switch count(items) {

        case 1:
        case 3:
            echo "odd items";
            break;

        case 2:
        case 4:
            echo "even items";
            break;

        default:
            echo "unknown items";
    }

循环
-----

While语句
^^^^^^^^^^^^^^^
'while'语句在其表达式为真时会不断的循环执行:

.. code-block:: zephir

    let counter = 5;
    while counter {
        let counter -= 1;
    }

Loop语句
^^^^^^^^^^^^^^
除了'while'之外，'loop'可以用来创建无限循环:

.. code-block:: zephir

    let n = 40;
    loop {
        let n -= 2;
        if n % 5 == 0 { break; }
        echo x, "\n";
    }

For语句
^^^^^^^^^^^^^
'for'语句可以用来遍历数组或字符串:

.. code-block:: zephir

    for item in ["a", "b", "c", "d"] {
        echo item, "\n";
    }

hash中的键值对可以使用如下方式来取得:

.. code-block:: zephir

    let items = ["a": 1, "b": 2, "c": 3, "d": 4];

    for key, value in items {
        echo key, " ", value, "\n";
    }

'for'语句也可以用来反向遍历数组或字符串:

.. code-block:: zephir

    let items = [1, 2, 3, 4, 5];

    for value in reverse items {
        echo value, "\n";
    }

'for'语句可以用来遍历字符串变量:

.. code-block:: zephir

    string language = "zephir"; char ch;

    for ch in language {
        echo "[", ch ,"]";
    }

反向遍历:

.. code-block:: zephir

    string language = "zephir"; char ch;

    for ch in reverse language {
        echo "[", ch ,"]";
    }

标准的'for'语句可以用来遍历range函数生成的array:

.. code-block:: zephir

    for i in range(1, 10) {
        echo i, "\n";
    }

如果你只想取键值对中的一个则可以使用匿名变量"_"（下划线）来屏蔽另一个不需要的，这可以避免警告的发出:

.. code-block:: zephir

    // 只用key不用value反之亦然
    for key, _ in data {
        echo key, "\n";
    }

Break语句
^^^^^^^^^^^^^^^
'break'语句可以用来停止'while'，'for'，'loop'等语句:

.. code-block:: zephir

    for item in ["a", "b", "c", "d"] {
        if item == "c" {
            break; // 退出for循环
        }
        echo item, "\n";
    }

Continue语句
^^^^^^^^^^^^^^^^^^
'continue'语句用在循环内部以实现跳过当前循环后面的代码，继续执行进行下面的循环操作。

.. code-block:: zephir

    let a = 5;
    while a > 0 {
        let a--;
        if a == 3 {
            continue;
        }
        echo a, "\n";
    }

Require语句
-------
这里的'require'的作用与PHP中的require的作用是一样，注意在Zephir中require的文件只能是PHP文件不能是Zephir文件。

.. code-block:: zephir

    if file_exists(path) {
        require path;
    }

Let
---
'Let'语句来用修改变量，属性或数组的值。Zephir中的变量默认是不可修改的，let语句可以让这些变量变成可修改的（相当于加了一层安全保障）:

.. code-block:: zephir

    let name = "Tony";           // 简单变量
    let this->name = "Tony";     // 对象属性
    let data["name"] = "Tony";   // array索引
    let self::_name = "Tony";    // 静态属性

当然let指令也可以用来对变量做自增或自减:

.. code-block:: zephir

    let number++;           // 对简单变量进行自增运算
    let number--;           // 对简单变量进行自减运算
    let this->number++;     // 对对象属性进行自增运算
    let this->number--;     // 对简单变量进行自减运算

在一个'let'语句中可以进行多个更新操作:

.. code-block:: zephir

    let price = 1.00, realPrice = price, status = false;
