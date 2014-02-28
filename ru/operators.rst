Операторы
=========
Операторы в Zephir похожи на их аналоги в PHP и также себя ведут.

Арифметические операторы.
------------------------
Поддерживаемые операторы:

+-------------------+-----------------------------------------------------+
| Оператор          | Пример                                              |
+-------------------+-----------------------------------------------------+
| Приведение к      |                                                     |
| отрицательному    | -a                                                  |
+-------------------+-----------------------------------------------------+
| Сложение          | a + b                                               |
+-------------------+-----------------------------------------------------+
| Вычитание         | a - b                                               |
+-------------------+-----------------------------------------------------+
| Умножение         | a * b                                               |
+-------------------+-----------------------------------------------------+
| Деление           | a / b                                               |
+-------------------+-----------------------------------------------------+
| Модуль            | a % b                                               |
+-------------------+-----------------------------------------------------+

Операторы сравнения
--------------------
Операции сравнения зависят от типа сравниваемых переменных, например если оба
операнда динамические (var), результат будет таким же как и в PHP:

+----------+--------------------------+------------------------------------------------------------------+
| a == b   | Равенство                | TRUE если a равно b после приведения типов.                      |
+----------+--------------------------+------------------------------------------------------------------+
| a === b  | Идентичность             | TRUE если a равно b, и операнды одного типа.                     |
+----------+--------------------------+------------------------------------------------------------------+
| a != b   | Не равны                 | TRUE если a не равно b после приведения типов.                   |
+----------+--------------------------+------------------------------------------------------------------+
| a <> b   | Не равны                 | TRUE если a не равно b после приведения типов.                   |
+----------+--------------------------+------------------------------------------------------------------+
| a !== b  | Не идентичны             | TRUE если a не равно b, или операнды разных типов.               |
+----------+--------------------------+------------------------------------------------------------------+
| a < b    | Меньше                   | TRUE если a строго меньше b.                                     |
+----------+--------------------------+------------------------------------------------------------------+
| a > b    | Больше                   | TRUE если a строго больше b.                                     |
+----------+--------------------------+------------------------------------------------------------------+
| a <= b   | Меньше, или равно        | TRUE если a меньше, или равно b.                                 |
+----------+--------------------------+------------------------------------------------------------------+
| a >= b   | Больше, или равно        | TRUE если a строго больше, или равно b.                          |
+----------+--------------------------+------------------------------------------------------------------+

Пример:

.. code-block:: zephir

    if a == b {
        return 0;
    } else {
        if a < b {
            return -1;
        } else {
            return 1;
        }
    }

Логические опреаторы
--------------------
Поддерживаемые операторы:

+-------------------+-----------------------------------------------------+
| Операция          | Пример                                              |
+-------------------+-----------------------------------------------------+
| И                 | a && b                                              |
+-------------------+-----------------------------------------------------+
| Или               | a || b                                              |
+-------------------+-----------------------------------------------------+
| Отрицание         | !a                                                  |
+-------------------+-----------------------------------------------------+

Пример:

.. code-block:: zephir

    if a && b || !c {
        return -1;
    }
    return 1;

Побитовые операторы
-------------------
Поддерживаемые операторы:

+---------------------+------------------------------------------------------+
| Операция            | Пример                                               |
+---------------------+------------------------------------------------------+
| Побитовое И         | a & b                                                |
+---------------------+------------------------------------------------------+
| Побитовое Или       | a | b                                                |
+---------------------+------------------------------------------------------+
| Побитовое отрицание | ~a                                                   |
+---------------------+------------------------------------------------------+

Пример:

.. code-block:: zephir

    if a & SOME_FLAG {
        echo "has some flag";
    }

Вы узнаете больше о сравнении динамических переменных в `php документации`_.

Тернарный оператор
------------------
Zephir поддерживает тернарный оператор, как в C или PHP.

.. code-block:: zephir

    let b = a == 1 ? "x" : "y"; // в b будет присвоен "x", если a равно 1 в противном случае "y"

Специальные операторы
---------------------
Поддерживаемые операторы:

Empty
^^^^^
Empty позволяет узнать пусто ли выражение. Под 'пусто' подразумевается выражение возврашающее:
 - null
 - пустую строку
 - пустой массив

.. code-block:: zephir

    let someVar = "";
    if empty someVar {
        echo "is empty!";
    }

    let someVar = "hello";
    if !empty someVar {
        echo "is not empty!";
    }

Isset
^^^^^
Проверяет, существует ли индекс у массива, или свойство у объекта:

.. code-block:: zephir

    let someArray = ["a": 1, "b": 2, "c": 3];
    if isset someArray["b"] { // проверим, есть ли у массива индекс "b"
        echo "yes, it has an index 'b'\n";
    }

Использование 'isset' возможно в return-конструкциях:

.. code-block:: zephir

    return isset this->{someProperty};

Учтите, что 'isset' в Zephir работает скорее как array_key_exists в PHP.
То есть оператор вернет true даже если значение равно null.

Fetch
^^^^^
Оператор 'fetch' создан для сокращения популярной в PHP конструкции:

.. code-block:: php

    <?php

    if (isset($myArray[$key])) {
        $value = $myArray[$key];
        echo $value;
    }

В Zephir тот же код будет можно написать так:

.. code-block:: zephir

    if fetch value, myArray[key] {
        echo value;
    }

'Fetch' вернет true, если в массиве есть что-то по ключу 'key' и тогда в 'value' будет присвоенно значение.

Type Hints
^^^^^^^^^^
Zephir always tries to check whether an object implements methods and properties called/accessed on a variable that is inferred to be an object:

.. code-block:: zephir

    let o = new MyObject();

    // Zephir checks if "myMethod" is implemented on MyObject
    o->myMethod();

However, due to the dynamism inherited from PHP, sometimes it is not easy to know the class of an object so Zephir can not produce errors reports effectively.
A type hint tells the compiler which class is related to a dynamic variable allowing the compiler to perform more compilation checks:

.. code-block:: zephir

    // Tell the compiler that "o"
    // is an instance of class MyClass
    let o = <MyClass> this->_myObject;
    o->myMethod();

Branch Prediction Hints
^^^^^^^^^^^^^^^^^^^^^^^
What is branch prediction? Check this `article out`_. In environments where performance is very important, it may be useful to introduce these hints.

Consider the following example:

.. code-block:: zephir

    let allPaths = [];
    for path in this->_paths {
        if path->isAllowed() == false {
            throw new App\Exception("error!!");
        } else {
            let allPaths[] = path;
        }
    }

The authors of the above code, know in advance that the condition that throws the exception is unlikely to happen. This means that 99.9% of the time, our method executes that condition, but it is probably never evaluated as true. For the processor, this could be hard to know, so we could introduce a hint there:

.. code-block:: zephir

    let allPaths = [];
    for path in this->_paths {
        if unlikely path->isAllowed() == false {
            throw new App\Exception("error!!");
        } else {
            let allPaths[] = path;
        }
    }

.. _`array_key_exists`: http://www.php.net/manual/en/function.array-key-exists.php
.. _`php документации`: http://www.php.net/manual/en/language.operators.comparison.php
.. _`article out`: http://igoro.com/archive/fast-and-slow-if-statements-branch-prediction-in-modern-processors/
