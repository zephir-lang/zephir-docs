---
layout: default
language: 'es-es'
version: '0.10'
---

# Arrays
Array manipulation in Zephir provides a way to use PHP [array](https://www.php.net/manual/en/language.types.array.php). An array is an implementation of a [hash table](https://en.wikipedia.org/wiki/Hash_table).

<a id='declaring-array-variables'></a>

## Declarar Variables de tipo Array
Variables de array pueden ser declaradas usando las palabras clave 'var' o 'array':

```zephir
var a   = []; // variable array, el tipo puede ser cambiado
array b = []; // variable array, el tipo no puede ser cambiado durante la ejecución
```

<a id='creating-arrays'></a>

## Creación de Arrays
Un array se crea encerrando sus elementos entre corchetes:

##### Crear una arreglo vacío

```zephir
let elements = [];
```

##### Crear un arreglo con elementos

```zephir
let elements = [1, 3, 4];
```

##### Crear un arreglo con elementos de diferentes tipos

```zephir
let elements = ["first", 2, true];
```

##### Un arreglo multidimensional

```zephir
let elements = [[0, 1], [4, 5], [2, 3]];
```

Al igual que en PHP los hashes o claves están soportadas, también conocidos como arreglos asociativos:

##### Creación de un arreglo asociativo con claves de texto

```zephir
let elements = ["foo": "bar", "bar": "foo"];
```

##### Creación de un array asociativo con claves numéricas

```zephir
let elements = [4: "bar", 8: "foo"];
```

##### Creación de un array asociativo mixto, con claves de texto y numéricas

```zephir
let elements = [4: "bar", "foo": 8];
```

<a id='updating-arrays'></a>

## Actualización de arrays
Los arreglos se actualizan en la misma manera que en PHP, utilizando corchetes cuadrados:

##### Actualización de un array con una llave de texto

```zephir
let elements["foo"] = "bar";
```

##### Actualización de un array con una llave numérica

```zephir
let elements[0] = "bar";
```

##### Actualización de un array multidimensional

```zephir
let elements[0]["foo"] = "bar";
let elements["foo"][0] = "bar";
```

<a id='appending-elements'></a>

## Agregando elementos
Los elementos pueden ser añadidos al final del array de la siguiente manera:

##### Añadir un nuevo elemento a un array

```zephir
let elements[] = "bar";
```

<a id='reading-elements-from-arrays'></a>

## Leyendo elementos desde arrays
Es posible leer los elementos de la matriz de la siguiente forma:

##### Obteniendo un elemento utilizando la clave de texto `foo`

```zephir
let foo = elements["foo"];
```

##### Obteniendo un elemento utilizando la clave numérica 0

```zephir
let foo = elements[0];
```
