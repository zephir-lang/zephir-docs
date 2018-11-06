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
    
    Since Zephir is written in PHP, you need to have a recent version of PHP installed, and it must be available in your console:
    
    ```bash
    php -v
    PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
            with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
    ```
    
    Also, make sure you have the PHP development libraries installed along with your PHP installation:
    
    ```bash
    phpize -v
    Configuring for:
    PHP Api Version:         20151012
    Zend Module Api No:      20151012
    Zend Extension Api No:   320151012
    ```
    
    You don't have to necessarily see the exact above output, but it's important that these commands are available to start developing with Zephir.
    
    

<a name='installing-zephir'></a>

    
    ## Installing Zephir
    
    

<a name='git-way'></a>

    
    ### Git Way
    
    First make sure that the Zephir parser extension is installed and activated. You can follow this [tutorial](https://github.com/phalcon/php-zephir-parser).
    
    The Zephir compiler currently must be cloned from Github:
    
    ```bash
    git clone https://github.com/phalcon/zephir
    ```
    
    Run the Zephir installer:
    
    ```bash
    cd zephir
    ./install -c
    ```
    
    The last thing you need is to make sure you have all the necessary dependencies and install additional PHP libraries:
    
    ```bash
    composer install
    ```
    
    This step is optional for version 0.10.x, however, it will become mandatory in future versions.
    
    

<a name='testing-the-installation'></a>

    
    ## Testing the Installation
    
    Check if Zephir is available from any directory by executing:
    
    ```bash
    zephir help
    ```