# Métodos integrados

Como se mencionó antes, Zephir promueve la programación orientada a objetos. Las variables relacionadas con los tipos estáticos también se pueden manejar como objetos.

Compare estos dos métodos:

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
    

Y:

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
    

Los tienen la misma funcionalidad, pero el segundo utiliza programación orientada a objecto. Los métodos de llamada en variables de tipo estático no tienen ningún impacto en el rendimiento, ya que Zephir transforma internamente el código de la versión orientada a objetos a la versión de procedimiento.

<a name='string'></a>

## String

Los siguientes métodos incorporados de cadena están disponibles:

| OO                   | Procedimientos        | Descripción                                                                   |
| -------------------- | --------------------- | ----------------------------------------------------------------------------- |
| `s->format()`     | `sprintf(s, "%s", x)` | Retorna una cadena formateada                                                 |
| `s->index("foo")` | `strpos(s, "foo")`    | Busca la posición de la primer aparición de una subcadena en una cadena       |
| `s->length()`     | `strlen(s)`           | Obtiene el largo de una cadena                                                |
| `s->lower()`      | `strtolower(s)`       | Make a string lowercase                                                       |
| `s->lowerfirst()` | `lcfirst(s)`          | Make a string's first character lowercase                                     |
| `s->md5()`        | `md5(s)`              | Calculate the md5 hash of a string                                            |
| `s->sha1()`       | `sha1(s)`             | Calculate the sha1 hash of a string                                           |
| `s->trim()`       | `trim(s)`             | Strip whitespace (or other characters) from the beginning and end of a string |
| `s->trimleft()`   | `ltrim(s)`            | Strip whitespace (or other characters) from the beginning of a string         |
| `s->trimright()`  | `rtrim(s)`            | Strip whitespace (or other characters) from the end of a string               |
| `s->upper()`      | `strtoupper(s)`       | Make a string uppercase                                                       |
| `s->upperfirst()` | `ucfirst(s)`          | Make a string's first character uppercase                                     |

<a name='array'></a>

## Array

The following array built-in methods are available:

| OO                   | Procedimientos          | Descripción                                                             |
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

| OO               | Procedimientos      |
| ---------------- | ------------------- |
| `ch->toHex()` | `sprintf("%X", ch)` |

<a name='integer'></a>

## Integer

The following integer built-in methods are available:

| OO            | Procedimientos |
| ------------- | -------------- |
| `i->abs()` | `abs(i)`       |