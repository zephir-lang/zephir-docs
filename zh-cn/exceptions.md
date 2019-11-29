---
layout: default
language: 'zh-cn'
version: '0.12'
---

# 例外情况

Zephir以非常低的级别实现异常，为PHP提供类似的行为和功能。

抛出异常时，可以使用` catch `块来捕获异常并允许开发人员提供正确的处理：

可以在`try`语句中抛出异常。 和PHP一样，在`catch`中捕获异常并处理

```zephir
var e;
try {

    throw new \Exception("This is an exception");

} catch \Exception, e {

    echo e->getMessage();
}
```

Zephir 还提供了一个“安静”`try`语句，用来忽略产生的任何异常：

```zephir
try {
    throw new \Exception("This is an exception");
}
```

如果在<0>catch</0>中不处理异常，可以不提供异常变量：

```zephir
try {

    throw new \Exception("This is an exception");

} catch \Exception {

    echo "An exception occur!";
}
```

在`catch`中捕获多个异常：

```zephir
var e;
try {

    throw new \Exception("This is an exception");

} catch \RuntimeException|\Exception, e {

    echo e->getMessage();
}
```

Zephir允许您抛出文字或静态类型变量，就像它们是异常的消息一样：

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

Zephir's exceptions provide the same methods to know where the exception happened that PHP's exceptions do. That is, `Exception::getFile()` and `Exception::getLine()` return the location in the Zephir code where the exception was thrown:

```bash
Exception: The static method 'someMethod' does not exist on model 'Robots'
File=phalcon/mvc/model.zep Line=4042
#0 /home/scott/test.php(64): Phalcon\Mvc\Model::__callStatic('someMethod', Array)
#1 /home/scott/test.php(64): Robots::someMethod()
#2 {main}
```
