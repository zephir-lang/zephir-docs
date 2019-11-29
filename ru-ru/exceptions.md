---
layout: default
language: 'ru-ru'
version: '0.10'
---

# Исключения

Zephir реализует исключения на очень низком уровне, обеспечивая подобное PHP поведение и функциональность.

Когда генерируется исключение, блок `catch` может быть использован для перехвата исключения и предоставления разработчику возможности обеспечить надлежащую обработку:

Исключения могут быть выброшены внутри блока `try`. Обработка происходит в блоке `catch` точно так же, как в PHP:

```zephir
var e;
try {

    throw new \Exception("Это исключение");

} catch \Exception, e {

    echo e->getMessage();
}
```

Zephir также предоставляет "тихий" блок `try`, который просто игнорирует любые исключения, генерируемые внутри этого блока:

```zephir
try {
    throw new \Exception("Это исключение");
}
```

Если вам не нужна переменная исключения при его перехвате, вы можете не указать её:

```zephir
try {

    throw new \Exception("Это исключение");

} catch \Exception {

    echo "An exception occur!";
}
```

Один `catch` блок может использоваться для перехвата нескольких типов исключений:

```zephir
var e;
try {

    throw new \Exception("Это исключение");

} catch \RuntimeException|\Exception, e {

    echo e->getMessage();
}
```

Zephir позволяет выбрасывать любые литеры или статически типизированные переменные, как если бы они были сообщением исключения:

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
