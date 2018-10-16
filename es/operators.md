# Operadores

Los operadores en Zephir son similares a los de PHP, también heredarán algunas de sus conductas.

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

    if a & SOME_FLAG {
        echo "tiene la bandera";
    }
    

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

    if a == b {
        return 0;
    } else {
        if a < b {
            return -1;
        } else {
            return 1;
        }
    }
    

<a name='logical-operators'></a>

## Operadores Lógicos

Son soportados los siguientes operadores:

| Operación | Ejemplo          |
| --------- | ---------------- |
| And       | `a && b` |
| Or        | `a || b`         |
| Not       | `!a`             |

Ejemplo:

    if a && b || !c {
        return -1;
    }
    return 1;
    

<a name='tenary-operator'></a>

## Operador Ternario

Zephir soporta el operador ternario habilitado en C o PHP:

    let b = a == 1 ? "x" : "y"; // b es igual a "x" si a es igual a 1, en otro caso será igual a "y"
    

<a name='special-operators'></a>

## Operadores Especiales

Son soportados los siguientes operadores:

<a name='special-operators-empty'></a>

### Empty

Este operador permite chequear si una expresión esta vacía. 'Empty' significa que la expresión es igual a `null`, a una cadena de texto vacía, o a un array vacío:

    let someVar = "";
    if empty someVar {
        echo "¡esta vacía!";
    }
    
    let someVar = "hola";
    if !empty someVar {
        echo "¡no esta vacía!";
    }
    

<a name='special-operators-fetch'></a>

### Fetch

'Fetch' es un operador que reduce una operación común en PHP a una sola instrucción:

    <?php
    
    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }
    

En Zephir, puedes escribir el mismo código de la siguiente manera:

    if fetch value, myArray[key] {
        echo value;
    }
    

'Fetch' solo retornará `true` si la clave 'key' es un item válido en el array, y solo si tiene un valor 'value' asignado.

<a name='special-operators-isset'></a>

### Isset

Este operador comprueba que una propiedad o un índice esté definido en un array o en un objecto:

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // comprueba que el array tenga el índice "b"
        echo "si, hay un índice 'b'\n";
    }
    

Utilizando `isset` como una expresión de retorno:

    return isset this->{someProperty};
    

Note that `isset` in Zephir works more like PHP's function [array_key_exists](http://www.php.net/manual/en/function.array-key-exists.php), `isset` in Zephir returns true even if the array index or property is null.

<a name='special-operators-typeof'></a>

### Typeof

This operator checks a variable's type. 'typeof' can be used with a comparison operator:

    if (typeof str == "string") { // or !=
        echo str;
    }
    

It can also work like the PHP function `gettype`.

    return typeof str;
    

**Be careful**, if you want to check whether an object is 'callable', you always have to use `typeof` as a comparison operator, not a function.

<a name='special-operators-type-hints'></a>

### Type Hints

Zephir always tries to check whether an object implements methods and properties called/accessed on a variable that is inferred to be an object:

    let o = new MyObject();
    
    // Zephir checks if "myMethod" is implemented on MyObject
    o->myMethod();
    

However, due to the dynamism inherited from PHP, sometimes it is not easy to know the class of an object, so Zephir can't produce error reports effectively. A type hint tells the compiler which class is related to a dynamic variable, allowing the compiler to perform more compilation checks:

    // Tell the compiler that "o"
    // is an instance of class MyClass
    let o = <MyClass> this->_myObject;
    o->myMethod();
    

These "type hints" are weak. This means the program does not check if the value is in fact an instance of the specified class, nor whether it implements the specified interface. If you want it to check this every time in execution, use a strict type:

    // Always check if the property is an instance
    // of MyClass before the assignment
    let o = <MyClass!> this->_myObject;
    o->myMethod();
    

<a name='special-operators-branch-prediction-hints'></a>

### Branch Prediction Hints

What is branch prediction? Check this [article](http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/) or refer to the [Wikipedia article](https://en.wikipedia.org/wiki/Branch_predictor). In environments where performance is very important, it may be useful to introduce these hints.

Consider the following example:

    let allPaths = [];
    for path in this->_paths {
        if path->isAllowed() == false {
            throw new App\Exception("Some error message here");
        } else {
            let allPaths[] = path;
        }
    }
    

The authors of the above code know in advance that the condition that throws the exception is unlikely to happen. This means that, 99.9% of the time, our method executes that condition, but it is probably never evaluated as true. For the processor, this could be hard to know, so we could introduce a hint there:

    let allPaths = [];
    for path in this->_paths {
        if unlikely path->isAllowed() == false {
            throw new App\Exception("Some error message here");
        } else {
            let allPaths[] = path;
        }
    }