---
layout: default
language: 'zh-CN'
version: '0.10'
menu:
  - text:
        '基础要求'
    url: '#prerequisites'
  - text:
        '安装 Zephir'
    url: '#installing-zephir'
  - text:
        '测试安装'
    url: '#testing-the-installation'
---
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
    PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
            with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
    ```
    
    此外, 请确保您在安装 php 的同时安装了 php 开发库:
    
    ```bash
    phpize -v
    Configuring for:
    PHP Api Version:         20151012
    Zend Module Api No:      20151012
    Zend Extension Api No:   320151012
    ```
    
    您不必确切看到的以上输出，但是这些命令对于开始使用Zephir进行开发非常重要。
    
    

<a name='installing-zephir'></a>

    
    ## 安装 Zephir
    
    

<a name='git-way'></a>

    
    ### Git 方式
    
    首先, 确保安装并激活了 Zephir 解析器扩展。 你可以按照这个 [tutorial](https://github.com/phalcon/php-zephir-parser)。
    
    Zephir 编译器当前必须从 github 克隆:
    
    ```bash
    git clone https://github.com/phalcon/zephir
    ```
    
    运行 Zephir 安装程序:
    
    ```bash
    cd zephir
    ./install -c
    ```
    
    最后一件事，你需要确保你有所有必要的依赖和安装额外的PHP库:
    
    ```bash
    composer install
    ```
    
    这个步骤对于0.10版本是可选的。 然而，在未来的版本中它将成为强制性的。
    
    

<a name='testing-the-installation'></a>

    
    ## 测试安装
    
    检查Zephir是否可以从任何目录执行:
    
    ```bash
    zephir 帮助
    ```
