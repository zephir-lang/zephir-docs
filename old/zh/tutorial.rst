教程
========
Zephir与这本书的目的都是为了让PHP的开发者可以更容易写出PHP的C扩展。

我们假定你至少有一门以上其他语言的开发经验。本书中我们拿PHP，C，Javascript，等语言相对比，我们会指出Zephir语言中的新特性及和
其他语言的不同等。

检查安装
---------------------
如果你已经成功安装了Zephir，则你可以在终端中执行如下的命令:

.. code-block:: bash

    $ zephir help

如果安装成功的话，会显示如下的帮助信息:

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

扩展的骨架
------------------
我们要做的第一件事即是先生成一个扩展的框架，这是我们开始下面工作的基础。在我们的例子中我们创建了一个名为utils的扩展:

.. code-block:: bash

    $ zephir init utils

执行完以上命令后，一个名为utils的目录会在当前的工作目录中创建出来:

.. code-block:: bash

    utils/
       ext/
       utils/

其中根目录utils中的"ext/"文件夹中包含了一些用来生成此扩展的代码。位于utils中的另一个名为utils的目录是你的Zephir代码所在的文件夹。

我们需要先进入utils工作目录之后才能编译我们的代码:

.. code-block:: bash

    $ cd utils
    $ ls
    ext/ utils/ config.json

扩展的根目录中有一个名为config.json的文件，其中包含了一些我们用来控制Zephir扩展行为的配置指令。

添加你的第一个类
----------------------
Zephir语言是完全面向对象的。在开发新功能前我们需要先添加一个类到扩展中。

像很多其他的语言一样，我们要做的第一件事即是写一个简单的"Hello World"程序来检测Zephir是否能正常工作。这里我们写的第一个类是
"Utils\Greeting"其中包含了一个输出"hello world"的方法。

下面的代码必须放在"utils/utils/greeting.zip"文件中:

.. code-block:: zephir

    namespace Utils;

    class Greeting
    {

        public static function say()
        {
            echo "hello world!";
        }

    }

现在我们可以告诉Zephir来编译我们的扩展:

.. code-block:: bash

    $ zephir build


首先在第一次编译时Zephir会执行一些内部命令来生成必须的代码和配置信息到以编译Zephir类到PHP的扩展中，如果一切运行良好的话会有如下输出:

.. code-block:: php

    ...
    Extension installed!
    Add extension=utils.so to your php.ini
    Don't forget to restart your web server

上面的步骤中，你可能需要输入你的root密码以安装扩展护到你的系统中。最后你还需通过一条配置指令extension=utils.so来把这个扩展添加到
php.ini中。

初步测试
---------------
现在这个新开发的PHP扩展已经添加到了php.ini中，可以通过如下命令来检查这个扩展是不是已经成功安装:

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

utils扩展如果已经成功安装了则如下的输出中一定有utils。现在让我们来看一下"hello world"程序是如何执行的。
要实现这个，你需要创建一个PHP文件，然后调用这个静态方法即可:

.. code-block:: php

    <?php

    echo Utils\Greeting::say(), "\n";

祝贺你！你的第一个PHP扩展已经开发完毕。

一个有用的类
--------------
"hello world"类的运行良好说明了开发环境已经安装成功了，现在我们来创建一些更有用的类。

下面我们要添加一个新的类为PHP的开发者提供了简单的筛选功能。这个类名中"Utils\Filter"，其文件在"utils/utils/filter.zep"中:

这个类的基础骨架如下:

.. code-block:: zephir

    namespace Utils;

    class Filter
    {

    }

这个类中有一个帮助PHP开发者去掉不需要字符的过滤方法。第一个方法名为alpha，作用是输出基础的ascii字符，去掉其他字符。
刚开始的第一个版本中我们先写一个循环遍历输出字符串的每个字符:

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

如下是调用方法:

.. code-block:: php

    <?php

    $f = new Utils\Filter();
    $f->alpha("hello");

你会看到如下输出:

.. code-block:: bash

    h
    e
    l
    l
    o

检查输出字符的方法非常简单，我们定义一个filtered来保存结果字符，然后直接返回到调用端:

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

测试代码如下:

.. code-block:: php

    <?php

    $f = new Utils\Filter();
    echo $f->alpha("!he#02l3'121lo."); // prints "hello"

下面的视频中你可以看到如何开发这个扩展的(可悲的是我看不到):

.. raw:: html

   <div align="center"><iframe src="//player.vimeo.com/video/84180223" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

结论
----------
正如你所见这个例子非常简单，我们可以如此简单的就写了一个PHP扩展。我们建议你继续阅读这个手册以便找出更多的关于Zephir的细节内容！
