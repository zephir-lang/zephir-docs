---
layout: default
language: 'ru-RU'
version: '0.11'
menu:
  - text:
      'Declaring Array Variables'
    url: '#declaring-array-variables'
  - text:
      'Creating Arrays'
    url: '#creating-arrays'
  - text:
      'Updating arrays'
    url: '#updating-arrays'
  - text:
      'Appending elements'
    url: '#appending-elements'
  - text:
      'Reading elements from arrays'
    url: '#reading-elements-from-arrays'
---
# Arrays

Array manipulation in Zephir provides a way to use PHP [array](http://www.php.net/manual/en/language.types.array.php). An array is an implementation of a [hash table](http://en.wikipedia.org/wiki/Hash_table).

<a name='declaring-array-variables'></a>

## Declaring Array Variables

Array variables can be declared using the keywords 'var' or 'array':

    var a   = []; // array variable, its type can be changed
    array b = []; // array variable, its type cannot be changed across execution
    

<a name='creating-arrays'></a>

## Creating Arrays

An array is created by enclosing its elements in square brackets:

##### Creating an empty array

    let elements = [];
    

##### Creating an array with elements

    let elements = [1, 3, 4];
    

##### Creating an array with elements of different types

    let elements = ["first", 2, true];
    

##### A multidimensional array

    let elements = [[0, 1], [4, 5], [2, 3]];
    

As PHP, hashes or dictionaries are supported:

##### Creating a hash with string keys

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