# Чому Zephir?

Сьогоднішні програми на PHP повинні збалансувати ряд проблем, таких як стабільність, продуктивність та функціональність. Every PHP application is based on a set of common components, that are also base for many other applications.

Цими загальними компонентами є бібліотеки, фреймворки або й те й інше в одному флаконі. Once installed, frameworks rarely change, and being the foundation of the application, they must be highly functional, and also very fast.

Getting fast and robust libraries can be complicated, due to high levels of abstraction that are typically implemented on them. Given the condition that base libraries or frameworks rarely change, there is an opportunity to build extensions that provide this functionality, taking advantage of the compilation improving performance and resource consumption.

With Zephir, you can implement object-oriented libraries/frameworks/applications that can be used from PHP, gaining important seconds that can make your application faster while improving the user experience.

<a name='if-you-are-a-php-programmer'></a>

## Якщо ви PHP-програміст...

PHP є однією з найпопулярніших мов, що використовуються для розробки веб-програм. Dynamically typed and interpreted languages like PHP offer very high productivity due to their flexibility.

Починаючи з версії 4, PHP базується на реалізації Zend Engine. This is a virtual machine that executes the PHP code from its bytecode representation. Zend Engine присутній практично у кожній установці PHP у світі. With Zephir, you can create extensions for PHP running under the Zend Engine.

PHP is hosting Zephir, so they obviously have a lot of similarities; however, they have important differences that give Zephir its own personality. For example, Zephir is more strict, and it could make you less productive compared to PHP due to the compilation step.

<a name='if-you-are-a-c-programmer'></a>

## Якщо ви C-програміст...

C — одна з найпотужніших та найпопулярніших мов програмування, серед мов, коли-небудь створених. In fact, PHP is written in C, which is one of the reasons why PHP extensions are available for it. C gives you the freedom to manage memory, use low level types and even inline assembly routines.

However, developing big applications in C can take much longer than expected compared to PHP or Zephir, and some errors can be tricky to find if you aren't an experienced developer.

Zephir was designed to be safe, so it does not implement pointers or manual memory management, so if you're a C programmer, you will feel Zephir less powerful, but more friendly, than C.

<a name='compilation-vs-interpretation'></a>

## Компіляція проти інтерпретації

Компіляція зазвичай гальмує розробку програм; вам буде потрібно трохи терпіння, щоб скомпілювати код перш ніж запустити його. On the other hand, interpretation tends to reduce code performance in favor of developer productivity. That said, in some cases, there is not any noticeable difference between the speed of interpreted and compiled code.

Zephir вимагає компіляції коду, але функціональність використовується з PHP, який інтерпретується.

Після компіляції коду не потрібно робити це знову. Інтерпретований код інтерпретується кожного разу, коли він запускається. Adeveloper can decide which parts of their application should be in Zephir and which not.

<a name='statically-typed-versus-dynamically-typed-languages'></a>

## Статичнотипізовані мови проти динамічнотипізованих

У цілому, в статичнотипізованій мові змінна пов'язана з певним типом протягом всього життєвого циклу програми. Тип змінної неможливо змінити і вона може лише посилатися на сумісні з типом екземпляри та операції. Мови, такі як C/C++, були реалізовані за такою схемою:

    int a = 0;
    a = "hello"; // not allowed
    

При динамічнотипізованих мовах тип прив'язується до значення, а не до змінної. Таким чином, змінна може мати значення одного типу, а потім бути перевизначеною значенням невизначеного типу. JavaScript/PHP є прикладами мов з динамічною типізацією:

    var a = 0;
    a = "hello"; // allowed
    

Despite their productivity advantages, dynamic languages may not be the best choices for all applications, particularly for very large code bases and high-performance applications.

Optimizing the performance of a dynamic language like PHP is more challenging than for a static language like C. In a static language, optimizers can exploit the type information attached to variables themselves to make decisions. In a dynamic language, fewer such clues are available for the optimizer, making optimization choices harder.

While recent advancements in optimizations for dynamic languages are promising (like JIT compilation), they lag behind the state of the art for static languages. So, if you require very high performance, static languages are probably a safer choice.

Another small benefit of static languages is the extra checking the compiler performs. A compiler can't find logic errors, which are far more significant, but a compiler can find errors in advance that in a dynamic language only can be found in runtime.

Zephir is both statically and dynamically typed, allowing you to take advantage of both approaches where possible.

<a name='compilation-scheme'></a>

## Схема компіляції

Zephir offers native code generation (currently via compilation to C). A compiler like gcc/clang/vc++ optimizes and compiles the code down to machine code. The following graph shows how the process works:

![](/images/content/scheme.png)

In addition to the ones provided by Zephir, over time, compilers have implemented and matured a number of optimizations that improve the performance of compiled applications:

* [GCC optimizations](http://gcc.gnu.org/onlinedocs/gcc-4.1.0/gcc/Optimize-Options.html)
* [LLVM passes](http://llvm.org/docs/Passes.html)
* [Visual C/C++ optimizations](http://msdn.microsoft.com/en-us/library/k1ack8f1.aspx)

<a name='code-protection'></a>

## Code Protection

In some circumstances, the compilation does not significantly improve performance. This may be because the bottleneck is located in the I/O bound portion(s) of the application (quite likely) rather than compute/memory bound. However, compiling code could also bring some level of intellectual protection to your application. With Zephir, producing native binaries, you also get the ability to "hide" the original code to users or customers.

<a name='conclusion'></a>

## Висновок

Zephir was not created to replace PHP or C. Instead, we think it is a complement to them, allowing PHP developers to venture into code compilation and static typing. Zephir is an attempt to join good things from the C and PHP worlds, looking for opportunities to make applications faster.