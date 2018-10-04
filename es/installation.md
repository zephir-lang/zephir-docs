# Instalación

Para instalar el Zephir, siga estos pasos:

<a name='prerequisites'></a>

## Prerequisitos

Para construir una extensión de PHP y usar Zephir necesita los siguientes requisitos:

* [Zephir parser](https://github.com/phalcon/php-zephir-parser) >= 1.1.0
* gcc >= 4.x/clang >= 3.x
* re2c 0.13 o superior
* gnu make 3.81 o superior
* autoconf 2.31 o superior
* automake 1.14 o superior
* libpcre3
* PHP development headers and tools
* The build-essential package when using gcc on Ubuntu (and likely in other distributions as well)

Si usas Ubuntu, puedes instalar los paquetes requeridos de esta forma:

    $ sudo apt-get update
    $ sudo apt-get install git gcc make re2c php php-json php-dev libpcre3-dev build-essential
    

Ya que Zephir está escrito en PHP, es necesario tener una versión reciente de PHP instalado y debe estar disponible en la consola:

    $ php -v
    PHP 7.0.8 (cli) (built: Jun 26 2016 00:59:31) ( NTS )
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
            with Zend OPcache v7.0.8, Copyright (c) 1999-2016, by Zend Technologies
    

También, asegúrese de tener las librerías de desarrollo de PHP instaladas junto con su instalación de PHP:

    $ phpize -v
    Configuring for:
    PHP Api Version:         20151012
    Zend Module Api No:      20151012
    Zend Extension Api No:   320151012
    

No necesariamente tiene que ver el mismo resultado exacto anterior, pero es importante que estos comandos estén disponibles para comenzar a desarrollar con Zephir.

<a name='installing-zephir'></a>

## Instalando Zephir

Primero, asegúrese de tener instalada la extensión Zephir parser y activada siguiendo este [tutorial](https://github.com/phalcon/php-zephir-parser)

El compilador de Zephir actualmente debe ser clonado desde Github:

    $ git clone https://github.com/phalcon/zephir
    

Ejecutar el instalador de Zephir (esto compila y crea el parser):

    $ cd zephir
    $ ./install -c
    

<a name='testing-the-installation'></a>

## Probando la Instalación

Comprobar si Zephir esta disponibles desde cualquier directorio ejecutando:

    $ zephir help