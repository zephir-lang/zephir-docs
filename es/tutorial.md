# Tutorial

Zephir y este libro, están destinados para desarrolladores PHP que desean crear extensiones en C, con una complejidad menor.

Asumimos que usted tiene experiencia en uno o varios lenguajes de programación. Trazaremos paralelismos entre las funciones de PHP, C, Javascript y otros lenguajes. Señalaremos características de Zephir que son similares a otros lenguajes, como también, muchas características que son nuevas o diferentes. Si estás familiarizado con estos lenguajes específicos, usted podrá entender estas comparaciones más rápidamente.

<a name='checking-the-installation'></a>

## Probando la Instalación

Si se ha instalado con éxito Zephir, usted será capaz de ejecutar el siguiente comando en la consola:

    $ zephir help
    

Si todo está bien, debería ver la siguiente ayuda (o algo muy similar):

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
        stubs               Genera extensiones de apendices de PHP
        install             Instalar la extensión (requiere contraseña root)
        version             Muestra la versión actual de Zephir
        compile             Compilar un extensión de Zephir
        api [--theme-path=/path][--output-directory=/path][--theme-options={json}|/path]Genera el API en HTML
        init [namespace]    Inicia una extensión Zephir
        fullclean           Limpia los archivos de objetos generados en la compilación
        builddev            Genera/Compila/Instala una extensión Zephir en modo de desarrollo
        clean               Limpia los archivos de objetos generados en la compilación
        generate            Genera código C desde código Zephir
        help                Muestra esta ayuda
        build               Genera/Compila/Instala una extensión Zephir
    
    Options:
        -f([a-z0-9\-]+)     Habilita las optimizaciones del compilador
        -fno-([a-z0-9\-]+)  Dehabilita las optimizaciones del compilador
        -w([a-z0-9\-]+)     Enciende una advertencia
        -W([a-z0-9\-]+)     Apaga una advertencia
    

<a name='extension-skeleton'></a>

## Esqueleto de la Extensión

Lo primero que tenemos que hacer es generar un esqueleto de extensión. Esto le dará a nuestra extensión la estructura básica que necesitamos para empezar a trabajar. En nuestro caso, vamos a crear una extensión llamada `utils`:

    $ zephir init utils
    

Después de esto, un directorio llamado "utils" es creado en el directorio actual de trabajo:

    utils/
       ext/
       utils/
    

El directorio `ext/` (dentro del directorio utils) contiene el código que utilizará el compilador para producir la extensión. Otro directorio creado es `utils` este directorio tiene el mismo nombre que nuestra extensión. Aquí pondremos nuestro código Zephir.

Necesitamos cambiar el directorio de trabajo a "utils" para comenzar a copilar nuestro código:

    $ cd utils
    $ ls
    ext/ utils/ config.json
    

Al listar el directorio, nos mostrará un archivo llamado `config.json`. Este archivo contiene los ajustes de configuración que podemos usar para alterar el comportamiento de Zephir y/o la extensión en sí.

<a name='adding-our-first-class'></a>

## Agregando nuestra primer clase

Zephir está diseñado para generar extensiones orientadas a objetos. Para empezar a desarrollar funcionalidades, tenemos que añadir nuestra primera clase de la extensión.

Como en muchos lenguajes y herramientas, lo primero que queremos hacer es ver un `Hola mundo` generado por Zephir, y comprobar que todo este bien. Así que nuestra primera clase se llamará `Utils\Greeting` y contendrá un método que imprima `¡Hola mundo!`.

El código para esta clase se debe colocar en `utils/utils/greeting.zep`:

    namespace Utils;
    
    class Greeting
    {
    
        public static function say()
        {
            echo "¡hola mundo!";
        }
    
    }
    

Ahora, debemos decirle a Zephir que nuestro proyecto debe ser compilado y la extensión generada:

    $ zephir build
    

Inicialmente, y sólo por primera vez, una serie de comandos internos son ejecutados produciendo el código necesario y configuraciones para exportar esta clase a la extensión PHP. Si todo va bien, verá el siguiente mensaje al final de la salida:

    ...
    Extension installed!
    Add extension=utils.so to your php.ini
    Don't forget to restart your web server
    

En el paso anterior, es probable que necesite suministrar su contraseña de root para poder instalar la extensión.

Finalmente, hay que añadir la extensión al archivo `php.ini` para ser cargado en PHP. Esto se consigue añadiendo la directiva de inicialización: `extension=utils.so` a él. (Nota: también se puede cargar en la línea de comandos con `-d extension=utils.so`, pero se cargará sólo para esa única solicitud, por lo que tendría que incluir cada vez que desea probar su extensión en el CLI. Agregando la directiva en el `php.ini` se asegurará que está cargada para cada solicitud de ahí en adelante.)

<a name='initial-testing'></a>

## Prueba Inicial

Ahora que la extensión fue agregada a su `php.ini`, compruebe si la extensión esta cargada correctamente, ejecutando lo siguiente:

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
    

La extensión `utils` debe ser parte de la salida, indicando que la extensión se cargo correctamente. Ahora, vemos nuestro `hello world` ejecutado directamente por PHP. Para lograr esto, puede crear un simple archivo PHP que llame al método estático que acabamos de crear:

    <?php
    
    echo Utils\Greeting::say(), "\n";
    

¡Felicitaciones!, tienes tu primer extensión corriendo en PHP.

<a name='a-useful-class'></a>

## Una clase útil

La clase `hello world` estaba bien para probar si el entorno estaba bien. Ahora, vamos a crear algunas clases más útiles.

La primer clase útil que vamos a agregar a esta extensión proveerá facilidades de filtrado a los usuarios. Esta clase es llamada `Utils\Filter` y su código estará ubicado en el archivo `utils/utils/filter.zep`:

El esqueleto básico de esta clase es el siguiente:

    namespace Utils;
    
    class Filter
    {
    
    }
    

La clase contiene métodos de filtrado para ayudar a los usuarios a filtrar caracteres no deseados de las cadenas de texto. El primer método es llamado `alpha`, y su propósito es filtrar solo las letras básicas de los caracteres ASCII. Para comenzar, vamos a recorrer la cadena de texto, imprimiendo cada byte en la salida estándar:

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
    

Cuando invocamos a este método:

    <?php
    
    $f = new Utils\Filter();
    $f->alpha("hola");
    

Usted verá:

    h
    o
    l
    a
    

La comprobación de todos los caracteres de la cadena es sencilla. Ahora crearemos otra cadena con los caracteres filtrados correctos:

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
    

El método completo se puede probar como antes:

    <?php
    
    $f = new Utils\Filter();
    echo $f->alpha("!ho#02l3'121a."); // imprime "hola"
    

En el siguiente video tutorial puedes ver como crear la extensión explicada en este tutorial: <iframe src="//player.vimeo.com/video/84180223" width="500" height="313" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen mark="crwd-mark"></iframe> 

<a name='conclusion'></a>

## Conclusión

Este es un tutorial muy simple y como se puede ver, es fácil empezar a construir una extensión usando Zephir. Te invitamos a seguir leyendo el manual para que puedas descubrir algunas funciones adicionales que ofrece Zephir!