---
layout: default
language: 'es-es'
version: '0.12'
---

# Estructuras de Control

Zephir implementa un conjunto simplificado de estructuras de control presentes en lenguajes similares como C, PHP, etc.

<a name='conditionals'></a>

## Condicionales

<a name='conditionals-if'></a>

### Declaración If

`if` statements evaluate an expression, executing the following block if the evaluation is `true`. Las llaves son obligatorias. Un `if` puede tener un `else` opcional y múltiples construcciones `if`/`else` se pueden encadenar juntas:

```zephir
if false {
    echo "¿falso?";
} else {
    if true {
        echo "¡verdadero!";
    } else {
        echo "ni verdadero ni falso";
    }
}
```

Las declaraciones `elseif` también están disponibles:

```zephir
if a > 100 {
    echo "muy grande";
} elseif a < 0 {
    echo "muy pequeño";
} elseif a == 50 {
    echo "¡perfecto!";
} else {
    echo "ok";
}
```

Los paréntesis en la expresión evaluada son opcionales:

```zephir
if a < 0 { return -1; } else { if a > 0 { return 1; } }
```

<a name='conditionals-switch'></a>

### Declaración Switch

Un `switch` evalúa una expresión contra una serie de valores literales predefinidos, ejecutando los correspondientes bloques `case` o en caso contrario, finalizando en `default`:

```zephir
switch count(items) {

    case 1:
    case 3:
        echo "impares";
        break;

    case 2:
    case 4:
        echo "pares";
        break;

    default:
        echo "desconocido";
}
```

<a name='loops'></a>

## Bucles

<a name='loops-while'></a>

### Declaración While

`while` denotes a loop that iterates as long as its given condition evaluates as `true`:

```zephir
let counter = 5;
while counter {
    let counter -= 1;
}
```

<a name='loops-loop'></a>

### Declaración Loop

Además de `while`, `loop` puede ser utilizado para crear bucles infinitos:

```zephir
let n = 40;
loop {
    let n -= 2;
    if n % 5 == 0 { break; }
    echo x, "\n";
}
```

<a name='loops-for'></a>

### Declaración For

`for` es una estructura de control que permite recorrer matrices o cadenas de texto:

```zephir
for item in ["a", "b", "c", "d"] {
    echo item, "\n";
}
```

Las claves hash se pueden obtener proporcionando una variable tanto para la clave como para el valor:

```zephir
let items = ["a": 1, "b": 2, "c": 3, "d": 4];

for key, value in items {
    echo key, " ", value, "\n";
}
```

Un bucle `for` también puede ser utilizado para recorrer una matriz o cadena de texto en orden inverso:

```zephir
let items = [1, 2, 3, 4, 5];

for value in reverse items {
    echo value, "\n";
}
```

Un bucle `for` también puede utilizarse para recorrer cadenas de texto:

```zephir
string language = "zephir"; char ch;

for ch in language {
    echo "[", ch ,"]";
}
```

En orden inverso:

```zephir
string language = "zephir"; char ch;

for ch in reverse language {
    echo "[", ch ,"]";
}
```

Un `for` estandar, que recorre un rango de valores enteros, se puede escribir de la siguiente manera:

```zephir
for i in range(1, 10) {
    echo i, "\n";
}
```

Para evitar avisos sobre variables no utilizadas, es posible utilizar variables anónimas en las declaraciones `for`, reemplazando el nombre de una variable por el marcador `_`:

##### Utilizar las claves pero ignorar los valores

```zephir
for key, _ in data {
    echo key, "\n";
}
```

<a name='loops-break'></a>

### Declaración Break

Un `break` termina con la ejecución actual de una declaración `while`, `for` o `loop`:

```zephir
for item in ["a", "b", "c", "d"] {
    if item == "c" {
        break; // salir del for
    }
    echo item, "\n";
}
```

<a name='loops-continue'></a>

### Declaración Continue

El `continue` se usa dentro de las estructuras de bucle para omitir el resto de la iteración del bucle actual y continuar la ejecución en la evaluación de condición, y luego al comienzo de la siguiente iteración.

```zephir
let a = 5;
while a > 0 {
    let a--;
    if a == 3 {
        continue;
    }
    echo a, "\n";
}
```

<a name='require'></a>

## Require

La declaración `require` incluye dinámicamente y evalúa un archivo especifico de PHP. Tenga en cuenta que estos archivos incluidos por Zephir son interpretados por el Zend Engine como un archivo normal de PHP. `require` does not allow Zephir code to include other Zephir files at runtime.

```zephir
if file_exists(path) {
    require path;
}
```

<a name='let'></a>

## Let

La declaración `let` es utilizada para mutar variables, propiedades y arrays. Las variables, por defecto, son inmutables y esta instrucción les hace mutables por la duración de la declaración:

```zephir
let name = "Tony";           // variable simple
let this->name = "Tony";     // propiedad de un objecto
let data["name"] = "Tony";   // índice de un array
let self::_name = "Tony";    // propiedad estática
```

Además esta instrucción debe ser utilizada para incrementar/disminuir variables:

```zephir
let number++;           // incrementar una variable simple
let number--;           // disminuir una variable simple
let this->number++;     // incrementar una propiedad de un objecto
let this->number--;     // disminuir una propiedad de un objecto
```

Varias mutaciones pueden ser realizadas en una sola operación `let`:

```zephir
let price = 1.00, realPrice = price, status = false;
```
