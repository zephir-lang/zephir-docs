Классы и объекты
================
Zephir позволяет создавать только классы, однако также становится возможным использовать
исключения, вмето фатальных ошибок, или предупреждений.

Классы
------
Каждый файл в Zephir должны содержать класс или интерфейс (причем только один). Структура класса
крайне схожа с структурой обьявления его в PHP:

.. code-block:: zephir

    namespace Test;

    /**
     * Это пример класса
     */
    class MyClass
    {

    }

Реализация методов
------------------
Ключевое слово "function" декларирует новый метод. Методы имеют те же модификаторы для разрешения
видимости, что и PHP, однако Zephir требует явно его указывать.

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        public function myPublicMethod()
        {
            // ...
        }

        protected function myProtectedMethod()
        {
            // ...
        }

        private function myPrivateMethod()
        {
            // ...
        }
    }

Методы могут принимать как обязательные так и необязательные параметры:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        /**
         * Все параметры обязательные
         */
        public function doSum1(a, b)
        {
            return a + b;
        }

        /**
         * Обязателен только 'a', 'b' не обязателен и имеет значение по умолчанию
         */
        public function doSum2(a, b=3)
        {
            return a + b;
        }

        /**
         * Оба параметра не обязательны
         */
        public function doSum3(a=1, b=2)
        {
            return a + b;
        }

        /**
         * Параметры обязательны, и они должны быть целочисленными
         */
        public function doSum4(int a, int b)
        {
            return a + b;
        }

        /**
         * Параметры обязательны, целочисленны и имеют значения по умолчанию
         */
        public function doSum4(int a=4, int b=2)
        {
            return a + b;
        }

    }

Поддерживаемые видимости метода (Инкапсуляция)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Открытый (public): Методы с модификатором "public" экспортируются в расширение, это значит, что этими методами можно пользоваться также, как и самому расширению.

* Защищенный (protected): Методы с модификатором "protected" экспортируются в расширение, это значит, что этими методами можно пользоваться также, как и самому расширению. Однако, защищенные методы могут быть вызваны либо внутри класса, либо наследником класса.

* Закрытый (private): Методы с модификатором "private" не экспортируются в расширение, это значит, что эти методы может использовать только класс, в котором метод реализован.


Поддерживаемые модификаторы
^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Устаревший (deprecated): Методы с модификатором "deprecated" бросают ошибку (E_DEPRECATED) где они вызываются.

Сокращения для геттеров/сеттеров
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Как в C#, вы можете использовать сокращения для get/set/toString методов.
Это значит, что вы можете создавать геттеры и сеттеры для свойств не явно.

Рассмотрим как выглядит код без сокращений:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {
        protected myProperty;

        protected someProperty = 10;

        public function setMyProperty(myProperty)
        {
            let this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }

        public function setSomeProperty(someProperty)
        {
            let this->someProperty = someProperty;
        }

        public function getSomeProperty()
        {
            return this->someProperty;
        }

        public function __toString()
        {
            return this->myProperty;
        }

     }

Вы можете написать тот же код, используя сокращения:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        protected myProperty {
            set, get, toString
        };

        protected someProperty = 10 {
            set, get
        };

    }

Zephir сгенерирует реальные методы, но вам не прийдется писать их самостоятельно.

Тип возвращаемого значения
^^^^^^^^^^^^^^^^^^^^^^^^^^
Методы классов и интерфейсов могут объявлять тип возвращаемого значения.
Это поможет компилятору подсказать вам о ошибках в расширении. Рассмотрим пример:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        public function getSomeData() -> string
        {
            // здесь будет сгенерирована ошибка
            // потому что возвращаемое значение (boolean)
            // не соответсвует ранее объявленному типу
            return false;
        }

        public function getSomeOther() -> <App\MyInterface>
        {
            // здесь будет сгенерирована ошибка
            // если возвращаемый объект не реализует
            // ожидаемый компилитором интерфейс
            return new App\MyObject;
        }

        public function process()
        {
            var myObject;

            // the type-hint will tell the compiler that
            // myObject is an instance of a class
            // that implement App\MyInterface
            let myObject = this->getSomeOther();

            // the compiler will check if App\MyInterface
            // implements a method called "someMethod"
            echo myObject->someMethod();
        }

    }

Метод может содержать более одного возвращаемого типа. Для объявления нескольких типов
возвращаемых значений, разделите их оператором '|'.

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        public function getSomeData(a) -> string|bool
        {
            if a == false {
                return false;
            }
            return "error";
        }

    }

Возвращаемый тип: void
^^^^^^^^^^^^^^^^^^^^^^
Методы также могут быть помечены как 'void'. Это значит, что метод не можете вернуть ничего.

.. code-block:: zephir

    public function setConnection(connection) -> void
    {
        let this->_connection = connection;
    }

Почему это полезно? Потому что если компилятор обнаружит, что программа
использует возвращаемое значение, то сгенерирует исключение.

.. code-block:: zephir

    let myDb = db->setConnection(connection);
    myDb->execute("SELECT * FROM robots"); // тут сгенерируется исключение

Строгие/Приводимые Типы Параметров
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
В Zephir вы можете определить тип для каждого параметра в методе. По умолчанию все типизированные
аргументы приводимы. Это зачит, что если значение не соответсвует ожидаемому типу,
Zephir сгенерирует код для его приведения к ожидаемому.

.. code-block:: zephir

    public function filterText(string text, boolean escape=false)
    {
        //...
    }

Этот метод будет работать так:

.. code-block:: php

    <?php

    $o->filterText(1111, 1); // OK
    $o->filterText("some text", null); // OK
    $o->filterText(null, true); // OK
    $o->filterText("some text", true); // OK
    $o->filterText(array(1, 2, 3), true); // Ошибка

Однако, передача не правильного типа часто может вести к багам. Вы можете
запретить автоматическое приведение, используя строгие типы:

.. code-block:: zephir

    public function filterText(string! text, boolean escape=false)
    {
        //...
    }

Теперь большинство вызовов сгенерируют исключения благодяря неправильному переданному типу:

.. code-block:: php

    <?php

    $o->filterText(1111, 1); // Ошибка
    $o->filterText("some text", null); // OK
    $o->filterText(null, true); // Ошибка
    $o->filterText("some text", true); // OK
    $o->filterText(array(1, 2, 3), true); // Ошибка

Определяя какие параметры строгие, а какие приводимые, вы можете контролировать поведение так, этого хочет.

Read-Only Параметры
^^^^^^^^^^^^^^^^^^^^
Используя ключевое слово 'const' вы можете объявить, что параметр только для чтения.
Такие параметры не могут быть изменены внутри метода:

.. code-block:: zephir

    namespace App;

    class MyClass
    {
        // "a" только для чтения
        public function getSomeData(const string a)
        {
            // компилятор сгенерирует ошибку
            let a = "hello";
        }
    }

Когда параметр объявлен только для чтения, компилятор может безопасно создавать оптимизации над этими переменными.

Реализация свойств
------------------
Переменные-члены класса называются "properties". По умолчанию они действуют как свойства PHP. 
Свойства экспортируются в расширение PHP и являются видимыми из кода PHP. 
Свойства реализуют обычные модификаторы видимости, доступные в PHP, 
явно задавать модификатор видимости обязательно в Zephir:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        public myProperty1;

        protected myProperty2;

        private myProperty3;

    }

В пределах методов класса нестатические свойства могут быть доступны с помощью -> (Оператор объекта): this->property (где property - это имя свойства):

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected myProperty;

        public function setMyProperty(var myProperty)
        {
            let this->myProperty = myProperty;
        }

        public function getMyProperty()
        {
            return this->myProperty;
        }

    }

Свойства могут иметь литерально совместимые значения по умолчанию. 
Эти значения должны быть в состоянии быть оценены во время компиляции 
и не должны зависеть от информации времени выполнения для оценки:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected myProperty1 = null;
        protected myProperty2 = false;
        protected myProperty3 = 2.0;
        protected myProperty4 = 5;
        protected myProperty5 = "my value";

    }

Обновление свойств
^^^^^^^^^^^^^^^^^^^
Свойства можно обновить, обратившись к ним с помощью оператора '->':

.. code-block:: zephir

    let this->myProperty = 100;

Zephir проверяет, что свойства существуют, когда программа обращается к ним, если свойство не объявлено, 
вы получите исключение компилятора:

.. code-block:: php

    CompilerException: Property '_optionsx' is not defined on class 
    'App\MyClass' in /Users/scott/cphalcon/phalcon/cache/backend.zep on line 62

          let this->_optionsx = options;
          ------------^

Если вы хотите избежать валидации компилятора или просто создать свойство динамически, 
вы можете заключить имя свойства, используя строковые кавычки:

.. code-block:: zephir

    let this->{"myProperty"} = 100;

Вы также можете использовать простую переменную для обновления свойства, имя свойства будет взято из переменной:

.. code-block:: zephir

    let someProperty = "myProperty";
    let this->{someProperty} = 100;

Чтение свойств 
^^^^^^^^^^^^^^
Свойства можно прочитать, обратившись к ним с помощью оператора '->':

.. code-block:: zephir

    echo this->myProperty;

Как и при обновлении, свойства могут динамически читаться следующим образом:

.. code-block:: zephir

    // Избежание проверки компилятора или чтение динамического пользовательского свойства
    echo this->{"myProperty"};

    //Чтение с использованием имени переменной
    let someProperty = "myProperty";
    echo this->{someProperty}

Константы классов
---------------
Класс может содержать константы класса, которые остаются неизменными и неизменными после компиляции расширения. 
Константы класса экспортируются в расширение PHP, что позволяет им использовать их с PHP.

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;

    }

К константам класса можно получить доступ, используя имя класса и статический оператор (: :):

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        const MYCONSTANT1 = false;
        const MYCONSTANT2 = 1.0;

        public function someMethod()
        {
            return MyClass::MYCONSTANT1;
        }

    }

Взов Методов
---------------
Методы можно вызывать, используя оператор объекта (->), как в PHP:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public function someMethod(c, d)
        {
            return this->_someHiddenMethod(c, d);
        }

    }

Статические методы должны вызываться с использованием статического оператора (: :):

.. code-block:: zephir

    namespace Test;

    class MyClass
    {

        protected static function _someHiddenMethod(a, b)
        {
            return a - b;
        }

        public static function someMethod(c, d)
        {
            return self::_someHiddenMethod(c, d);
        }

    }

Вы можете вызвать методы динамическим способом следующим образом:

.. code-block:: zephir

    namespace Test;

    class MyClass
    {
        protected adapter;

        public function setAdapter(var adapter)
        {
            let this->adapter = adapter;
        }

        public function someMethod(var methodName)
        {
            return this->adapter->{methodName}();
        }

    }
