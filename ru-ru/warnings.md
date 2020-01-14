---
layout: default
language: 'ru-ru'
version: '0.10'
---

# Предупреждения компилятора
Компилятор выдает предупреждения, когда находит ситуации, в которых можно улучшить код или избежать возможной ошибки.

Предупреждения могут быть включены через параметры командной строки, или могут быть добавлены в `config.json`, чтобы включить или отключить их на продолжительное время.

Вы можете включить предупреждения, передав их имя с префиксом `-w`:

```bash
zephir -wunused-variable -wnonexistent-function
```

Вы можете включить предупреждения, передав их имя с префиксом `-w`:

```bash
zephir -Wunused-variable -Wnonexistent-function
```

Поддерживаются следующие типы предупреждений:

<a name='unused-variable'></a>

## unused-variable
Вызывается, когда переменная объявлена, но не используется внутри метода. Это предупреждение включено по умолчанию.

```zephir
public function some()
{
    var e; // declared but not used

    return false;
}
```

<a name='unused-variable-external'></a>

## unused-variable-external
Вызывается, когда параметр объявлен, но не используется внутри метода.

```zephir
public function sum(a, b, c) // c is not used
{
    return a + b;
}
```

<a name='possible-wrong-parameter-undefined'></a>

## possible-wrong-parameter-undefined
Вызывается при вызове метода с неправильным типом параметра:

```zephir
public function some()
{
    return this->sum("a string", "another");  // wrong parameters passed
}

public function sum(int a, int b)
{
    return a + b;
}
```

<a name='nonexistent-function'></a>

## nonexistent-function
Вызывается при вызове функции, которая не существует в момент компиляции:

```zephir
public function some()
{
    someFunction(); // someFunction does not exist
}
```

<a name='nonexistent-class'></a>

## nonexistent-class
Вызывается при использовании класса, который не существует в момент компиляции:

```zephir
public function some()
{
    var a;

    let a = new \MyClass(); // MyClass does not exist
}
```

<a name='non-valid-isset'></a>

## non-valid-isset
Вызывается, когда компилятор обнаруживает, что операция 'isset' выполняется на значении, не являющимся массивом или объектом:

```zephir
public function some()
{
    var b = 1.2;

    return isset b[0]; // variable integer 'b' used as array
}
```

<a name='non-array-update'></a>

## non-array-update
Возникает, когда компилятор обнаруживает, что операция обновления массива выполняется для значения, не являющегося значением массива:

```zephir
public function some()
{
    var b = 1.2;
    let b[0] = true; // variable 'b' cannot be used as array
}
```

<a name='non-valid-objectupdate'></a>

## non-valid-objectupdate
Вызывается когда компилятор обнаруживает, что выполняется операция обновления объекта, не являющегося объектом:

```zephir
public function some()
{
    var b = 1.2;
    let b->name = true; // variable 'b' cannot be used as object
}
```

<a name='non-valid-fetch'></a>

## non-valid-fetch
Вызывается, когда компилятор обнаруживает, что выполняется операция 'fetch' над значением, не являющимся массивом или объектом:

##### переменная 'b' используется в качестве массива

```zephir
public function some()
{
    var b = 1.2, a;
    fetch a, b[0];
}
```

<a name='invalid-array-index'></a>

## invalid-array-index
Вызывается, когда компилятор обнаруживает, что используется неверный индекс массива:

```zephir
public function some(var a)
{
    var b = [];
    let a[b] = true;
}
```

<a name='non-array-append'></a>

## non-array-append
Вызывается, когда компилятор обнаруживает, что элемент добавляется к переменной, не относящейся к массиву:

```zephir
public function some()
{
    var b = false;
    let b[] = "some value";
}
```
