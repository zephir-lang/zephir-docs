Introducing Zephir
==================
Zephir is a language that addresses the major needs of a PHP developer trying to write and compile code that
can be executed by PHP. It is a dynamically/statically typed, some of its features can be familiar to
PHP developers.

The name Zephir is a contraction of the words Zend Engine/PHP/Intermediate. While this suggests that the
pronunciation should be zephyr, the creators of Zephir actually pronounce it zaefire_.

Hello World!
------------
Every language has its own "Hello World!" sample, in Zephir this introductory example showcases some important
features of this language.

Code in Zephir must be placed in classes. This language is intended to create object-oriented libraries/frameworks,
so code out of a class is not allowed. Also, a namespace is required:

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

Once this class is compiled it produce the following code that is transparently compiled by gcc/clang/vc++:

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

Actually, it is not expected that a developer that uses Zephir must know or even understand C,
however, if you have any experience with compilers, php internals or the C language itself,
it would provide a more clear sceneario to the developer when working with Zephir.

A Taste of Zephir
-----------------
In the following examples, we’ll describe just enough of the details, so you understand what’s going on.
The goal is to give you a sense of what programming in Zephir is like. We’ll explore the details of the
features in subsequent chapters.

The following example is very simple, it implements a class and a method with a small program that checks
the types of an array

Let’s examine the code in detail, so we can begin to learn Zephir syntax.
There are a lot of details in just a few lines of code! We’ll explain the general ideas here:

.. code-block:: zephir

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

In the method, the first lines use the 'var' and 'int' keywords are used to declare a variable in the local scope.
Every variable used in a method must be declared with its respective type. This declaration is not optional,
it helps the compiler to report you about mistyped variables or about the use of variables out of their scope
which usually ends in runtime errors.

Dynamic variables are declared with the keyword 'var'. These variables can be assigned and reassigned
to different types. On the other hand, we have 'i' and 'length' integer static typed variables
that can only have values of this type in the entire program execution.

In contrast with PHP, you are not required to put a dollar sign ($) in front of variable names.

Zephir follows the same comment conventions as Java, C#, C++, etc.
A //comment goes to the end of a line, while a /* comment \*/ can cross line boundaries.

Variables are by default immutable, this means that Zephir expects that most variables stay
unchanged. Variables that maintain their initial value can be optimized down by the compiler to static constants.
When the variable value needs to be changed, the keyword 'let' must be used:

.. code-block:: zephir

    /* Create an array */
    let myArray = ["hello", 0, 100.25, false, null];

By default, arrays are dynamic like in PHP, they may contain values of different types.
Functions from the PHP userland can be called in Zephir code, in the example the function 'count'
was called, the compiler can performs optimizations like avoid this call because it already knows the size of
the array:

.. code-block:: zephir

    /* Count the array into a 'int' variable */
    let length = count(myArray);

Parentheses in control flow statements are optional, you can also use them if you feel more confortable.

.. code-block:: zephir

    while i < length {
        echo typeof myArray[i], "\n";
        let i++;
    }

PHP only works with dynamic variables, methods always return dynamic variables, this means that if a
static typed variable is returned, in the PHP side, you will get a dynamic variable that can be used
in PHP code. Note that memory is automatically managed by the compiler, so you don't need to allocate or free
memory like in C, working in a similar way than PHP.

.. _zaefire: http://translate.google.com/#en/en/zaefire
