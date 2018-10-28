# Базовый синтаксис

В этой главе мы обсудим организацию файлов и пространств имен, объявления переменных, разные синтаксические соглашения и несколько других концепций.

<a name='organizing-code-in-files-and-namespaces'></a>

## Организация кода в файлах и пространствах имён

В PHP вы можете поместить код в любой файл без определенной структуры. В Zephir каждый файл должен содержать класс (и только один класс). Каждый класс должен иметь пространство имён, а структура каталогов должна соответствовать именам используемых классов и имен. (Это похоже на соглашение автозагрузки классов PSR-4, за исключением того, что оно обеспечивается за счёт самого языка).

Например, для следующей структуры классы в каждом файле должны быть:

    mylibrary/
        router/
            exception.zep # MyLibrary\Router\Exception
        router.zep # MyLibrary\Router
    

Класс в `mylibrary/router.zep`:

    namespace MyLibrary;
    
    class Router
    {
    
    }
    

Класс в `mylibrary/router/exception.zep`:

    namespace MyLibrary\Router;
    
    class Exception extends \Exception
    {
    
    }
    

Zephir выбрасывает ошибку компиляции, если файл или класс не находится в ожидаемом файле или наоборот.

<a name='instruction-separation'></a>

## Разделение инструкций

Возможно, вы уже заметили, что в примерах кода в предыдущей главе было очень мало точек с запятой. Вы можете использовать точки с запятой для разделения операторов и выражений, как в Java, C / C ++, PHP и подобных языках:

    myObject->myMethod(1, 2, 3); echo "world";
    

<a name='comments'></a>

## Комментарии

Zephir поддерживает комментарии в стиле C и C++. Это однострочные комментарии вида `// ...`, и многострочные комментариями вида `/* ... */`:

    // Это однострочный
    
    /**
     * Многострочный комментарий
     */
    

В большинстве языков комментарии — это просто текст, игнорируемый компилятором/интерпретатором. В Zephir многострочные комментарии также используются в качестве блоков документации (docblock), и они экспортируются в сгенерированный код, поэтому они являются частью языка!

Если блок документации не находится там, где ожидается, компилятор выдаст исключение.

<a name='variable-declarations'></a>

## Объявления переменных

В Zephir все переменные, используемые в заданной области видимости, должны быть объявлены. Этот процесс предоставляет компилятору важную информацию для выполнения оптимизаций и проверок. Переменные должны быть уникальными идентификаторами, и они не могут быть зарезервированными словами.

    // Объявление переменных для одного и того же типа в одной инструкции
    var a, b, c;
    
    // Объявление каждой переменной с новой строки
    var a;
    var b;
    var c;
    

Переменные могут дополнительно иметь начальное совместимое значение по умолчанию:

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