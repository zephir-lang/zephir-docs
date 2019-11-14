* * *

layout: default language: 'ru-ru' version: '0.10'

* * *

# Встроенные методы

Как упоминалось ранее, Zephir способствует объектно-ориентированному программированию. Переменные, относящиеся к статическим типам, также могут обрабатываться как объекты.

Сравните эти два метода:

```zephir
public function binaryToHex(string! s) -> string
{
    var o = "", n; char ch;

    for ch in range(0, strlen(s)) {
        let n = sprintf("%X", ch);
        if strlen(n) < 2 {
            let o .= "0" . n;
        } else {
            let o .= n;
        }
    }
    return o;
}
```

И:

```zephir
public function binaryToHex(string! s) -> string
{
    var o = "", n; char ch;

    for ch in range(0, s->length()) {
        let n = ch->toHex();
        if n->length() < 2 {
            let o .= "0" . n;
        } else {
            let o .= n;
        }
    }
    return o;
}
```

Оба они имеют одинаковую функциональность, но второй использует объектно-ориентированное программирование. Вызывающие методы для статических типизированных переменных не оказывают никакого влияния на производительность, поскольку Zephir внутренне преобразует код из объектно-ориентированной версии в процедурную версию.

<a name='string'></a>

## String

Доступны следующие строковые встроенные методы:

| ООП                  | Процедурный           | Описание                                                     |
| -------------------- | --------------------- | ------------------------------------------------------------ |
| `s->format()`     | `sprintf(s, "%s", x)` | Отформатировать строку                                       |
| `s->index("foo")` | `strpos(s, "foo")`    | Найти позицию первого вхождения подстроки в строке           |
| `s->length()`     | `strlen(s)`           | Получить длину строки                                        |
| `s->lower()`      | `strtolower(s)`       | Перевести строку в нижний регистр                            |
| `s->lowerfirst()` | `lcfirst(s)`          | Перевести первый символ строки в нижний регистр              |
| `s->md5()`        | `md5(s)`              | Вычислить md5 хэш строки                                     |
| `s->sha1()`       | `sha1(s)`             | Вычислить sha1 хэш строки                                    |
| `s->trim()`       | `trim(s)`             | Убрать пробелы (или другие символы) из начала и конца строки |
| `s->trimleft()`   | `ltrim(s)`            | Убрать пробелы (или другие символы) из начала строки         |
| `s->trimright()`  | `rtrim(s)`            | Убрать пробелы (или другие символы) с конца строки           |
| `s->upper()`      | `strtoupper(s)`       | Перевести строку в верхний регистр                           |
| `s->upperfirst()` | `ucfirst(s)`          | Перевести первый символ строки в верхний регистр             |

<a name='array'></a>

## Array

The following array built-in methods are available:

| ООП                  | Procedural              | Описание                                                                |
| -------------------- | ----------------------- | ----------------------------------------------------------------------- |
| `a->combine(b)`   | `array_combine(a, b)`   | Creates an array by using one array for keys and another for its values |
| `a->diff()`       | `array_diff(a)`         | Computes the difference of arrays                                       |
| `a->flip()`       | `array_flip(a)`         | Exchanges all keys with their associated values in an array             |
| `a->hasKey()`     | `array_key_exists(a)`   | Checks if the given key or index exists in the array                    |
| `a->intersect(b)` | `array_intersect(a, b)` | Computes the intersection of arrays                                     |
| `a->join(" ")`    | `join(" ", a)`          | Join array elements with a string                                       |
| `a->keys()`       | `array_keys(a)`         | Return all the keys or a subset of the keys of an array                 |
| `a->merge(b)`     | `array_merge(a, b)`     | Merge one or more arrays                                                |
| `a->pad()`        | `array_pad(a, b)`       | Pad array to the specified length with a value                          |
| `a->rev()`        | `array_reverse(a)`      | Return an array with elements in reverse order                          |
| `a->reversed()`   | `array_reverse(a)`      | Return an array with elements in reverse order                          |
| `a->split()`      | `array_chunk(a)`        | Split an array into chunks                                              |
| `a->values()`     | `array_values(a)`       | Return all the values of an array                                       |
| `a->walk()`       | `array_walk(a)`         | Apply a user supplied function to every member of an array              |

<a name='char'></a>

## Char

The following char built-in methods are available:

| ООП              | Процедурный         |
| ---------------- | ------------------- |
| `ch->toHex()` | `sprintf("%X", ch)` |

<a name='integer'></a>

## Integer

The following integer built-in methods are available:

| ООП           | Процедурный |
| ------------- | ----------- |
| `i->abs()` | `abs(i)`    |