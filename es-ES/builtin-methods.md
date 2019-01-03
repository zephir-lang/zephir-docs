---
layout: default
language: 'es-ES'
version: '0.11'
menu:
  - text:
      'String'
    url: '#string'
  - text:
      'Array'
    url: '#array'
  - text:
      'Char'
    url: '#char'
  - text:
      'Integer'
    url: '#integer'
---
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

| OO                   | Procedimientos        | Descripción                                                                        |
| -------------------- | --------------------- | ---------------------------------------------------------------------------------- |
| `s->format()`     | `sprintf(s, "%s", x)` | Retorna una cadena formateada                                                      |
| `s->index("foo")` | `strpos(s, "foo")`    | Busca la posición de la primer aparición de una subcadena en una cadena            |
| `s->length()`     | `strlen(s)`           | Obtiene el largo de una cadena                                                     |
| `s->lower()`      | `strtolower(s)`       | Convierte a minúsculas una cadena de texto                                         |
| `s->lowerfirst()` | `lcfirst(s)`          | Convierte en minúscula la primera letra de la cadena de texto                      |
| `s->md5()`        | `md5(s)`              | Calcula el hash MD5 de la cadena de texto                                          |
| `s->sha1()`       | `sha1(s)`             | Calcula el hash SHA1 de la cadena de texto                                         |
| `s->trim()`       | `trim(s)`             | Quita los espacios en blanco (u otros caracteres) del inicio y del final del texto |
| `s->trimleft()`   | `ltrim(s)`            | Quita los espacios en blanco (u otros caracteres) del inicio del texto             |
| `s->trimright()`  | `rtrim(s)`            | Quita los espacios en blanco (u otros caracteres) del final del texto              |
| `s->upper()`      | `strtoupper(s)`       | Convierte a mayúsculas una cadena de texto                                         |
| `s->upperfirst()` | `ucfirst(s)`          | Convierte en mayúscula la primera letra de la cadena de texto                      |

<a name='array'></a>

## Array

Los siguientes métodos incorporados de array están disponibles:

| OO                   | Procedimientos          | Descripción                                                               |
| -------------------- | ----------------------- | ------------------------------------------------------------------------- |
| `a->combine(b)`   | `array_combine(a, b)`   | Crea un array utilizando un array para las claves y otro para sus valores |
| `a->diff()`       | `array_diff(a)`         | Calcula la diferencia entre arrays                                        |
| `a->flip()`       | `array_flip(a)`         | Intercambia todos las claves del array por sus valores asociados          |
| `a->hasKey()`     | `array_key_exists(a)`   | Comprueba si el índice o clave dada existe en el array                    |
| `a->intersect(b)` | `array_intersect(a, b)` | Calcula la intersección entre arrays                                      |
| `a->join(" ")`    | `join(" ", a)`          | Une los elementos de un array con un texto                                |
| `a->keys()`       | `array_keys(a)`         | Devolver todas las claves o un subconjunto de las claves de una matriz    |
| `a->merge(b)`     | `array_merge(a, b)`     | Une uno o más arrays                                                      |
| `a->pad()`        | `array_pad(a, b)`       | Rellena un array de un largo y con un valor dados                         |
| `a->rev()`        | `array_reverse(a)`      | Regresa un array con los elementos en orden inverso                       |
| `a->reversed()`   | `array_reverse(a)`      | Regresa un array con los elementos en orden inverso                       |
| `a->split()`      | `array_chunk(a)`        | Divide un array en partes                                                 |
| `a->values()`     | `array_values(a)`       | Regresa todos los valores de un array                                     |
| `a->walk()`       | `array_walk(a)`         | Aplica una función definida por el usuario a cada elemento de un array    |

<a name='char'></a>

## Char

Los siguientes métodos incorporados para chars están disponibles:

| OO               | Procedimientos      |
| ---------------- | ------------------- |
| `ch->toHex()` | `sprintf("%X", ch)` |

<a name='integer'></a>

## Integer

Los siguientes métodos incorporados para enteros están disponibles:

| OO            | Procedimientos |
| ------------- | -------------- |
| `i->abs()` | `abs(i)`       |