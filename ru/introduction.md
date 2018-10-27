# Введение в Zephir

Zephir — это язык, который обращается к основным потребностям разработчика PHP, пытающегося писать и компилировать код, который может быть выполнен PHP. Он динамически/статически типизирован, некоторые его функции могут быть знакомы разработчикам PHP.

Название Zephir является сокращением слов Z(end) E(ngine)/PH(P)/I(nte)r(mediate). Хотя это говорит о том, что произношение должно быть зефир, создатели Zephir фактически произносят его [zaefire](http://translate.google.com/#en/en/zaefire).

<a name='hello-world'></a>

## Привет мир!

Каждый язык имеет свой собственный пример "Привет мир!". В Zephir, этот вводный пример демонстрирует некоторые важные особенности этого языка.

Код в Zephir должен быть помещены в классы. Этот язык предназначен для создания объектно-ориентированных библиотек/фреймворков, поэтому использование кода без класса за очень редкими исключениями не допускается. Кроме того, наличие пространства имён является обязательным требованием:

    namespace Test;
    
    /**
     * Это пример класса
     */
    class Hello
    {
        /**
         * Это пример метода
         */
        public function say()
        {
            echo "Привет мир!";
        }
    }
    

После компиляции этого класса он создает следующий код, который прозрачно компилируется gcc/clang/vc++:

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif
    
    #include "php.h"
    #include "php_test.h"
    #include "test.h"
    
    #include "kernel/main.h"
    
    /**
     * Это пример класса
     */
    ZEPHIR_INIT_CLASS(Test_Hello) {
        ZEPHIR_REGISTER_CLASS(Test, Hello, hello, test_hello_method_entry, 0);
        return SUCCESS;
    }
    
    /**
     * Это пример метода
     */
    PHP_METHOD(Test_Hello, say) {
        php_printf("%s", "Привет мир!");
    }
    

На самом деле, не ожидается, что разработчик, который использует Zephir должен знать или даже понимать C, однако, если у вас есть опыт работы с компиляторами, PHP внутренностей или самого языка C, это обеспечит более чёткий сценарий разработчику при работе с Zephir.

<a name='a-taste-of-zephir'></a>

## Понимание Zephir

В следующих примерах мы опишем достаточно подробностей, чтобы вы поняли, что происходит. Цель состоит в том, чтобы дать вам понять, что такое программирование в Zephir. Мы рассмотрим *детали* функций в последующих главах.

Следующий пример очень прост, он реализует класс и метод с небольшой программой, которая проверяет типы массива.

Рассмотрим код подробно, чтобы мы могли начать изучать синтаксис Zephir. В нескольких строках кода много деталей! Мы объясним общие идеи здесь:

    namespace Test;
    
    /**
     * MyTest (test/mytest.zep)
     */
    class MyTest
    {
        public function someMethod()
        {
            /* Переменные должны быть объявлены */
            var myArray;
            int i = 0, length;
    
            /* Создать массив */
            let myArray = ["hello", 0, 100.25, false, null];
    
            /* Подсчитать массив в переменную типа int */
            let length = count(myArray);
    
            /* Вывод типов переменных */
            while i < length {
                echo typeof myArray[i], "\n";
                let i++;
            }
    
            return myArray;
        }
    }
    

В методе в первых строках ключевые слова `var` и `int`. Этот синтаксис используется для объявления переменной в локальной области. Каждая переменная, используемая в методе, должна быть объявлена с соответствующим типом. Эта декларация не является обязательным, это помогает компилятору сообщать вам о неверном вводе переменных или об использовании переменных из сферы их применения, который обычно заканчивается ошибки во время выполнения.

Dynamic variables are declared with the keyword `var`. These variables can be assigned and reassigned to different types. On the other hand, the `int` variables are statically typed integer variables, that can only have integer values in the entire program execution.

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