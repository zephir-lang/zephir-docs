---
layout: default
language: 'uk-ua'
version: '0.11'
---

# Масиви
Робота з масивами в Zephir здійснюється таким самим чином, [як і в PHP](https://www.php.net/manual/en/language.types.array.php). An array is an implementation of a [hash table](https://en.wikipedia.org/wiki/Hash_table).

<a id='declaring-array-variables'></a>

## Оголошення масивів
Масив можна оголосити за допомогою двох ключслів `var` та `array`:

```zephir
var a   = []; // масив, з можливістю зміни типу
array b = []; // масив, без можливості зміни типу
```

<a id='creating-arrays'></a>

## Створення масивів
Масив створюється шляхом укладення його елементів в квадратні дужки:

##### Створення порожнього масиву

```zephir
let elements = [];
```

##### Створення масиву з елементами

```zephir
let elements = [1, 3, 4];
```

##### Створення масиву з елементами різних типів

```zephir
let elements = ["first", 2, true];
```

##### Створення багатовимірного масиву

```zephir
let elements = [[0, 1], [4, 5], [2, 3]];
```

Як і в PHP, підтримуються прості (списки) і асоціативні масиви:

##### Створення масиву з рядковими ключами

```zephir
let elements = ["foo": "bar", "bar": "foo"];
```

##### Створення масиву з числовими ключами

```zephir
let elements = [4: "bar", 8: "foo"];
```

##### Створення масиву з змішаними ключами (рядковими та числовими)

```zephir
let elements = [4: "bar", "foo": 8];
```

<a id='updating-arrays'></a>

## Оновлення масивів
Масиви оновлюються таким же чином, як і PHP, використовуючи квадратні дужки:

##### Оновлення масиву з рядковим ключем

```zephir
let elements["foo"] = "bar";
```

##### Оновлення масиву з числовим ключем

```zephir
let elements[0] = "bar";
```

##### Оновлення багатовимірного масиву

```zephir
let elements[0]["foo"] = "bar";
let elements["foo"][0] = "bar";
```

<a id='appending-elements'></a>

## Додавання елементів
Елементи можуть бути додані в кінці масиву наступним чином:

##### Додавання елемента до масиву

```zephir
let elements[] = "bar";
```

<a id='reading-elements-from-arrays'></a>

## Читання елементів з масивів
Прочитати елементи масиву можна наступним чином:

##### Отримання елемента, використовуючи рядковий ключ `foo`

```zephir
let foo = elements["foo"];
```

##### Отримання елемента за допомогою числового ключа 0

```zephir
let foo = elements[0];
```
