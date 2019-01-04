---
layout: default
language: 'es-ES'
version: '0.11'
menu:
  - text:
      'Operadores Aritméticos'
    url: '#arithmetic-operators'
  - text:
      'Operadores bit a bit'
    url: '#bitwise-operators'
  - text:
      'Comparación de Operadores'
    url: '#comparison-operators'
  - text:
      'Operadores Lógicos'
    url: '#logical-operators'
  - text:
      'Operador Ternario'
    url: '#tenary-operator'
  - text:
      'Operadores Especiales'
    url: '#special-operators'
    sub:
      - text:
            'Empty'
        url: '#special-operators-empty'
      - text:
            'Fetch'
        url: '#special-operators-fetch'
      - text:
            'Isset'
        url: '#special-operators-isset'
      - text:
            'Sugerencias de Tipos'
        url: '#special-operators-type-hints'
      - text:
            'Consejos de Predicción de Rama'
        url: '#special-operators-branch-prediction-hints'
---
# Operadores

Los operadores en Zephir son similares a los de PHP, y también heredarán algunos de sus comportamientos.

<a name='arithmetic-operators'></a>

## Operadores Aritméticos

Son soportados los siguientes operadores:

| Operación      | Ejemplo |
| -------------- | ------- |
| Negación       | `-a`    |
| Suma           | `a + b` |
| Resta          | `a - b` |
| Multiplicación | `a * b` |
| División       | `a / b` |
| Módulo         | `a % b` |

<a name='bitwise-operators'></a>

## Operadores bit a bit

Son soportados los siguientes operadores:

| Operación            | Ejemplo        |
| -------------------- | -------------- |
| And                  | `a & b`    |
| Or (o inclusivo)     | `a | b`        |
| Xor (o exclusivo)    | `a ^ b`        |
| Not                  | `~a`           |
| Mover a la izquierda | `a << b` |
| Mover a la derecha   | `a >> b` |

Ejemplo:

```zephir
if a & SOME_FLAG {
    echo "tiene la bandera";
}
```

Para más información sobre la comparación de variables dinámicas vea el [manual de PHP](http://www.php.net/manual/en/language.operators.comparison.php).

<a name='comparison-operators'></a>

## Comparación de Operadores

Los operadores de comparación dependen del tipo de variables en comparación. Por ejemplo, si ambos operandos de la comparación son variables dinámicas, el comportamiento es igual que en PHP:

| Ejemplo        | Operación         | Descripción                                                                  |
| -------------- | ----------------- | ---------------------------------------------------------------------------- |
| `a == b`       | Igual             | `true` si a es igual a b después de arreglar de otro modo las variables.     |
| `a === b`      | Idéntico          | `true` si a es igual a b, y ambas son del mismo tipo.                        |
| `a != b`       | No iguales        | `true` si a no es igual a b, después de arreglar de otro modo las variables. |
| `a <> b` | No iguales        | `true` si a no es igual a b, después de arreglar de otro modo las variables. |
| `a !== b`      | No idénticos      | `true` si a no es igual a b, o ambos no son del mismo tipo.                  |
| `a < b`     | Menor Que         | `true` si a es estrictamente menor que b.                                    |
| `a > b`     | Mayor que         | `true` si a es estrictamente mayor que b.                                    |
| `a <= b`    | Menor o igual que | `true` si a es menor o igual que b.                                          |
| `a >= b`    | Mayor o igual que | `true` si a es mayor o igual que b.                                          |

Ejemplo:

```zephir
if a == b {
    return 0;
} else {
    if a < b {
        return -1;
    } else {
        return 1;
    }
}
```

<a name='logical-operators'></a>

## Operadores Lógicos

Son soportados los siguientes operadores:

| Operación | Ejemplo          |
| --------- | ---------------- |
| And       | `a && b` |
| Or        | `a || b`         |
| Not       | `!a`             |

Ejemplo:

```zephir
if a && b || !c {
    return -1;
}
return 1;
```

<a name='tenary-operator'></a>

## Operador Ternario

Zephir soporta el operador ternario habilitado en C o PHP:

```zephir
let b = a == 1 ? "x" : "y"; // b es igual a "x" si a es igual a 1, en otro caso será igual a "y"
```

<a name='special-operators'></a>

## Operadores Especiales

Son soportados los siguientes operadores:

<a name='special-operators-empty'></a>

### Empty

Este operador permite chequear si una expresión esta vacía. 'Empty' significa que la expresión es igual a `null`, a una cadena de texto vacía, o a un array vacío:

```zephir
let someVar = "";
if empty someVar {
    echo "¡esta vacía!";
}

let someVar = "hola";
if !empty someVar {
    echo "¡no esta vacía!";
}
```

<a name='special-operators-fetch'></a>

### Fetch

'Fetch' es un operador que reduce una operación común en PHP a una sola instrucción:

```php
<?php

if (isset($myArray[$key])) {
    $value = $myArray[$key];
    echo $value;
}
```

En Zephir, puedes escribir el mismo código de la siguiente manera:

```zephir
if fetch value, myArray[key] {
    echo value;
}
```

'Fetch' solo retornará `true` si la clave 'key' es un item válido en el array, y solo si tiene un valor 'value' asignado.

<a name='special-operators-isset'></a>

### Isset

Este operador comprueba que una propiedad o un índice esté definido en un array o en un objecto:

```zephir
let someArray = ["a": 1, "b": 2, "c": 3];
if isset someArray["b"] { // comprueba que el array tenga el índice "b"
    echo "si, hay un índice 'b'\n";
}
```

Utilizando `isset` como una expresión de retorno:

```zephir
return isset this->{someProperty};
```

Nota: en Zephir `isset` funciona como la función [array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php) de PHP, `isset` en Zephir retornará `true` incluso cuando el índice del array o la propiedad del objecto sean nulas.

<a name='special-operators-typeof'></a>

### Typeof

Este operador comprueba el tipo de una variable. `typeof` puede usarse con un operador de comparación:

```zephir
if (typeof str == "string") { // o !=
    echo str;
}
```

También puede trabajar como la función `gettype` de PHP.

```zephir
return typeof str;
```

**Cuidado** si desea comprobar que si un objeto es "callable", siempre tienes que usar `typeof` como un operador de comparación, no como una función.

<a name='special-operators-type-hints'></a>

### Sugerencias de Tipos

Zephir siempre trata de comprobar si un objeto implementa métodos y propiedades llamado/accedido a una variable, que se infiere que es un objeto:

```zephir
let o = new MyObject();

// Zephir comprueba si "myMethod" es implementado en MyObject
o->myMethod();
```

Sin embargo, debido al dinamismo heredado de PHP, a veces no es fácil saber la clase de un objeto, entonces Zephir no puede producir informes de errores con eficacia. Una sugerencia de tipo le indica al compilador que clase se relaciona con una variable dinámica, permitiendo que el compilador hacer más verificaciones de compilación:

```zephir
// Decirle al compilador que "o"
// es una instancia de la clase MyClass
let o = <MyClass> this->_myObject;
o->myMethod();
```

Estos "consejos de tipo" son débiles. Esto significa que el programa no comprueba si el valor es de hecho una instancia de la clase especificada, ni si implementa la interfaz especificada. Si desea comprobarlo en ejecución cada vez, utilice un tipo estricto:

```zephir
// Siempre comprueba si la propiedades es una instancia
// de MyClass antes de asignarla
let o = <MyClass!> this->_myObject;
o->myMethod();
```

<a name='special-operators-branch-prediction-hints'></a>

### Consejos de Predicción de Rama

¿Qué es la predicción de rama? Revisa este [artículo](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) o este otro [artículo en la Wikipedia](https://en.wikipedia.org/wiki/Branch_predictor). En entornos donde el desempeño es muy importante, puede ser útil introducir estos consejos.

Considere el siguiente ejemplo:

```zephir
let allPaths = [];
for path in this->_paths {
    if path->isAllowed() == false {
        throw new App\Exception("Un mensaje de error");
    } else {
        let allPaths[] = path;
    }
}
```

Los autores del código anterior saben, de antemano, que es poco probable que ocurra la condición que produce la excepción. Esto significa que, el 99.9% del tiempo, nuestro método ejecuta esa condición, pero probablemente nunca se evalúe como verdadero. Para el procesador, esto podría ser difícil de saber, pero le podríamos presentar una sugerencia allí:

```zephir
let allPaths = [];
for path in this->_paths {
    if unlikely path->isAllowed() == false {
        throw new App\Exception("Un mensaje de error");
    } else {
        let allPaths[] = path;
    }
}
```
