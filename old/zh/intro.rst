介绍 Zephir
==================
Zephir这门语言满足了PHP开发者写出可运行于Zend引擎上的编译型代码的需求。这门语言即是动态的又是静态的，他的一些特性对PHP的开发者来说
不会陌生。

Zephir这个单词取自Zend Engine/PHP/Intermediate。从这拼写上我们可以看出这个单词应该读作zephyr，Zephir的创造者读作他为 zaefire_。

Hello World!
------------
惯例每门语言在初学时都是从"Hello World!"的例子开始的，从这个例子中我们可以看出Zephir语言的一些特点。

Zephir中的代码必须放在类中。这门语言设计的目的是让开发者用来开发面向对象的类库或框架的，任何类之外的执行代码是不允许出现的。当然命名空间也要写:

.. code-block:: zephir

    namespace Test;

    /**
     * This is a sample class
     */
    class Hello
    {
        /**
         * This is a sample method
         */
        public function say()
        {
            echo "Hello World!";
        }
    }

一旦上面的类被编译后其产生的由gcc/clang或vc++编译器处理的中间代码如下：

.. code-block:: c

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif

    #include "php.h"
    #include "php_test.h"
    #include "test.h"

    #include "kernel/main.h"

    /**
     * This is a sample class
     */
    ZEPHIR_INIT_CLASS(Test_Hello) {
        ZEPHIR_REGISTER_CLASS(Test, Hello, hello, test_hello_method_entry, 0);
        return SUCCESS;
    }

    /**
     * This is a sample method
     */
    PHP_METHOD(Test_Hello, say) {
        php_printf("%s", "Hello World!");
    }

事实上开发者不需要了解C语言，当然如果你有编译器、PHP内核相关知识或C语言相关知识的话会对你使用Zephir进行开发带来很多好处，
你会更清晰的知道这一切是如何进行的。

体验Zephir
-----------------
下面的例子中，我们会更深入的描述，这样我们可以更清晰了解其是如何工作的。这可以让我们更进一步的了解Zephir是如何编程的。
在接下来的章节中我们会进行更深入的探讨。

下面的例子非常简单，下面的这个类实现了一个检查数组类型的一小段代码

让我们深入的看一下这段代码，通过这段代码我们开始Zephir语法的学习之旅吧。下面的这段的代码注释的比较清晰，下面是注解:

.. code-block:: zephir

    namespace Test;

    /**
     * MyTest (test/mytest.zep)
     */
    class MyTest
    {
        public function someMethod()
        {
            /* 变量必须提前定义 */
            var myArray;
            int i = 0, length;

            /* 创建Array */
            let myArray = ["hello", 0, 100.25, false, null];

            /* 取myArray的元素个数然后赋值给length */
            let length = count(myArray);

            /* 打印输出值类型 */
            while i < length {
                echo typeof myArray[i], "\n";
                let i++;
            }

            return myArray;
        }
    }

上面的方法中的第一行使用var与int关键字用来定义局部代码中的变量。每个变量必须有指定的类型。定义是必须的，这样类型错误就会在编译时直接被检测出来。

动态变量使用var关键字进行定义。动态变量可以被赋值和再赋值成不同种类的变量。与之相似的上面代码中的i与length的类型在其整个生命周期中是固定不变的。

与PHP不同的是Zephir中的变量是不需要以$开头。

Zephir中的注释与Java，C#，C++，等相同。
//是行注释，/* 这里是注释 */是块注释。

变量默认情况下是不可修改的，这就意味着Zephir期望大部分的变量是不修改的。变量的不可修改可以使得编译器在生成代码时直接将其替换成静态常量。
当需要修改变量的值是，必须使用关键字let:

.. code-block:: zephir

    /* 创建array */
    let myArray = ["hello", 0, 100.25, false, null];

默认情况下，Zephir中的数组与PHP中的类似，数组中可以有多种类型的数据。PHP中的函数在Zephir中可以直接使用，上面的例子中我们使用了count方法，
编译器会对上面的调用进行优化，因为编译器已经知道了数组的大小:

.. code-block:: zephir

    /* 取myArray的元素个数然后赋值给length */
    let length = count(myArray);

控制语句中的花括号是可选的，如果使用他们会让我们感觉更爽尽管用就是。
.. code-block:: zephir

    while i < length {
        echo typeof myArray[i], "\n";
        let i++;
    }

PHP只使用动态变量（PHP7中也可以使用静态变量了），Zephir中的方法通常返回动态变量，这就意味着如果Zephir方法返回了静态变量，
在PHP端你也会收到可用的动态值(中间的转化过程由Zephir的编译器进行的)。请注意Zephir中的内存管理是由编译器自动进行的，所以你
不必像使用C语言一样自己分配与释放内存。

.. _zaefire: http://translate.google.com/#en/en/zaefire
