Управляющие структуры
=====================
Zephir реализует упрощенный набор структур управления, присутствующих в подобных языках, таких как C, PHP и т.д.

Условные 
--------

Оператор If
^^^^^^^^^^^^
Оператор 'if' оценивает выражение, выполняет свой блок, если оценка true. Фигурные скобки обязательны, 
'if' может иметь необязательное предложение 'else', несколько конструкций 'if'/'else' могут быть соединены вместе:

.. code-block:: zephir

    if false {
        echo "false?";
    } else {
        if true {
            echo "true!";
        } else {
            echo "neither true nor false";
        }
    }

Вы также можете использовать 'elseif':

.. code-block:: zephir

    if a > 100 {
        echo "слишком много";
    } elseif a < 0 {
        echo "слишком мало";
    } elseif a == 50 {
        echo "идеально!";
    } else {
        echo "сойдет";
    }


Скобки в оцениваемом выражении необязательны:

.. code-block:: zephir

    if a < 0 { return -1; } else { if a > 0 { return 1; } }

Оператор Switch
^^^^^^^^^^^^^^^^
Оператор 'switch' сравнивает выражение с рядом предопределенных значений литералов, 
и выполняет соответствующий блок 'case' или в случае неудачи, выполняет блок 'default':

.. code-block:: zephir

    switch count(items) {
        case 1:
        case 3:
            echo "odd items";
            break;
        case 2:
        case 4:
            echo "even items";
            break;
        default:
            echo "unknown items";
    }

Циклы
-----

Цикл While
^^^^^^^^^^
'While' обозначает цикл, который выполняет итерацию, пока его заданное условие принимает значение true:

.. code-block:: zephir

    let counter = 5;
    while counter {
        let counter -= 1;
    }

Цикл Loop
^^^^^^^^^
В дополнение к 'while', 'loop' может использоваться для создания бесконечных циклов:

.. code-block:: zephir

    let n = 40;
    loop {
        let n -= 2;
        if n % 5 == 0 { break; }
        echo x, "\n";
    }

Цикл For
^^^^^^^^
Цикл 'for' является структурой управления, которая позволяет перебирать массивы или строки:

.. code-block:: zephir

    for item in ["a", "b", "c", "d"] {
        echo item, "\n";
    }

Ключи в хэшах можно получить следующим образом:

.. code-block:: zephir

    let items = ["a": 1, "b": 2, "c": 3, "d": 4];

    for key, value in items {
        echo key, " ", value, "\n";
    }

Цикл 'for' также может быть проинструктирован об обходе массива или строки в обратном порядке:

.. code-block:: zephir

    let items = [1, 2, 3, 4, 5];

    for value in reverse items {
        echo value, "\n";
    }

Цикл 'for' может использоваться для перемещения по строковым переменным:

.. code-block:: zephir

    string language = "zephir"; char ch;

    for ch in language {
        echo "[", ch ,"]";
    }

В обратном порядке:

.. code-block:: zephir

    string language = "zephir"; char ch;

    for ch in reverse language {
        echo "[", ch ,"]";
    }

Стандартный 'for', который проходит диапазон целочисленных значений, можно записать следующим образом:

.. code-block:: zephir

    for i in range(1, 10) {
        echo i, "\n";
    }

Оператор break
^^^^^^^^^^^^^^^
'break' ends execution of the current 'while', 'for' or 'loop' statements:
'break' завершает выполнение текущих операторов 'while', 'for' или 'loop':

.. code-block:: zephir

    for item in ["a", "b", "c", "d"] {
        if item == "c" {
            break; // exit the for
        }
        echo item, "\n";
    }

Оператор  continue
^^^^^^^^^^^^^^^^^^
'continue' используется внутри структур цикла, чтобы пропустить оставшуюся часть текущей итерации цикла и продолжить 
выполнение при оценке условия, а затем в начале следующей итерации.

.. code-block:: zephir

    let a = 5;
    while a > 0 {
        let a--;
        if a == 3 {
            continue;
        }
        echo a, "\n";
    }

Оператор require
----------------
Инструкция 'require' динамически включает и оценивает указанный PHP-файл. Обратите внимание, что файлы, включенные через Zephir,
интерпретируются Zend Engine как обычные PHP-файлы. 'Require' не позволяет включать другие файлы Zephir во время выполнения.

.. code-block:: zephir

    if file_exists(path) {
        require path;
    }

Оператор let
------------
Оператор 'let' используется для изменения переменных, свойств и массивов. Переменные по умолчанию неизменяемые, 
и эта команда делает их изменяемыми:

.. code-block:: zephir

    let name = "Tony";           // simple variable
    let this->name = "Tony";     // object property
    let data["name"] = "Tony";   // array index
    let self::_name = "Tony";    // static property

Также эта инструкция должна использоваться для увеличения/уменьшения переменных:

.. code-block:: zephir

    let number++;           // increment simple variable
    let number--;           // decrement simple variable
    let this->number++;     // increment object property
    let this->number--;     // decrement object property

Множественные изменения могут быть выполнены в единственной операции 'let':

.. code-block:: zephir

    let price = 1.00, realPrice = price, status = false;


