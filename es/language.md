# Sintaxis básica

En este capítulo, analizaremos la organización de archivos y espacios de nombres, declaraciones de variables, varios convenios de sintaxis y algunos otros conceptos generales.

<a name='organizing-code-in-files-and-namespaces'></a>

## Organización del código en archivos y espacios de nombres

En PHP, usted puede colocar código en cualquier archivo, sin una estructura específica. En Zephir, cada archivo debe contener una clase (y sólo una). Cada clase debe tener un espacio de nombres, y la estructura de directorios debe coincidir con los nombres de las clases y espacios de nombres utilizados. (Esto es similar al convenio PSR-4 de carga automática, salvo que se aplica al lenguaje en sí mismo)

Por ejemplo, teniendo en cuenta la siguiente estructura, las clases en cada archivo deben ser:

    mylibrary/
        router/
            exception.zep # MyLibrary\Router\Exception
        router.zep # MyLibrary\Router
    

La clase en `mylibrary/router.zep`:

    namespace MyLibrary;
    
    class Router
    {
    
    }
    

Class in `mylibrary/router/exception.zep`:

    namespace MyLibrary\Router;
    
    class Exception extends \Exception
    {
    
    }
    

Zephir will raise a compiler exception if a file or class is not located in the expected file, or vice versa.

<a name='instruction-separation'></a>

## Instruction separation

You may have already noticed that there were very few semicolons in the code examples in the previous chapter. You can use semicolons to separate statements and expressions, as in Java, C/C++, PHP, and similar languages:

    myObject->myMethod(1, 2, 3); echo "world";
    

<a name='comments'></a>

## Comments

Zephir supports 'C'/'C++' comments. These are one line comments with `// ...`, and multi line comments with `/* ... */`:

    // this is a one line comment
    
    /**
     * multi-line comment
     */
    

In most languages, comments are simply text ignored by the compiler/interpreter. In Zephir, multi-line comments are also used as docblocks, and they're exported to the generated code, so they're part of the language!

If a docblock is not located where it is expected, the compiler will throw an exception.

<a name='variable-declarations'></a>

## Variable Declarations

In Zephir, all variables used in a given scope must be declared. This gives important information to the compiler to perform optimizations and validations. Variables must be unique identifiers, and they cannot be reserved words.

    // Declaring variables for the same type    in the same instruction
    var a, b, c;
    
    // Declaring each variable in separate lines
    var a;
    var b;
    var c;
    

Variables can optionally have an initial compatible default value:

    // Declaring variables with default values
    var a = "hello", b = 0, c = 1.0;
    int d = 50; bool some = true;
    

Variable names are case-sensitive, the following variables are different:

    // Different variables
    var somevalue, someValue, SomeValue;
    

<a name='variable-scope'></a>

## Variable Scope

All variables declared are locally scoped to the method where they were declared:

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
    

<a name='super-global'></a>

## Super Globals

Zephir does not support global variables - accessing global variables from the PHP userland is not allowed. However, you can access PHP's super-globals as follows:

    // Getting a value from _POST
    let price = _POST["price"];
    
    // Read a value from _SERVER
    let requestMethod = _SERVER["REQUEST_METHOD"];
    

<a name='local-symbol-table'></a>

## Local Symbol Table

Every method or context in PHP has a symbol table that allows you to write variables in a very dynamic way:

    <?php
    
    $b = 100;
    $a = "b";
    echo $$a; // prints 100
    

Zephir does not implement this feature, since all variables are compiled down to low-level variables, and there is no way to know which variables exist in a specific context. If you want to create a variable in the current PHP symbol table, you can use the following syntax:

    // Set variable $name in PHP
    let {"name"} = "hello";
    
    // Set variable $price in PHP
    let name = "price";
    let {name} = 10.2;