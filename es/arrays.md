# Arrays

La manipulación de arrays o arreglos en Zephir proporciona una manera de utilizar los [arrays de PHP](http://www.php.net/manual/en/language.types.array.php). Un array es una implementación de una [tabla hash](http://en.wikipedia.org/wiki/Hash_table).

<a name='declaring-array-variables'></a>

## Declarar Variables de tipo Array

Variables de array pueden ser declaradas usando las palabras clave 'var' o 'array':

    var a   = []; // variable array, el tipo puede ser cambiado
    array b = []; // variable array, el tipo no puede ser cambiado durante la ejecución
    

<a name='creating-arrays'></a>

## Creación de Arrays

Un array se crea encerrando sus elementos entre corchetes:

##### Crear una arreglo vacío

    let elements = [];
    

##### Crear un arreglo con elementos

    let elements = [1, 3, 4];
    

##### Crear un arreglo con elementos de diferentes tipos

    let elements = ["first", 2, true];
    

##### Un arreglo multidimensional

    let elements = [[0, 1], [4, 5], [2, 3]];
    

Al igual que en PHP los hashes o claves están soportadas:

##### Creación de un arreglo asociativo con claves de texto

    let elements = ["foo": "bar", "bar": "foo"];
    

##### Creación de un array asociativo con claves numéricas

    let elements = [4: "bar", 8: "foo"];
    

##### Creación de un array asociativo mixto, con claves de texto y numéricas

    let elements = [4: "bar", "foo": 8];
    

<a name='updating-arrays'></a>

## Actualización de arrays

Los arreglosse actualizan en la misma manera que en PHP, utilizando corchetes cuadrados:

##### Actualización de un array con una llave de texto

    let elements["foo"] = "bar";
    

##### Actualización de un array con una llave numérica

    let elements[0] = "bar";
    

##### Actualización de un array multidimensional

    let elements[0]["foo"] = "bar";
    let elements["foo"][0] = "bar";
    

<a name='appending-elements'></a>

## Agregando elementos

Los elementos pueden ser añadidos al final del array de la siguiente manera:

##### Añadir un nuevo elemento a un array

    let elements[] = "bar";
    

<a name='reading-elements-from-arrays'></a>

## Leyendo elementos desde arrays

Es posible leer los elementos de la matriz de la siguiente forma:

##### Obteniendo un elemento utilizando la clave de texto `foo`

    let foo = elements["foo"];
    

##### Obteniendo un elemento utilizando la clave numérica 0

    let foo = elements[0];