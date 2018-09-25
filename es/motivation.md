# ¿Por qué Zephir?

Actualmente las aplicaciones PHP deben equilibrar una serie de problemas, la estabilidad, rendimiento y funcionalidad. Cada aplicación PHP se basa en un conjunto de componentes comunes, que también son base para muchas otras aplicaciones.

Estos componentes comunes son librerías, frameworks o una combinación de los dos. Una vez instalado, los frameworks raramente cambian, y siendo la base de la aplicación, deben ser altamente funcionales y también muy rápidos.

Conseguir bibliotecas rápidas y robustas puede ser complicado, debido a los altos niveles de abstracción que típicamente se aplican sobre ellas. Dada la condición de que las librerías base o frameworks raramente cambian, allí esta una oportunidad para crear extensiones que proporcionan esta funcionalidad, tomando ventaja de que la compilación mejora rendimiento y el consumo de recursos.

Con Zephir, se puede implementar bibliotecas/frameworks/aplicaciones orientadas a objetos que se pueden utilizar desde PHP, ganando segundos importantes que pueden hacer a la aplicación más rápidas, mejorando la experiencia del usuario.

<a name='if-you-are-a-php-programmer'></a>

## Si eres un programador PHP...

PHP es uno de los idiomas más populares en el desarrollo de aplicaciones web. Los idiomas dinámicamente tipificados e interpretados como PHP ofrecen muy alta productividad debido a su flexibilidad.

Desde la versión 4, PHP está basado en la implementación del motor Zend. Se trata de una máquina virtual que ejecuta el código PHP desde su representación de código en bytes. El motor Zend está presente en casi todas las instalaciones de PHP en el mundo. Con Zephir, se pueden crear extensiones para PHP que se ejecuten en el motor Zend.

PHP tiene alojando a Zephir, por lo que obviamente tienen muchas similitudes; sin embargo, tienen diferencias importantes ya que Zephir tiene propia personalidad. Por ejemplo, Zephir es más estricto, y que podría hacerte menos productivo en comparación con PHP por el paso en compilación.

<a name='if-you-are-a-c-programmer'></a>

## Si eres un programador C...

C es una de los lenguajes más poderosos y populares jamás creados. De hecho, PHP está escrito en C, es una de las razones por qué las extensiones PHP están disponibles para él. C le da la libertad de administrar la memoria, usar tipos de bajo nivel e incluso rutinas de ensamblaje en línea.

Sin embargo, desarrollar grandes aplicaciones en C puede tomar mucho más tiempo de lo esperado, comparado con PHP o Zephir, y algunos errores pueden ser difíciles de encontrar si no eres un programador experimentado.

Zephir fue diseñado para ser seguro, por lo que no implementa punteros o gestión de memoria manual, así que si eres un programador C, usted sentirá que Zephir menos potente pero más amigable, que C.

<a name='compilation-vs-interpretation'></a>

## Compilación vs Interpretación

La compilación generalmente retrasa el desarrollo; necesitará un poco más de paciencia para compilar el código antes de ejecutarlo. Por otro lado, la interpretación tiende a reducir el rendimiento del código a favor de la productividad del desarrollador. Dicho esto, en algunos casos, no hay ninguna diferencia notable entre la velocidad del código interpretado y del compilado.

Zephir requiere la compilación de su código, pero la funcionalidad se utiliza desde PHP, que se interpretado.

Una vez que se compila el código, no es necesario hacerlo otra vez. El código interpretado, se interpreta cada vez que se ejecuta. Un desarrollador puede decidir qué partes de su aplicación deben estar en Zephir y cuales no.

<a name='statically-typed-versus-dynamically-typed-languages'></a>

## Lenguajes de Tipificado Estático vs Dinámico

Generally speaking, in a statically typed language, a variable is bound to a particular type for its lifetime. Its type can't be changed and it can only reference type-compatible instances and operations. Languages like C/C++ were implemented with this scheme:

    int a = 0;
    a = "hello"; // no permitido
    

In dynamic typing, the type is bound to the value, not the variable. So, a variable might refer to a value of one type, then be reassigned later to a value of an unrelated type. Javascript/PHP are examples of a dynamically typed languages:

    var a = 0;
    a = "hello"; // permitido
    

Despite their productivity advantages, dynamic languages may not be the best choices for all applications, particularly for very large code bases and high-performance applications.

Optimizing the performance of a dynamic language like PHP is more challenging than for a static language like C. In a static language, optimizers can exploit the type information attached to variables themselves to make decisions. In a dynamic language, fewer such clues are available for the optimizer, making optimization choices harder.

While recent advancements in optimizations for dynamic languages are promising (like JIT compilation), they lag behind the state of the art for static languages. So, if you require very high performance, static languages are probably a safer choice.

Another small benefit of static languages is the extra checking the compiler performs. A compiler can't find logic errors, which are far more significant, but a compiler can find errors in advance that in a dynamic language only can be found in runtime.

Zephir is both statically and dynamically typed, allowing you to take advantage of both approaches where possible.

<a name='compilation-scheme'></a>

## Esquema de compilación

Zephir offers native code generation (currently via compilation to C). A compiler like gcc/clang/vc++ optimizes and compiles the code down to machine code. The following graph shows how the process works:

![](/images/content/scheme.png)

In addition to the ones provided by Zephir, over time, compilers have implemented and matured a number of optimizations that improve the performance of compiled applications:

* [GCC optimizaciones](http://gcc.gnu.org/onlinedocs/gcc-4.1.0/gcc/Optimize-Options.html)
* [LLVM pases](http://llvm.org/docs/Passes.html)
* [Visual C/C++ optimizaciones](http://msdn.microsoft.com/en-us/library/k1ack8f1.aspx)

<a name='code-protection'></a>

## Protección del código

In some circumstances, the compilation does not significantly improve performance. This may be because the bottleneck is located in the I/O bound portion(s) of the application (quite likely) rather than compute/memory bound. However, compiling code could also bring some level of intellectual protection to your application. With Zephir, producing native binaries, you also get the ability to "hide" the original code to users or customers.

<a name='conclusion'></a>

## Conclusión

Zephir no fue creado para reemplazar a PHP o C. En cambio, creemos que es un complemento a ellos, permitiendo a los desarrolladores PHP a aventurarse en la compilación de código y en el tipificado estático. Zephir es un intento para unirse a las cosas buenas de los mundos C y PHP, buscando oportunidades para hacer a las aplicaciones más rápidas.