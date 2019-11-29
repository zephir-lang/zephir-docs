---
layout: default
language: 'es-es'
version: '0.12'
---

# Sintaxis básica

En este capítulo, analizaremos la organización de archivos y espacios de nombres, declaraciones de variables, varios convenios de sintaxis y algunos conceptos generales.

<a name='organizing-code-in-files-and-namespaces'></a>

## Organización del código en archivos y espacios de nombres

En PHP, usted puede colocar código en cualquier archivo, sin una estructura específica. En Zephir, cada archivo debe contener una sola clase. Cada clase debe tener un espacio de nombres, y la estructura de directorios debe coincidir con los nombres de las clases y espacios de nombres utilizados. (Esto es similar al convenio PSR-4 de carga automática, salvo que se aplica al lenguaje en sí mismo)

Por ejemplo, teniendo en cuenta la siguiente estructura, las clases en cada archivo deben ser:

```bash
mylibrary/
    router/
        exception.zep # MyLibrary\Router\Exception
    router.zep # MyLibrary\Router
```

La clase en `mylibrary/router.zep`:

```zephir
namespace MyLibrary;

class Router
{

}
```

La clase en `mylibrary/router/exception.zep`:

```zephir
namespace MyLibrary\Router;

class Exception extends \Exception
{

}
```

Zephir generará una excepción del compilador si un archivo o una clase no se encuentra en el archivo esperado, o viceversa.

<a name='instruction-separation'></a>

## Separación de instrucciones

Puede que ya ha notado, que había muy pocos puntos y comas en los ejemplos de código en el capítulo anterior. Usted puede utilizar puntos y comas para separar declaraciones y expresiones, como en Java, C/C++, PHP, y otros lenguajes similares:

```zephir
myObject->myMethod(1, 2, 3); echo "mundo";
```

<a name='comments'></a>

## Comentarios

Zephir soporta los comentarios de tipo 'C'/'C++'. Estos son los comentarios de una linea `// ...`, y los comentarios multi linea`/* ... */`:

```zephir
// este es un comentario de una linea

/**
 * este es un comentario multi linea
 */
```

En la mayoría de los lenguajes, los comentarios son simplemente ignorados por el compilador/interprete. En Zephir, los comentarios multi linea son utilizados como bloques de documentación (docblocks), y son exportados al código generado, por lo que son parte del lenguaje!

Si un bloque de comentarios no esta ubicado donde sea esperado, el compilador lanzará una excepción.

<a name='variable-declarations'></a>

## Declaración de variables

En Zephir, se deben declarar todas las variables utilizadas en un determinado ámbito. Esto proporciona información importante para que el compilador para realizar optimizaciones y validaciones. Las variables deben ser identificadores únicos y no pueden ser palabras reservadas.

```zephir
// Declarando variables del mismo tipo en la misma instrucción
var a, b, c;

// Declarando cada variable en lineas separadas
var a;
var b;
var c;
```

Las variables pueden tener opcionalmente un valor inicial por defecto compatible con el tipo:

```zephir
// Declarando variables con valores por defecto
var a = "hola", b = 0, c = 1.0;
int d = 50; bool some = true;
```

Los nombres de variables son sensibles a mayúsculas y minúsculas, las siguientes variables son diferentes:

```zephir
// Variables diferentes
var somevalue, someValue, SomeValue;
```

<a name='variable-scope'></a>

## Ámbito de la variable

Todas las variables declaradas localmente, tienen un ámbito local al método donde se declaró:

```zephir
namespace Test;

class MyClass
{
    public function someMethod1()
    {
        int a = 1, b = 2;
        return a + b;
    }

    public function someMethod2()
    {
        int a = 3, b = 4;
        return a + b;
    }
}
```

<a name='super-global'></a>

## Super globales

Zephir no admite variables globales; no se permite el acceso a variables globales desde la zona de usuario de PHP. Sin embargo, se puede acceder a las super globales de PHP de la siguiente forma:

```zephir
// Obteniendo un valor desde _POST
let price = _POST["price"];

// Leer un valor de _SERVER
let requestMethod = _SERVER["REQUEST_METHOD"];
```

<a name='local-symbol-table'></a>

## Tabla de símbolos locales

Cada método o contexto en PHP tiene una tabla de símbolos que le permite escribir variables de una manera muy dinámica:

```php
<?php

$b = 100;
$a = "b";
echo $$a; // imprime 100
```

Zephir no implementa esta característica, ya que todas las variables se compilan en variables de bajo nivel y no hay forma de saber qué variables existen en un contexto específico. Si deseas crear una variable en la tabla de símbolos actual de PHP, puedes utilizar la siguiente sintaxis:

```zephir
// Establecer la variable $name en PHP
let {"name"} = "hola";

// Establecer la variable $precio en PHP
let name = "precio";
let {name} = 10.2;
```
