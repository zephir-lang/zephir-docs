* * *

layout: default language: 'en' version: '0.11'

* * *

# 安装

要安装 Zephir, 请按照下列步骤操作:

<a name='prerequisites'></a>

## 基础要求

要构建PHP扩展并使用Zephir，您需要满足以下要求：

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* A C编译器，例如 gcc </ 0>> = 4.4或替代方法，例如 clang </ 1>> = 3.0， Visual C ++ </ 2>> = 11或[ Intel C ++](https://software.intel.com/en-us/c-compilers)。 建议使用 `gcc` 4.4 或更高版本</li> 
    
    * [re2c](http://re2c.org/) 0.13.6 或更高版本
    * PHP development headers and tools</ul> 
    
    对于基于 linux 的系统, 您还需要:
    
    * [GNU make](https://www.gnu.org/software/make/) 3.81 or later
    * [autoconf](https://www.gnu.org/software/autoconf/autoconf.html) 2.31 或更高版本
    * [automake](https://www.gnu.org/software/automake/) 1.14 或更高版本
    * libpcre3
    * 在 ubuntu 上 `gcc` 时使用`build-essential` 包 (也可能在其他发行版中使用)
    
    如果您使用的是 ubuntu, 则可以通过以下方式安装所需的包:
    
    ```bash
    sudo apt-get update
    sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
    ```
    
    请注意，阅读本指南时特定版本的库和程序可能有所不同。
    
    由于Zephir代码的编写是在PHP中进行的，所以需要安装最新版本的PHP，并且必须在您的控制台中可用:
    
    ```bash
    php -v
    ```
    
        PHP 7.2.17-0ubuntu0.19.04.1 (cli) (built: Apr 18 2019 18:01:25) ( NTS )
        Copyright (c) 1997-2018 The PHP Group
        Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
            with Zend OPcache v7.2.17-0ubuntu0.19.04.1, Copyright (c) 1999-2018, by Zend Technologies
            with Xdebug v2.6.1, Copyright (c) 2002-2018, by Derick Rethans
        
    
    此外, 请确保您在安装 php 的同时安装了 php 开发库:
    
    ```bash
    phpize -v
    ```
    
        Configuring for:
        PHP Api Version:         20170718
        Zend Module Api No:      20170718
        Zend Extension Api No:   320170718
        
    
    您不必确切看到的以上输出，但是这些命令对于开始使用Zephir进行开发非常重要。
    
    

<a name='installing-zephir'></a>

    
    ## 安装 Zephir
    
    首先, 确保安装并激活了 Zephir 解析器扩展。 你可以按照这个 [tutorial](https://github.com/phalcon/php-zephir-parser)。
    
    ### Release PHAR
    
    The recommended, **officially supported**, and easiest-to-use way to install Zephir is to simply grab the latest release PHAR [from GitHub](https://github.com/phalcon/zephir/releases/latest), and download/move it to somewhere in your `$PATH`. (You'll probably also want to rename it to drop the `.phar` extension, so you can run it as `zephir` instead of `zephir.phar`.)
    
    ### Composer
    
    The PHAR isn't available before 0.11.4, so if you need an older version, you can use Composer, in one of two ways:
    
    #### Global Composer Application
    
    ```bash
    composer global require phalcon/zephir
    ```
    
    There are two approaches to running Zephir at this point. The first is to ensure that `${COMPOSER_HOME}/vendor/bin` is in your `$PATH`, then Zephir should be available as `zephir` on the command line. The second is to simply use `composer global exec zephir` instead.
    
    #### Project Dependency
    
    ```bash
    composer require phalcon/zephir
    ```
    
    Use `composer exec zephir` within the project you installed Zephir in, above, to run it. (Alternately, you can still run `vendor/bin/zephir`.)
    
    ### Git Clone
    
    Finally, you can also simply clone the latest tag from GitHub, install the dependencies, and run Zephir from there:
    
    ```bash
    git clone --depth 1 -b $(git ls-remote --sort=-v:refname https://github.com/phalcon/zephir 0.11.* | head -n1 | awk -F/ '{ print $NF }') https://github.com/phalcon/zephir
    composer install
    ```
    
    You'll need to either use the path to `zephir/zephir`, or create a symlink in a directory in your `$PATH`, to run Zephir using this option.
    
    

<a name='testing-the-installation'></a>

    
    ## 测试安装
    
    检查Zephir是否可以从任何目录执行:
    
    ```bash
    zephir list
    ```