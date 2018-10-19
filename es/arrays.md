# Arrays

La manipulación de arrays o arreglos en Zephir proporciona una manera de utilizar [arrays de PHP](http://www.php.net/manual/en/language.types.array.php). Un array es una implementación de una [tabla hash](http://en.wikipedia.org/wiki/Hash_table).

<a name='declaring-array-variables'></a>

## Declarar Variables de Array

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

##### Creación de un hash con claves de texto

    let elements = ["foo": "bar", "bar": "foo"];
    

##### Creating a hash with numeric keys

    let elements = [4: "bar", 8: "foo"];
    

##### Creating a hash with mixed string and numeric keys

    let elements = [4: "bar", "foo": 8];
    

<a name='updating-arrays'></a>

## Updating arrays

Arrays are updated in the same way as PHP, using square brackets:

##### Updating an array with a string key

    let elements["foo"] = "bar";
    

##### Updating an array with a numeric key

    let elements[0] = "bar";
    

##### Updating multi-dimensional array

    let elements[0]["foo"] = "bar";
    let elements["foo"][0] = "bar";
    

<a name='appending-elements'></a>

## Appending elements

Elements can be appended at the end of the array as follows:

##### Append an element to the array

    let elements[] = "bar";
    

<a name='reading-elements-from-arrays'></a>

## Reading elements from arrays

It is possible to read array elements as follows:

##### Getting an element using the string key `foo`

    let foo = elements["foo"];
    

##### Getting an element using the numeric key 0

    let foo = elements[0];