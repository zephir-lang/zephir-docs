---
layout: default
language: 'uk-ua'
version: '0.12'
---

# Вбудовані методи

Як було зазначено раніше, Zephir сприяє об'єктно-орієнтованому програмуванню. Змінні, які вважаються статичними типами, також можуть бути оброблені як об'єкти.

Порівняйте ці два методи:

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

та:

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

Обидва методи мають однакову функціональність, але другий використовує об'єктно-орієнтований підхід. Використання методів на статичнотипізованих змінних не мають жодного впливу на продуктивність, оскільки Zephir внутрішньо перетворює код з об'єктно-орієнтованої версії в процедурну версію.

<a id='string'></a>

## Рядок

Для рядкових типів даних визначені наступні вбудовані методи:

| ООП                  | Процедурний           | Опис                                                                  |
| -------------------- | --------------------- | --------------------------------------------------------------------- |
| `s->format()`     | `sprintf(s, "%s", x)` | Відформатувати рядок                                                  |
| `s->index("foo")` | `strpos(s, "foo")`    | Знайти позицію першого входження підрядка в рядок                     |
| `s->length()`     | `strlen(s)`           | Отримати довжину рядка                                                |
| `s->lower()`      | `strtolower(s)`       | Перевести рядок в нижній регістр                                      |
| `s->lowerfirst()` | `lcfirst(s)`          | Перекласти перший символ рядка в нижній регістр                       |
| `s->md5()`        | `md5(s)`              | Обчислити хеш md5 рядка                                               |
| `s->sha1()`       | `sha1(s)`             | Обчислити хеш sha1 рядка                                              |
| `s->trim()`       | `trim(s)`             | Прибрати пробіли (або інші символи) з початку та кінця рядка          |
| `s->trimleft()`   | `ltrim(s)`            | Strip whitespace (or other characters) from the beginning of a string |
| `s->trimright()`  | `rtrim(s)`            | Strip whitespace (or other characters) from the end of a string       |
| `s->upper()`      | `strtoupper(s)`       | Перевести рядок у верхній регістр                                     |
| `s->upperfirst()` | `ucfirst(s)`          | Перекласти перший символ рядка у верхній регістр                      |

<a id='array'></a>

## Array

Для масивів визначені наступні вбудовані методи:

| ООП                  | Процедурний             | Опис                                                                           |
| -------------------- | ----------------------- | ------------------------------------------------------------------------------ |
| `a->combine(b)`   | `array_combine(a, b)`   | Створити масив, використовуючи один масив для ключів, а інший для його значень |
| `a->diff()`       | `array_diff(a)`         | Обчислює різницю масивів                                                       |
| `a->flip()`       | `array_flip(a)`         | Обмін всіх ключів зі зв'язаними значеннями в масиві                            |
| `a->hasKey()`     | `array_key_exists(a)`   | Перевіряє, чи існує даний ключ або індекс в масиві                             |
| `a->intersect(b)` | `array_intersect(a, b)` | Обчислює січення масивів                                                       |
| `a->join(" ")`    | `join(" ", a)`          | Join array elements with a string                                              |
| `a->keys()`       | `array_keys(a)`         | Return all the keys or a subset of the keys of an array                        |
| `a->merge(b)`     | `array_merge(a, b)`     | Merge one or more arrays                                                       |
| `a->pad()`        | `array_pad(a, b)`       | Pad array to the specified length with a value                                 |
| `a->rev()`        | `array_reverse(a)`      | Return an array with elements in reverse order                                 |
| `a->reversed()`   | `array_reverse(a)`      | Return an array with elements in reverse order                                 |
| `a->split()`      | `array_chunk(a)`        | Split an array into chunks                                                     |
| `a->values()`     | `array_values(a)`       | Return all the values of an array                                              |
| `a->walk()`       | `array_walk(a)`         | Apply a user supplied function to every member of an array                     |

<a id='char'></a>

## Char

Для символів визначені наступні вбудовані методи:

| ООП              | Процедурний         |
| ---------------- | ------------------- |
| `ch->toHex()` | `sprintf("%X", ch)` |

<a id='integer'></a>

## Integer

The following integer built-in methods are available:

| ООП           | Процедурний |
| ------------- | ----------- |
| `i->abs()` | `abs(i)`    |