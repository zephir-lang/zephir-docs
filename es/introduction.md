# Introducción a Zephir

Zephir es un lenguaje que aborda las principales necesidades de un desarrollador PHP tratando de escribir y compilar código que pueda ser ejecutado por PHP. Es de tipificado dinámico/estático, y algunas de sus características serán familiares para los desarrolladores PHP.

El nombre Zephir es una contracción de las palabras Z(end) E(ngine)/PH(P)/I(nte)r(mediate). Mientras que esto sugiere que la pronunciación debería ser "zephyr", los creadores de Zephir realmente pronuncian [zaefire](http://translate.google.com/#en/en/zaefire).

<a name='hello-world'></a>

## ¡Hola Mundo!

Cada lenguaje tiene su propio ejemplo "¡Hola mundo!". En Zephir, este ejemplo introductorio presenta algunas características importantes del lenguaje.

El código en Zephir se debe colocar en clases. El lenguaje está diseñado para crear librerías de orientadas a objetos y frameworks, por lo que no se permite el código fuera de una clase. Además, se requiere un espacio de nombres:

    namespace Test;
    
    /**
     * Esta es una clase de ejemplo
     */
    class Hello
    {
        /**
         * Este es un método de ejemplo
         */
        public function say()
        {
            echo "¡Hola Mundo!";
        }
    }
    

Una vez que se compila esta clase va a producir el código siguiente, que es compilado de forma transparente por gcc/clang/vc++:

    #ifdef HAVE_CONFIG_H
    #include "config.h"
    #endif
    
    #include "php.h"
    #include "php_test.h"
    #include "test.h"
    
    #include "kernel/main.h"
    
    /**
     * Esta es una clase de ejemplo
     */
    ZEPHIR_INIT_CLASS(Test_Hello) {
        ZEPHIR_REGISTER_CLASS(Test, Hello, hello, test_hello_method_entry, 0);
        return SUCCESS;
    }
    
    /**
     * Este es un método de ejemplo
     */
    PHP_METHOD(Test_Hello, say) {
        php_printf("%s", "¡Hola Mundo!");
    }
    

Actualmente, no se espera que un desarrollador que utiliza Zephir deba saber o entender C. Sin embargo, si usted tiene alguna experiencia con compiladores, con el funcionamiento interno PHP o el lenguaje C, le proporcionará una comprensión más clara de lo que está sucediendo internamente cuando se trabaja con Zephir.

<a name='a-taste-of-zephir'></a>

## Una prueba de Zephir

En los siguientes ejemplos, describiremos los detalles suficientes para entender lo que está sucediendo. El objetivo es darle un sentido de como es la programación en Zephir. Exploraremos los *detalles* de las características de los capítulos siguientes.

En el siguiente ejemplo es muy simple; implementa una clase y un método, con un pequeño programa que verifica los tipos de datos de una matriz.

Examinemos el código detalladamente, así podemos empezar a aprender la sintaxis de Zephir. ¡Hay un montón de detalles en unas pocas líneas de código! Vamos a explicar aquí las ideas generales:

    namespace Test;
    
    /**
     * MyTest (test/mytest.zep)
     */
    class MyTest
    {
        public function someMethod()
        {
            /* Las variables deben ser declaradas */
            var myArray;
            int i = 0, length;
    
            /* Crear un array */
            let myArray = ["hello", 0, 100.25, false, null];
    
            /* Medir un array en una variable 'int' */
            let length = count(myArray);
    
            /* Imprimir los tipos de los valores */
            while i < length {
                echo typeof myArray[i], "\n";
                let i++;
            }
    
            return myArray;
        }
    }
    

En el método, las primeras líneas utilizan las palabras clave `var` e `int`. There are used to declare a variable in the local scope. Every variable used in a method must be declared with its respective type. This declaration is not optional - it helps the compiler warn you about mistyped variables, or about the use of variables out of scope, which usually ends in runtime errors.

Dynamic variables are declared with the keyword `var`. These variables can be assigned and reassigned to different types. On the other hand, the `int` variables are statically typed integer variables, that can only have integer values in the entire program execution.

In contrast with PHP, you are not required to put a dollar sign ($) in front of variable names.

Zephir follows the same comment conventions as Java, C#, C++, etc. A `// comment` goes to the end of a line, while a `/* comment */` can cross line boundaries.

Variables are, by default, immutable. This means that Zephir expects that most variables will stay unchanged. Variables that maintain their initial value can be optimized down by the compiler to static constants. When the variable value needs to be changed, the keyword `let` must be used:

    /* Crear un array */
    let myArray = ["hello", 0, 100.25, false, null];
    

By default, arrays are dynamically typed like in PHP - they may contain values of different types. Functions from the PHP userland can be called in Zephir code. In the next example, the function `count` is called, but the compiler can perform optimizations like avoiding this call, because it already knows the size of the array:

    /* Medir un array a una variable 'int' */
    let length = count(myArray);
    

Parentheses in control flow statements are optional. You can use them if you feel more comfortable doing so, but you aren't required to.

    while i < length {
        echo typeof myArray[i], "\n";
        let i++;
    }
    

Since PHP only works with dynamic variables, methods always return dynamic variables. This means that if a statically typed variable is returned, in the PHP side you will get a dynamic variable that can be used in PHP code. Note that memory is automatically managed by the compiler, similarly to how PHP does it, so you don't need to allocate or free memory like in C.