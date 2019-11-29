---
layout: default
language: 'en'
version: '0.12'
---

# Exceptions

Zephir implements exceptions at a very low level, providing similar behavior and functionality to PHP.

When an exception is thrown, a `catch` block can be used to capture the exception and allow the developer to provide proper handling:

Exceptions can be thrown inside the `try` block. Handling happens in the `catch` block, exactly as in PHP:

```zephir
var e;
try {

    throw new \Exception("This is an exception");

} catch \Exception, e {

    echo e->getMessage();
}
```

Zephir also provides a "silent" `try` block, that simply ignores any exceptions produced within that block:

```zephir
try {
    throw new \Exception("This is an exception");
}
```

If you don't need an exception variable when 'catch'ing, then you can safely not provide it:

```zephir
try {

    throw new \Exception("This is an exception");

} catch \Exception {

    echo "An exception occur!";
}
```

A single `catch` block can be used to catch multiple types of exception:

```zephir
var e;
try {

    throw new \Exception("This is an exception");

} catch \RuntimeException|\Exception, e {

    echo e->getMessage();
}
```

Zephir allows you to throw literals or static typed variables as if they were the message of the exception:

```zephir
// throw new \Exception("Тест");
throw "Тест";

// throw new \Exception((string) 't');
throw 't';

// throw new \Exception((string) 123);
throw 123;

// throw new \Exception((string) 123.123);
throw 123.123;
```

Исключения Zephir предоставляют те же методы, что и исключения PHP, чтобы узнать, где произошло исключение. Таким образом, `Exception::getFile()` и ` Exception::getLine()` возвращают имя файла и номер строки соотвественно, где в Zephir коде было выброшено исключение:

```bash
Exception: The static method 'someMethod' does not exist on model 'Robots'
File=phalcon/mvc/model.zep Line=4042
#0 /home/scott/test.php(64): Phalcon\Mvc\Model::__callStatic('someMethod', Array)
#1 /home/scott/test.php(64): Robots::someMethod()
#2 {main}
```