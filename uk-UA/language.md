---
layout: default
language: 'uk-UA'
version: '0.10'
menu:
  - text:
        'Organizing code in files and namespaces'
    url: '#organizing-code-in-files-and-namespaces'
  - text:
        'Instruction separation'
    url: '#instruction-separation'
  - text:
        'Comments'
    url: '#comments'
  - text:
        'Variable declarations'
    url: '#variable-declarations'
  - text:
        'Variable scope'
    url: '#variable-scope'
  - text:
        'Super globals'
    url: '#super-globals'
  - text:
        'Local symbol table'
    url: '#local-symbol-table'
---
# Basic Syntax

У цьому розділі ми обговоримо організацію файлів, простори імен, оголошення змінних, різні синтаксичні конвенції та кілька інших загальних понять.

<a name='organizing-code-in-files-and-namespaces'></a>

## Organizing Code in Files and Namespaces

У PHP ви можете розмістити код в будь-який файл, без конкретної структури. У Zephir кожен файл мусить містити клас (і тільки один клас). Кожен клас повинен мати простір імен, а структура каталогів повинна відповідати іменам класів та просторам імен. (Це схоже на PSR-4-автозавантажувальну конвенцію, за винятком того, що вона застосовується самою мовою.)

Наприклад, з огляду на наступну структуру кожен файл має мати наступні класи:

```bash
mylibrary/
    router/
        exception.zep # MyLibrary\Router\Exception
    router.zep # MyLibrary\Router
```

Клас у `mylibrary/router.zep`:

```zephir
namespace MyLibrary;

class Router
{

}
```

Клас у `mylibrary/router/exception.zep`:

```zephir
namespace MyLibrary\Router;

class Exception extends \Exception
{

}
```

Zephir викине виняток (exception) компілятора, якщо файл або клас не знаходяться в очікуваному файлі, або навпаки.

<a name='instruction-separation'></a>

## Instruction separation

Можливо, ви вже помітили, що в прикладах коду в попередньому розділі було дуже мало крапок з комою. Ви можете використовувати крапку з комою для відокремлення тверджень та виразів, як у Java, C/C++, PHP та подібних мовах:

```zephir
myObject->myMethod(1, 2, 3); echo "world";
```

<a name='comments'></a>

## Comments

Zephir підтримує коментарі в стилі 'C'/'C++'. Це однорядкові коментарі з `// ...`, та багаторядкові з `/* ... */`:

```zephir
// this is a one line comment

/**
 * multi-line comment
 */
```

У більшості мов коментарі це просто текст, який ігнорується компілятором/інтерпретатором. У Zephir-і багаторядкові коментарі також використовуються як док-блоки (docblocks) і вони експортуються до згенерованого коду, так що вони - частина мови!

Якщо док-блок не знаходиться там, де він очікується, компілятор викине виключення.

<a name='variable-declarations'></a>

## Variable Declarations

У Zephir-і всі змінні, які використовуються в даній області видимості мають бути оголошені. Це дає компілятору можливість виконати оптимізацію та перевірки. Змінні мають бути унікальними ідентифікаторами. Ключові слова не можуть бути іменами змінних.

```zephir
// Declaring variables for the same type    in the same instruction
var a, b, c;

// Declaring each variable in separate lines
var a;
var b;
var c;
```

Змінні можуть мати початкове сумісне значення:

```zephir
// Declaring variables with default values
var a = "hello", b = 0, c = 1.0;
int d = 50; bool some = true;
```

Імена змінних чутливі до регістру, наступні змінні є різними:

```zephir
// Different variables
var somevalue, someValue, SomeValue;
```

<a name='variable-scope'></a>

## Variable Scope

Усі оголошені в методі змінні залишаються в його локальній області видимості:

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

## Super Globals

Zephir не підтримує глобальні змінні - доступу до глобальних змінних з PHP немає. Однак, ви можете отримати доступ до суперглобальних змінних PHP наступним чином:

```zephir
// Getting a value from _POST
let price = _POST["price"];

// Read a value from _SERVER
let requestMethod = _SERVER["REQUEST_METHOD"];
```

<a name='local-symbol-table'></a>

## Local Symbol Table

Кожен метод або контекст у PHP має таблицю символів, яка дозволяє вам записувати змінні у дуже гнучкий спосіб:

```php
<?php

$b = 100;
$a = "b";
echo $$a; // prints 100
```

У Zephir не передбачено реалізації цієї функціональності, тому що всі змінні компілюються до низькорівневих змінних, і немає ніякого способу дізнатися, які змінні існують у специфічному контексті. Якщо ви хочете створити змінну в поточній таблиці символів PHP, використайте такий синтаксис:

```zephir
// Set variable $name in PHP
let {"name"} = "hello";

// Set variable $price in PHP
let name = "price";
let {name} = 10.2;
```
