---
layout: default
language: 'en'
version: '0.12'
---

# Πίνακες

Array manipulation in Zephir provides a way to use PHP [array](http://www.php.net/manual/en/language.types.array.php). An array is an implementation of a [hash table](http://en.wikipedia.org/wiki/Hash_table).

<a name='declaring-array-variables'></a>

## Declaring Array Variables

Array variables can be declared using the keywords 'var' or 'array':

```zephir
var a   = []; // array variable, its type can be changed
array b = []; // array variable, its type cannot be changed across execution
```

<a name='creating-arrays'></a>

## Creating Arrays

An array is created by enclosing its elements in square brackets:

##### Creating an empty array

```zephir
let elements = [];
```

##### Creating an array with elements

```zephir
let elements = [1, 3, 4];
```

##### Creating an array with elements of different types

```zephir
let elements = ["first", 2, true];
```

##### A multidimensional array

```zephir
let elements = [[0, 1], [4, 5], [2, 3]];
```

As PHP, hashes or dictionaries are supported:

##### Creating a hash with string keys

```zephir
let elements = ["foo": "bar", "bar": "foo"];
```

##### Creating a hash with numeric keys

```zephir
let elements = [4: "bar", 8: "foo"];
```

##### Creating a hash with mixed string and numeric keys

```zephir
let elements = [4: "bar", "foo": 8];
```

<a name='updating-arrays'></a>

## Updating arrays

Arrays are updated in the same way as PHP, using square brackets:

##### Updating an array with a string key

```zephir
let elements["foo"] = "bar";
```

##### Updating an array with a numeric key

```zephir
let elements[0] = "bar";
```

##### Updating multi-dimensional array

```zephir
let elements[0]["foo"] = "bar";
let elements["foo"][0] = "bar";
```

<a name='appending-elements'></a>

## Appending elements

Elements can be appended at the end of the array as follows:

##### Append an element to the array

```zephir
let elements[] = "bar";
```

<a name='reading-elements-from-arrays'></a>

## Reading elements from arrays

It is possible to read array elements as follows:

##### Getting an element using the string key `foo`

```zephir
let foo = elements["foo"];
```

##### Getting an element using the numeric key 0

```zephir
let foo = elements[0];
```