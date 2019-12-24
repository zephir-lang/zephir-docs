---
layout: default
language: 'ru-ru'
version: '0.10'
---

# Оптимизации
Поскольку код на Zephir иногда очень высокоуровневый, C-компилятор может быть не в состоянии эффективно оптимизировать этот код.

Благодаря AOT-компилятору (ahead-of-time), Zephir способен оптимизировать код во время компиляции, потенциально улучшая время выполнения или уменьшая объем памяти, необходимый программе.

Вы можете включить оптимизацию, передав её название при помощи ключа `-f`:

```bash
zephir -fstatic-type-inference -flocal-context-pass
```

Оптимизация может быть отключена при помощи ключа `-fno-`:

```bash
zephir -fno-static-type-inference -fno-call-gatherer-pass
```

Оптимизации также могут настроены в конфигурационном файле `config.json`, как показано ниже:
```json
{
  "namespace": "mae",
  "name": "My Awesome Extension",
  "author": "ACME",
  "version": "1.0.0",

  "optimizations": {
    "static-type-inference": true,
    "static-type-inference-second-pass": true,
    "local-context-pass": true,
    "constant-folding": true,
    "static-constant-class-folding": true,
    "call-gatherer-pass": true,
    "check-invalid-reads": false,
    "private-internal-methods": false,
    "public-internal-methods": false,
    "public-internal-functions": true
  }
}
```

Поддерживаются следующие типы оптимизаций:

<a name='call-gatherer-pass'></a>

## call-gatherer-pass
Эта оптимизации учитывает, сколько раз функция или метод вызывается в рамках одного и того же метода. Это позволяет компилятору использовать встроенное кэширование, что позволяет избежать повторного поиска владельца метода или функции:

```zephir
class MyClass extends OtherClass
{

    public function getValue()
    {
        this->someMethod();

        // Этот метод может быть вызван быстрее
        this->someMethod();
    }
}
```

<a name='check-invalid-reads'></a>

## check-invalid-reads
Этот флаг форсирует принудительную проверку типов во время компиляции на выявление неверных операций чтения. Это гарантирует, что все переменные и указатели правильно определены и инициализированы значениями по умолчанию. Например, сравните:

```zephir
namespace Acme;

class ForInRange
{
    public static function forEmpty(var n)
    {
        var i;
        for i in range(1, n) {
            // Здесь происходит полезная работа
        }
    }
}
```

со следующим примером:


```zephir
namespace Acme;

class ForInRange
{
    public static function forEmpty(var n)
    {
        var i = null;
        for i in range(1, n) {
            // Здесь происходит полезная работа
        }
    }
}
```

Оба примера в действительности являются корректными с точки зрения синтаксиса Zephir. Разница проявляется при генерации Си-кода:

```c
zval *n;

// ...

zephir_fetch_params(1, 1, 0, &n);
```

сравните с:

```c
zval *n = NULL;

// ...

zephir_fetch_params(1, 1, 0, &n);
```

Хорошей практикой для любого языка программирования является инициализация переменных значениями по умолчанию с соблюдением корректности типов. Если не следовать этому принципу, потенциально это может привести к непредвиденным последствиям для приложения, а также может привести к ошибкам, утечкам памяти и т.д. Используя флаг `check-invalid-reads` в `config.json`, мы гарантируем, что указатели правильно инициализированы вместе с соответствующими переменными в Си-коде. Zephir-разработчики не увидят изменений в коде. Эта оптимизация влияет на сгенерированный Си-код.

Более подробную информацию о том, почему указатели в Си необходимо обнулять при переполнении стека, можно найти [здесь](https://stackoverflow.com/q/12253191/1661465).

<a name='constant-folding'></a>

## constant-folding
Свертывание констант — это процесс упрощения константных выражений во время компиляции. Следующий код упрощается, когда эта оптимизация включена:

```zephir
public function getValue()
{
    return (86400 * 30) / 12;
}
```

Преобразуется в:

```zephir
public function getValue()
{
    return 216000;
}
```

<a name='internal-call-transformation'></a>

## internal-call-transformation
The `internal-call-transformation` is required to generate internal methods, based on their equivalent PHP ones, allowing for the bypass of the PHP userspace for those internal method calls. By default, this optimization is turned off.

This optimization generates 2 implementations per method, one that is exposed in PHP and an internal one.

Exceptions to the above are:

- Only PHP methods allowed to be replaced (eg. we can't do this for Phalcon's methods)
- Closures (`__invoke`) and `__construct` are not supported
- The number of required parameters must exactly match the number of actual parameters
- Does not work for ZendEngine2 (PHP 5.6)

<a name='local-context-pass'></a>

## local-context-pass
This compilation pass moves variables that will be allocated in the heap to the stack. This optimization can reduce the number of memory indirections a program has to do.

<a name='static-constant-class-folding'></a>

## static-constant-class-folding
This optimization replaces values of class constants in compile time:

```zephir
class MyClass
{

    const SOME_CONSTANT = 100;

    public function getValue()
    {
        return self::SOME_CONSTANT;
    }
}
```

Преобразуется в:

```zephir
class MyClass
{

    const SOME_CONSTANT = 100;

    public function getValue()
    {
        return 100;
    }
}
```

<a name='static-type-inference'></a>

## static-type-inference
This compilation pass is very important, since it looks for dynamic variables that can potentially be transformed into static/primitive types, which are better optimized by the underlying compiler.

The following code uses a set of dynamic variables to perform some mathematical calculations:

```zephir
public function someCalculations(var a, var b)
{
    var i = 0, t = 1;

    while i < 100 {
        if i % 3 == 0 {
            continue;
        }
        let t += (a - i), i++;
    }

    return i + b;
}
```

Variables `a`, `b`, and `i` are used exclusively in mathematical operations, and can thus be transformed into static variables taking advantage of other compilation passes. After this pass, the compiler automatically rewrites this code to:

```zephir
public function someCalculations(int a, int b)
{
    int i = 0, t = 1;

    while i < 100 {
        if i % 3 == 0 {
            continue;
        }
        let t += (a - i), i++;
    }

    return i + b;
}
```

By disabling this compilation pass, all variables will maintain the type with which they were originally declared, without optimization.

<a name='static-type-inference-second-pass'></a>

## static-type-inference-second-pass
Эта оптимизация включает повторный вывод типов, что в целом улучшает работу проделанную на основе данных, собранных при первом проходе вывода типов.
