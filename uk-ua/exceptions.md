---
layout: default
language: 'uk-ua'
version: '0.10'
---

# Винятки
Zephir реалізує винятки на дуже низькому рівні, забезпечуючи схожу поведінку і функціональність як у PHP.

Коли генерується виняток, блок `catch` може бути використаний для перехоплення виключення і надання розробнику можливості забезпечити належну обробку:

Винятки можуть бути викинуті всередині блоку `try`. Обробка виконується в блоці `catch`, як і в PHP:

```zephir
var e;
try {

    throw new \Exception("This is an exception");

} catch \Exception, e {

    echo e->getMessage();
}
```

Zephir також забезпечує "тихий" блок `try`, який просто ігнорує будь-які винятки всередині цього блоку:

```zephir
try {
    throw new \Exception("This is an exception");
}
```

Якщо вам не потрібна змінна виключення в блоці `catch`, ви можете не вказувати її:

```zephir
try {

    throw new \Exception("This is an exception");

} catch \Exception {

    echo "An exception occur!";
}
```

Один блок `catch` може використовуватися для перехоплення декількох типів винятків:

```zephir
var e;
try {

    throw new \Exception("This is an exception");

} catch \RuntimeException|\Exception, e {

    echo e->getMessage();
}
```

Zephir дозволяє викидати літерали або статичнотипізовані змінні так, ніби вони є повідомленням про виняток:

```zephir
// throw new \Exception("Test");
throw "Test";

// throw new \Exception((string) 't');
throw 't';

// throw new \Exception((string) 123);
throw 123;

// throw new \Exception((string) 123.123);
throw 123.123;
```

Винятки в Zephir надають ті самі методи, що й методи в PHP, для того, щоб дізнатися де сталася помилка. Тобто, `Exception::getFile()` та `Exception::getLine()` повертають ім'я файлу та номер рядка де в Zephir-коді був згенерований виняток:

```bash
Exception: The static method 'someMethod' does not exist on model 'Robots'
File=phalcon/mvc/model.zep Line=4042
#0 /home/scott/test.php(64): Phalcon\Mvc\Model::__callStatic('someMethod', Array)
#1 /home/scott/test.php(64): Robots::someMethod()
#2 {main}
```
