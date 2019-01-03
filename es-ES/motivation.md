---
layout: default
language: 'es-ES'
version: '0.10'
menu:
  - text:
        'Si eres un programador PHP...'
    url: '#if-you-are-a-php-programmer'
  - text:
        'Si eres un programador C...'
    url: '#if-you-are-a-c-programmer'
  - text:
        'Compilación vs Interpretación'
    url: '#compilation-vs-interpretation'
  - text:
        'Lenguajes de Tipificado Estático vs Dinámico'
    url: '#statically-typed-versus-dynamically-typed-languages'
  - text:
        'Esquema de compilación'
    url: '#compilation-scheme'
  - text:
        'Protección del código'
    url: '#code-protection'
  - text:
        'Conclusión'
    url: '#conclusion'
---
# ¿Por qué Zephir?

Actualmente las aplicaciones PHP deben equilibrar una serie de problemas, la estabilidad, el rendimiento y la funcionalidad. Cada aplicación PHP se basa en un conjunto de componentes comunes, que también son la base para muchas otras aplicaciones.

Estos componentes comunes son librerías, frameworks o una combinación de ellos. Una vez instalados, los frameworks raramente cambian, y siendo la base de la aplicación, deben ser altamente funcionales y también muy rápidos.

Conseguir bibliotecas rápidas y robustas puede ser complicado, debido a los altos niveles de abstracción que típicamente se aplican sobre ellas. Dada la condición de que las librerías base o frameworks raramente cambian, allí hay una oportunidad para crear extensiones que proporcionen esta funcionalidad, tomando ventaja que la compilación mejora el rendimiento y el consumo de recursos.

Con Zephir, se pueden implementar bibliotecas/frameworks/aplicaciones orientadas a objetos que se puedan utilizar desde PHP, ganando segundos importantes, los cuales pueden hacer a la aplicación más rápida, mejorando la experiencia del usuario.

<a name='if-you-are-a-php-programmer'></a>

## Si eres un programador PHP...

PHP es uno de los idiomas más populares en el desarrollo de aplicaciones web. Los lenguajes dinámicamente tipificados e interpretados como PHP ofrecen un productividad muy alta debido a su flexibilidad.

Desde la versión 4, PHP está basado en la implementación del motor Zend. Se trata de una máquina virtual que ejecuta el código PHP desde su representación de código en bytes. El motor Zend está presente en casi todas las instalaciones de PHP en el mundo. Con Zephir, se pueden crear extensiones para PHP que se ejecuten en el motor Zend.

PHP tiene alojando a Zephir, por lo que obviamente tienen muchas similitudes; sin embargo, tienen diferencias importantes ya que Zephir tiene personalidad propia. Por ejemplo, Zephir es más estricto, y podría hacerte menos productivo en comparación con PHP por el paso en compilación.

<a name='if-you-are-a-c-programmer'></a>

## Si eres un programador C...

C es una de los lenguajes más poderosos y populares jamás creados. De hecho, PHP está escrito en C, es una de las razones de que las extensiones PHP estén disponibles para él. C le da la libertad de administrar la memoria, usar tipos de bajo nivel e incluso rutinas de ensamblaje en línea.

Sin embargo, desarrollar grandes aplicaciones en C puede tomar mucho más tiempo de lo esperado, comparado con PHP o Zephir, y algunos errores pueden ser difíciles de encontrar si no eres un programador experimentado.

Zephir fue diseñado para ser seguro, por lo que no implementa punteros o gestión de memoria manual, así que si eres un programador C, sentirá que Zephir es menos potente pero más amigable, que C.

<a name='compilation-vs-interpretation'></a>

## Compilación vs Interpretación

La compilación generalmente retrasa el desarrollo; necesitará un poco más de paciencia para compilar el código antes de ejecutarlo. Por otro lado, la interpretación tiende a reducir el rendimiento del código a favor de la productividad del desarrollador. Dicho esto, en algunos casos, no hay ninguna diferencia notable entre la velocidad del código interpretado y del compilado.

Zephir requiere la compilación de su código, pero la funcionalidad se utiliza desde PHP, que es interpretado.

Una vez que se compila el código, no es necesario hacerlo otra vez. El código interpretado, se interpreta cada vez que se ejecuta. Un desarrollador, puede decidir cuales partes de su aplicación deben estar en Zephir y cuales no.

<a name='statically-typed-versus-dynamically-typed-languages'></a>

## Lenguajes de Tipificado Estático vs Dinámico

En general, en un lenguaje estáticamente tipificado, una variable está limitada a un tipo particular durante toda su vida. No se puede cambiar su tipo y sólo puede hacer referencia a las operaciones e instancias de tipo compatible. Los lenguajes como C o C++ fueron implementados con este esquema:

    int a = 0;
    a = "hello"; // no permitido
    

En tipificado dinámico, el tipo está limitado al valor, no la variable. Por lo tanto, una variable puede hacer referencia a un valor de un tipo, entonces puede reasignarse posteriormente a un valor de un tipo sin relación. JavaScript y PHP son ejemplos de lenguajes dinámicamente tipificados:

    var a = 0;
    a = "hello"; // permitido
    

A pesar de sus ventajas en productividad, los lenguajes dinámicos pueden no ser las mejores opciones para todas las aplicaciones, particularmente para bases de código muy grandes y aplicaciones de alto rendimiento.

Optimizar el rendimiento de un lenguaje dinámico como PHP es más difícil que para un lenguaje estático como C. En un lenguaje estático, los optimizadores pueden aprovechar la información de tipo de las variables para tomar decisiones. En un lenguaje dinámico, hay menos pistas de este tipo disponibles para el optimizador, haciendo que las decisiones de optimización sean más difíciles.

Mientras que los avances recientes en optimizaciones para lenguajes dinámicos son prometedoras (como la compilación JIT), se quedan atrás del estado del arte de los lenguajes estáticos. Por lo tanto, si usted requiere un rendimiento muy elevado, los lenguajes estáticos son probablemente una opción más segura.

Otro pequeño beneficio de los lenguajes estáticos es la comprobación extra que realiza el compilador. Un compilador no puede encontrar errores lógicos, que son mucho más significativos, pero un compilador puede encontrar errores por adelantado que en un lenguaje dinámico solo se pueden encontrar en tiempo de ejecución.

Zephir es tanto estática como dinámicamente tipificado, lo que le permite aprovechar ambos enfoques, siempre que sea posible.

<a name='compilation-scheme'></a>

## Esquema de compilación

Zephir ofrece la generación nativa de código (actualmente a través de la compilación a C). Un compilador como gcc/clang/vc++ optimiza y compila el código hasta código de máquina. El siguiente gráfico muestra como funciona el proceso:

![esquema de compilación](/assets/content/scheme.png)

Además de los proporcionados por Zephir, con el tiempo, los compiladores han implementado y madurado una serie de optimizaciones que mejoran el rendimiento de las aplicaciones compiladas:

* [GCC optimizaciones](http://gcc.gnu.org/onlinedocs/gcc-4.1.0/gcc/Optimize-Options.html)
* [LLVM pases](http://llvm.org/docs/Passes.html)
* [Visual C/C++ optimizaciones](http://msdn.microsoft.com/en-us/library/k1ack8f1.aspx)

<a name='code-protection'></a>

## Protección del código

En algunas circunstancias, la compilación no mejora significativamente el rendimiento. Esto puede deberse a que el cuello de botella se encuentra en la porción o porciones de entrada y salida de la aplicación en lugar del límite de cálculo/memoria. Sin embargo, compilando el código, también podría brindar algún nivel de protección intelectual a su aplicación. Con Zephir, que produce nativos binarios, también tiene la capacidad de "ocultar" el código original a usuarios o clientes.

<a name='conclusion'></a>

## Conclusión

Zephir no fue creado para reemplazar a PHP o C. En cambio, creemos que es un complemento a ellos, invitando a los desarrolladores PHP a aventurarse en la compilación de código y en el tipificado estático. Zephir es un intento para unirse a las cosas buenas de los mundos C y PHP, buscando oportunidades para hacer a las aplicaciones más rápidas.