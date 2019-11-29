---
layout: default
language: 'en'
version: '0.12'
---

# Introducción a Zephir

Zephir es un lenguaje que aborda las principales necesidades de un desarrollador PHP tratando de escribir y compilar código que pueda ser ejecutado por PHP. Es de tipificado dinámico/estático, y algunas de sus características serán familiares para los desarrolladores PHP.

El nombre Zephir es una contracción de las palabras Z(end) E(ngine)/PH(P)/I(nte)r(mediate). While this suggests that the pronunciation should be "zephyr", the creators of Zephir actually pronounce it [zaefire](http://translate.google.com/#en/en/zaefire).

<a name='hello-world'></a>

## ¡Hola Mundo!

Cada lenguaje tiene su propio ejemplo "¡Hola mundo!". En Zephir, este ejemplo introductorio presenta algunas características importantes del lenguaje.

El código en Zephir se debe colocar en clases. El lenguaje está diseñado para crear librerías de orientadas a objetos y frameworks, por lo que no se permite el código fuera de una clase. Además, se requiere un espacio de nombres:

```zephir
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
```

Una vez que se compila esta clase va a producir el código siguiente, que es compilado de forma transparente por gcc/clang/vc++:

```c
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
```

Actualmente, no se espera que un desarrollador que utiliza Zephir deba saber o entender C. Sin embargo, si usted tiene alguna experiencia con compiladores, con el funcionamiento interno PHP o el lenguaje C, le proporcionará una comprensión más clara de lo que está sucediendo internamente cuando se trabaja con Zephir.

<a name='a-taste-of-zephir'></a>

## Una prueba de Zephir

En los siguientes ejemplos, describiremos los detalles suficientes para entender lo que está sucediendo. El objetivo es darle un sentido de como es la programación en Zephir. Exploraremos los *detalles* de las características en los capítulos siguientes.

En el siguiente ejemplo es muy simple; implementa una clase y un método, con un pequeño programa que verifica los tipos de datos de una matriz.

Examinemos el código detalladamente, así podemos empezar a aprender la sintaxis de Zephir. ¡Hay un montón de detalles en unas pocas líneas de código! Vamos a explicar aquí las ideas generales:

```zephir
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
```

En el método, las primeras líneas utilizan las palabras clave `var` e `int`. Se utilizan para declarar una variable en el ámbito local. Cada variable utilizada en un método debe declararse con su tipo respectivo. Esta declaración no es opcional, ayuda el compilador a advertirle sobre variables mal escritas, o sobre el uso de variables fuera de ámbito, que generalmente termina en errores en tiempo de ejecución.

Las variables dinámicas son declaradas con la palabra clave `var`. Estas variables pueden ser asignadas o reasignadas a tipos diferentes. Por otro lado, las variables `i` y `length` son estrictamente tipificadas a un entero, estas solo pueden tener valores enteros en la ejecución de todo el programa.

En contraste con PHP, no necesitas colocar el signo de dolar ($) como prefijo de nombre de variable.

Zephir permite las mismas convenciones de comentarios como en Java, C#, C++, etc. Un `// comentario` al final de una linea, con un `/* comentario */` puedes crear comentarios multi linea.

Las variables, por defecto, son inmutables. Esto significa que Zephir espera que la mayoría de las variables se mantengan sin cambios. Las variables que mantienen su valor inicial pueden ser optimizadas por el compilador a constantes estáticas. Cuando una variable necesita ser modificada, la palabra reservada `let` debe ser utilizada:

```zephir
/* Crear un array */
let myArray = ["hello", 0, 100.25, false, null];
```

Por defecto, los arrays son de tipificado dinámico como en PHP, ellos pueden contener valores de distintos tipos. Las funciones de PHP se pueden llamar en el código de Zephir. En el siguiente ejemplo, la función `count` es llamada, pero el compilador puede realizar optimizaciones, como evitar esta llamada, porque ya conoce el tamaño del arreglo:

```zephir
/* Medir un array a una variable 'int' */
let length = count(myArray);
```

Los paréntesis en las declaraciones de control de flujo son opcionales. Puede utilizarlos si se siente más cómodo, pero no son obligatorios.

```zephir
while i < length {
    echo typeof myArray[i], "\n";
    let i++;
}
```

Dado que PHP solo funciona con variables dinámicas, los métodos siempre retornan variables dinámicas. Esto significa que si se devuelve una variable de tipo estático, en el lado de PHP obtendrá una variable dinámica que se puede utilizar en el código PHP. Note que la memoria es manejada automáticamente por el compilador, al igual que lo hace PHP, entonces usted no necesita asignar o liberar memoria como en C.