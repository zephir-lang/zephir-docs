# Представляємо Zephir

Zephir - мова, яка відповідає основним потребам розробника PHP, який намагається написати та скомпілювати код, який може бути виконаний з-під PHP. Він має динамічну/статичну типізацію і деякі його функції будуть знайомі PHP-розробникам.

Zephir це скорочення слів Z(end) E(ngine)/PH(P)/I(nte)r(mediate). While this suggests that the pronunciation should be "zephyr", the creators of Zephir actually pronounce it [zaefire](http://translate.google.com/#en/en/zaefire).

<a name='hello-world'></a>

## Привіт світ!

Кожна мова власний приклад написання «Привіт світ» програми. У цьому вступі продемонстровано деякі важливі особливості мови.

Код в Zephir повинен розміщатися в класах. Мова призначена для створення об'єктно-орієнтованих бібліотек/фреймворків, тому код за межами класу не допускається. Крім того, необхідно вказувати простір імен:

    namespace Test;
    
    /**
     * Це зразок класу
     */
    class Hello
    {
        /**
         * Це зразок методу
         */
        public function say()
        {
            echo "Привіт світ!";
        }
    }
    

Після компіляції код матиме вигляд, наведений нижче. Такий код без перешкод буде скомпільований за допомогою gcc/clang/vc++:

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif
    
    #include "php.h"
    #include "php_test.h"
    #include "test.h"
    
    #include "kernel/main.h"
    
    /**
     * Це зразок класу
     */
    ZEPHIR_INIT_CLASS(Test_Hello) {
        ZEPHIR_REGISTER_CLASS(Test, Hello, hello, test_hello_method_entry, 0);
        return SUCCESS;
    }
    
    /**
     * Це зразок методу
     */
    PHP_METHOD(Test_Hello, say) {
        php_printf("%s", "Привіт світ!");
    }
    

Насправді, розробник, який використовує Zephir, не повинен знати або навіть зрозуміти C. Однак, якщо у вас є досвід роботи з компіляторами, нутрощами PHP або самою мовою С, це дасть змогу чіткіше зрозуміти, як працює Zephir з середини.

<a name='a-taste-of-zephir'></a>

## Смак Зефіру

У наступних прикладах ми опишемо лише частину деталей, щоб зрозуміти, що відбувається. Задум в тому, щоб показати вам, на що схоже програмування на Zephir. Детальніші *деталі* ми розберемо в наступних розділах.

Наступний приклад дуже простий; він реалізує клас і метод, який є маленькою програмою, що перевіряє типи масиву.

Розгляньмо детальніше цей код, щоб ви могли почати вивчати синтаксис Zephir. Ці рядки містять багацько деталей! Ми пояснимо загальні ідеї:

    namespace Test;
    
    /**
     * MyTest (test/mytest.zep)
     */
    class MyTest
    {
        public function someMethod()
        {
            /* Змінні мають бути оголошені */
            var myArray;
            int i = 0, length;
    
            /* Створюємо масив */
            let myArray = ["hello", 0, 100.25, false, null];
    
            /* Рахуємо довжину масиву в змінні типу 'int' (ціле число) */
            let length = count(myArray);
    
            /* Друкуємо значення */
            while i < length {
                echo typeof myArray[i], "\n";
                let i++;
            }
    
            return myArray;
        }
    }
    

Перший рядок у цьому методі містить ключові слова `var` та `int`. Вони використовуються для оголошення змінної в локальній області. Кожна змінна, яка використовується в методі, повинна бути оголошена з відповідним типом. This declaration is not optional - it helps the compiler warn you about mistyped variables, or about the use of variables out of scope, which usually ends in runtime errors.

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