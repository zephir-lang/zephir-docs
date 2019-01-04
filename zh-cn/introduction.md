---
layout: default
language: 'zh-cn'
version: '0.11'
menu:
  - text:
        'Hello World!'
    url: '#hello-world'
  - text:
        '一个 Zephir 的尝试'
    url: '#a-taste-of-zephir' 
---
# Zephir 简介

Zephir 是一种语言, 它能够满足 php 开发人员的主要需求, 他们试图编写和编译可由 php 执行的代码。 它是动态的/静态类型的, 它的一些功能为 php 开发人员所熟悉。

Zephir 这个名字是单词 z (end) e (ngine)/ph (p)/i (nte) r (mediate) 的收缩。 这表明 发音应该是“zephyr”，Zephir的创建者实际上是把它发音为[zaefire](http://translate.google.com/#en/en/zaefire)。

<a name='hello-world'></a>

## Hello World!

每种语言都有自己的 "Hello World!" 示例。 在 Zephir 中, 这个介绍性示例展示了该语言的一 些重要功能。

Zephir 中的代码必须放在类中。 该语言旨在创建面向对象的库框架, 因此不允许在类之外使用代码。 此外, 还需要命名空间:

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
    

编译此类后, 它将生成以下代码, 该代码由 gcc/clang/vc ++ 透明地编译:

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
    

实际上, 使用 Zephir 的开发人员必须知道甚至理解 c, 这是不希望的。但是, 如果您对编译器、php 内部或 c 语言本身有任何经验, 这将使您更清楚地了解在使用 Zephir 时内部发生的情况。

<a name='a-taste-of-zephir'></a>

## 一个 Zephir 的尝试

在下面的例子中，我们将描述足够多的细节来理解发生了什么。 目标是让您了解Zephir中的编程是什么样子的。 我们将在后面的章节中探索*细节*的特性。

下面的例子很简单; 它实现了一个类和一个方法，以及一个检查数组类型的小程序。

让我们详细检查代码，以便开始学习Zephir语法。 在短短几行代码中有很多细节! 我们将在这里解释其大意:

    namespace Test;
    
    /**
     * MyTest (test/mytest.zep)
     */
    class MyTest
    {
        public function someMethod()
        {
            /* Variables must be declared */
            var myArray;
            int i = 0, length;
    
            /* Create an array */
            let myArray = ["hello", 0, 100.25, false, null];
    
            /* Count the array into a 'int' variable */
            let length = count(myArray);
    
            /* Print value types */
            while i < length {
                echo typeof myArray[i], "\n";
                let i++;
            }
    
            return myArray;
        }
    }
    

在该方法中，第一行使用`var`和`int`关键字。 用于在局部作用域中声明一个变量。 方法中使用的每个变量都必须使用其各自的类型声明。 这个声明不是可选的——它帮助编译器警告您输入错误的变量，或者警告您使用超出范围的变量，这通常会导致运行时错误。

使用关键字`var`声明动态变量。 可以将这些变量分配并重新分配给不同的类型。 另一方面, `i` 和 `length` 变量是静态类型的整数变量, 在整个程序执行中只能有整数值。

与 php 不同的是, 你不需要在变量名前面放一个美元符号 ($)。

Zephir 遵循与 java、c#、c++ 等相同的注释约定。 `//评论 </0 > 到了一行的末尾, 而 <code>/* 注释 */` 可以跨越线边界。

默认情况下, 变量是不可变的。 这意味着 Zephir 预计大多数变量将保持不变。 保持其初始值的变量可以由编译器优化为静态常量。 当需要更改变量值时, 必须使用关键字 `let`:

    /* Create an array */
    let myArray = ["hello", 0, 100.25, false, null];
    

默认情况下, 数组是动态类型的, 就像 php 中一样-它们可能包含不同类型的值。 PHP的函数可以在Zephir代码中调用。 在下一个示例中, 调用函数 `count`, 但编译器可以执行优化, 如避免此调用, 因为它已经知道数组的大小:

    /* Count the array into a 'int' variable */
    let length = count(myArray);
    

控件流语句中的括号是可选的。 如果你觉得这样做更舒服，你可以使用它们，但不是必须的。

    while i < length {
        echo typeof myArray[i], "\n";
        let i++;
    }
    

由于PHP只处理动态变量，因此方法总是返回动态变量。 这意味着如果返回静态类型的变量，在PHP端您将得到一个可用于PHP代码的动态变量。 注意，内存是由编译器自动管理的，类似于PHP的管理方式，所以您不需要像在C中那样分配或释放内存。
