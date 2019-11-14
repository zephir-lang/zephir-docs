* * *

layout: default language: 'en' version: '0.11'

* * *

# Встроенные методы

As mentioned before, Zephir promotes object-oriented programming. Variables related to static types can also be handled as objects.

Compare these two methods:

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

And:

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

They both have the same functionality, but the second one uses object-oriented programming. Calling methods on static-typed variables does not have any impact on performance since Zephir internally transforms the code from the object-oriented version to the procedural version.

<a name='string'></a>

## String

The following string built-in methods are available:

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

| ООП                  | Процедурный             | Описание                                                                    |
| -------------------- | ----------------------- | --------------------------------------------------------------------------- |
| `a->combine(b)`   | `array_combine(a, b)`   | Создать массив, используя один массив для ключей, а другой для его значений |
| `a->diff()`       | `array_diff(a)`         | Вычисляет разницу массивов                                                  |
| `a->flip()`       | `array_flip(a)`         | Обмен всех ключей со связанными значениями в массиве                        |
| `a->hasKey()`     | `array_key_exists(a)`   | Проверяет, существует ли данный ключ или индекс в массиве                   |
| `a->intersect(b)` | `array_intersect(a, b)` | Вычисляет пересечение массивов                                              |
| `a->join(" ")`    | `join(" ", a)`          | Объединение элементов массива в строку                                      |
| `a->keys()`       | `array_keys(a)`         | Возвращает все ключи или подмножество ключей массива                        |
| `a->merge(b)`     | `array_merge(a, b)`     | Объединяет один или большее количество массивов                             |
| `a->pad()`        | `array_pad(a, b)`       | Расширить массив до указанной длины с указанным значением                   |
| `a->rev()`        | `array_reverse(a)`      | Возвращает массив с элементами в обратном порядке                           |
| `a->reversed()`   | `array_reverse(a)`      | Возвращает массив с элементами в обратном порядке                           |
| `a->split()`      | `array_chunk(a)`        | Разбивает массив на части                                                   |
| `a->values()`     | `array_values(a)`       | Выбирает все значения массива                                               |
| `a->walk()`       | `array_walk(a)`         | Применяет заданную пользователем функцию к каждому элементу массива         |

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