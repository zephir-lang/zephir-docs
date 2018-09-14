Урок
====
Zephir и эта книга предназначены для разработчиков PHP, которые хотят создавать C-расширения с меньшей сложностью.

Мы предполагаем, что вы знакомы с одним или несколькими языками программирования. Мы проводим параллели с функциями в PHP,
C, Javascript и других языках. Если вы знаете какой-либо из этих языков, мы укажем на похожие функции в Zephir, а также
много новых или отличий.

Проверка установки
------------------
Если вы успешно установили Zephir, вы должны быть в состоянии выполнить следующую команду в своей консоли:

.. code-block:: bash

    $ zephir help

Если все в порядке, на вашем экране должна появиться следующая справка:

.. code-block:: bash

     _____              __    _
    /__  /  ___  ____  / /_  (_)____
      / /  / _ \/ __ \/ __ \/ / ___/
     / /__/  __/ /_/ / / / / / /
    /____/\___/ .___/_/ /_/_/_/
             /_/

    Zephir version 0.10.9a-dev

    Usage:
        command [options]

    Available commands:
        stubs               Generates extension PHP stubs
        install             Installs the extension (requires root password)
        version             Shows the Zephir version
        compile             Compile a Zephir extension
        api [--theme-path=/path][--output-directory=/path][--theme-options={json}|/path]Generates a HTML API
        init [namespace]    Initializes a Zephir extension
        fullclean           Cleans the generated object files in compilation
        builddev            Generate/Compile/Install a Zephir extension in development mode
        clean               Cleans the generated object files in compilation
        generate            Generates C code from the Zephir code
        help                Displays this help
        build               Generate/Compile/Install a Zephir extension

    Options:
        -f([a-z0-9\-]+)     Enables compiler optimizations
        -fno-([a-z0-9\-]+)  Disables compiler optimizations
        -w([a-z0-9\-]+)     Turns a warning on
        -W([a-z0-9\-]+)     Turns a warning off

Каркас расширения
-----------------
Первое, что нам нужно сделать, это сгенерировать скелет расширения, это предоставит нашему расширению базовую структуру,
которую мы должны начать работать. В нашем случае мы создадим расширение под названием "utils":

.. code-block:: bash

    $ zephir init utils

После этого в текущем рабочем каталоге создается каталог с именем "utils":

.. code-block:: bash

    utils/
       ext/
       utils/

Каталог "ext/" (внутри utils) содержит код, который будет использоваться компилятором для создания расширения.
Другой созданный каталог - "utils", этот каталог имеет то же самое, что и наше расширение. Мы разместим код Zephir
в этом каталоге.

Нам нужно изменить рабочий каталог на "utils", чтобы начать компилировать наш код:

.. code-block:: bash

    $ cd utils
    $ ls
    ext/ utils/ config.json

В листинге каталога также будет отображаться файл с именем «config.json». Этот файл содержит параметры конфигурации,
которые мы можем использовать для изменения поведения Zephir и/или этого расширения.

Добавление нашего первого класса
--------------------------------
Zephir предназначен для создания объектно-ориентированных расширений. Чтобы начать разработку функциональности,
нам нужно добавить наш первый класс к расширению.

Как и во многих языках/инструментах, первое, что мы хотим сделать, это увидеть «hello world»,
сгенерированный Zephir, и проверить, что все в порядке. Итак, наш первый класс будет называться «Utils\\Greeting»
и он содержит метод печати «hello world!».

Код для этого класса должен быть помещен в "utils/utils/greeting.zep":

.. code-block:: zephir

    namespace Utils;

    class Greeting
    {

        public static function say()
        {
            echo "hello world!";
        }

    }

Теперь нам нужно сказать Zephir, что наш проект должен быть скомпилирован и расширение сгенерировано:

.. code-block:: bash

    $ zephir build

Изначально и только в первый раз выполняется ряд внутренних команд, создающих необходимый код и конфигурации,
чтобы экспортировать этот класс в расширение PHP, если все пойдет хорошо, вы увидите следующее сообщение
в конце вывода:

.. code-block:: php

    ...
    Extension installed!
    Add extension=utils.so to your php.ini
    Don't forget to restart your web server

На этом этапе вполне вероятно, что вам потребуется указать пароль root, чтобы установить расширение.
Наконец, расширение должно быть добавлено в php.ini для загрузки PHP. Это достигается добавлением директивы
инициализации: extension=utils.so к нему.

Первоначальное тестирование
---------------------------
Теперь, когда расширение было добавлено в ваш php.ini, проверьте, правильно ли загружается расширение,
выполнив следующее:

.. code-block:: bash

    $ php -m
    [PHP Modules]
    Core
    date
    libxml
    pcre
    Reflection
    session
    SPL
    standard
    tokenizer
    utils
    xdebug
    xml

Расширения «utils» должны быть частью вывода, указывающего, что расширение было загружено правильно.
Теперь давайте посмотрим на наш «hello world», непосредственно выполняемый PHP.
Для этого вы можете создать простой PHP-файл, вызывающий статический метод, который мы только что создали:

.. code-block:: php

    <?php

    echo Utils\Greeting::say(), "\n";

Поздравляем !, у вас есть первое расширение, работающее на PHP.

Удобные класс
-------------
Класс «hello world» был хорош, чтобы проверить, правильная ли наша среда, теперь давайте создадим еще
несколько полезных классов.

Первый полезный класс, который мы добавим к этому расширению, предоставит пользователям средства фильтрации.
Этот класс называется «Utils\\Filter», и его код должен быть помещен в «utils/utils/filter.zep»:

Основным скелетом этого класса является следующее:

.. code-block:: zephir

    namespace Utils;

    class Filter
    {

    }

Класс содержит методы фильтрации, которые помогают пользователям фильтровать нежелательные символы из строк.
Первый метод называется "alpha", и его целью является отфильтровать только те символы, которые являются
основными буквами ascii. Для начала, мы просто пройдем через строковую печать каждого байта в стандартный вывод:

.. code-block:: zephir

    namespace Utils;

    class Filter
    {

        public function alpha(string str)
        {
            char ch;

            for ch in str {
                echo ch, "\n";
            }

        }

    }

При вызове этого метода:

.. code-block:: php

    <?php

    $f = new Utils\Filter();
    $f->alpha("hello");

Ты увидишь:

.. code-block:: bash

    h
    e
    l
    l
    o

Проверка каждого символа в строке проста, теперь мы можем просто создать другую строку
с правильными отфильтрованными символами:

.. code-block:: zephir

    class Filter
    {

        public function alpha(string str) -> string
        {
            char ch; string filtered = "";

            for ch in str {
                if (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') {
                    let filtered .= ch;
                }
            }

            return filtered;
        }
    }

Полный метод может быть проверен, как и прежде:

.. code-block:: php

    <?php

    $f = new Utils\Filter();
    echo $f->alpha("$he$02l3'121lo."); // prints "hello"

В следующем скринкасте вы можете посмотреть, как создать расширение, объясненное в этом уроке:

.. raw:: html

   <div align="center"><iframe src="//player.vimeo.com/video/84180223" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

Заключение
----------
Это очень простой учебник, и, как вы можете видеть, легко начать создание расширений с помощью Zephir.
Мы приглашаем вас продолжить чтение руководства, чтобы вы могли ознакомиться с дополнительными функциями, которые предлагает Zephir!
