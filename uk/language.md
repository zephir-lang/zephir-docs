# Базовий синтаксис

У цьому розділі ми обговоримо організацію файлів, простори імен, оголошення змінних, різні синтаксичні конвенції та кілька інших загальних понять.

<a name='organizing-code-in-files-and-namespaces'></a>

## Розміщення коду в файлах та простори імен

У PHP ви можете розмістити код в будь-який файл, без конкретної структури. У Zephir кожен файл мусить містити клас (і тільки один клас). Кожен клас повинен мати простір імен, а структура каталогів повинна відповідати іменам класів та просторам імен. (Це схоже на PSR-4-автозавантажувальну конвенцію, за винятком того, що вона застосовується самою мовою.)

Наприклад, з огляду на наступну структуру кожен файл має мати наступні класи:

    mylibrary/
        router/
            exception.zep # MyLibrary\Router\Exception
        router.zep # MyLibrary\Router
    

Клас у `mylibrary/router.zep`:

    namespace MyLibrary;
    
    class Router
    {
    
    }
    

Клас у `mylibrary/router/exception.zep`:

    namespace MyLibrary\Router;
    
    class Exception extends \Exception
    {
    
    }
    

Zephir викине виняток (exception) компілятора, якщо файл або клас не знаходяться в очікуваному файлі, або навпаки.

<a name='instruction-separation'></a>

## Розділення інструкцій

Можливо, ви вже помітили, що в прикладах коду в попередньому розділі було дуже мало крапок з комою. Ви можете використовувати крапку з комою для відокремлення тверджень та виразів, як у Java, C/C++, PHP та подібних мовах:

    myObject->myMethod(1, 2, 3); echo "world";
    

<a name='comments'></a>

## Коментарі

Zephir підтримує коментарі в стилі 'C'/'C++'. These are one line comments with `// ...`, and multi line comments with `/* ... */`:

    // this is a one line comment
    
    /**
     * multi-line comment
     */
    

In most languages, comments are simply text ignored by the compiler/interpreter. In Zephir, multi-line comments are also used as docblocks, and they're exported to the generated code, so they're part of the language!

If a docblock is not located where it is expected, the compiler will throw an exception.

<a name='variable-declarations'></a>

## Оголошення змінних

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

## Область видимості

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