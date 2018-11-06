# Zephir 简介

Zephir 是一种语言, 它能够满足 php 开发人员的主要需求, 他们试图编写和编译可由 php 执行的代码。 它是动态的/静态类型的, 它的一些功能为 php 开发人员所熟悉。

Zephir 这个名字是单词 z (end) e (ngine)/ph (p)/i (nte) r (mediate) 的收缩。 虽然这表明 </div> 发音的 "div class="notranslate">0 应该是" zephyr ", 但 zefir 的创作者实际上 [zaefire](http://translate.google.com/#en/en/zaefire) 发音。

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

让我们详细检查代码，以便开始学习Zephir语法。 There are a lot of details in just a few lines of code! 我们将在这里解释其大意:

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
    

在该方法中，第一行使用`var`和`int`关键字。 用于在局部作用域中声明一个变量。 方法中使用的每个变量都必须使用其各自的类型声明。 This declaration is not optional - it helps the compiler warn you about mistyped variables, or about the use of variables out of scope, which usually ends in runtime errors.

Dynamic variables are declared with the keyword `var`. These variables can be assigned and reassigned to different types. On the other hand, the `i` and `length` variables are statically typed integer variables, that can only have integer values in the entire program execution.

In contrast with PHP, you are not required to put a dollar sign ($) in front of variable names.

Zephir follows the same comment conventions as Java, C#, C++, etc. A `// comment` goes to the end of a line, while a `/* comment */` can cross line boundaries.

Variables are, by default, immutable. This means that Zephir expects that most variables will stay unchanged. Variables that maintain their initial value can be optimized down by the compiler to static constants. When the variable value needs to be changed, the keyword `let` must be used:

    /* Create an array */
    let myArray = ["hello", 0, 100.25, false, null];
    

By default, arrays are dynamically typed like in PHP - they may contain values of different types. Functions from the PHP userland can be called in Zephir code. In the next example, the function `count` is called, but the compiler can perform optimizations like avoiding this call, because it already knows the size of the array:

    /* Count the array into a 'int' variable */
    let length = count(myArray);
    

Parentheses in control flow statements are optional. You can use them if you feel more comfortable doing so, but you aren't required to.

    while i < length {
        echo typeof myArray[i], "\n";
        let i++;
    }
    

Since PHP only works with dynamic variables, methods always return dynamic variables. This means that if a statically typed variable is returned, in the PHP side you will get a dynamic variable that can be used in PHP code. Note that memory is automatically managed by the compiler, similarly to how PHP does it, so you don't need to allocate or free memory like in C.