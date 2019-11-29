---
layout: default
language: 'tr-tr'
version: '0.11'
---

# Why Zephir?

Today's PHP applications must balance a number of concerns including stability, performance, and functionality. Every PHP application is based on a set of common components, that are also base for many other applications.

These common components are libraries, frameworks, or a combination of the two. Once installed, frameworks rarely change, and being the foundation of the application, they must be highly functional, and also very fast.

Getting fast and robust libraries can be complicated, due to high levels of abstraction that are typically implemented on them. Given the condition that base libraries or frameworks rarely change, there is an opportunity to build extensions that provide this functionality, taking advantage of the compilation improving performance and resource consumption.

With Zephir, you can implement object-oriented libraries/frameworks/applications that can be used from PHP, gaining important seconds that can make your application faster while improving the user experience.

<a name='if-you-are-a-php-programmer'></a>

## If You Are a PHP Programmer...

PHP is one of the most popular languages in use for the development of web applications. Dynamically typed and interpreted languages like PHP offer very high productivity due to their flexibility.

Since version 4, PHP is based on the Zend Engine implementation. This is a virtual machine that executes the PHP code from its bytecode representation. Zend Engine is present in almost every PHP installation in the world. With Zephir, you can create extensions for PHP running under the Zend Engine.

PHP is hosting Zephir, so they obviously have a lot of similarities; however, they have important differences that give Zephir its own personality. For example, Zephir is more strict, and it could make you less productive compared to PHP due to the compilation step.

<a name='if-you-are-a-c-programmer'></a>

## If You Are a C Programmer...

C is one of the most powerful and popular languages ever created. In fact, PHP is written in C, which is one of the reasons why PHP extensions are available for it. C gives you the freedom to manage memory, use low level types and even inline assembly routines.

However, developing big applications in C can take much longer than expected compared to PHP or Zephir, and some errors can be tricky to find if you aren't an experienced developer.

Zephir was designed to be safe, so it does not implement pointers or manual memory management, so if you're a C programmer, you will feel Zephir less powerful, but more friendly, than C.

<a name='compilation-vs-interpretation'></a>

## Compilation vs Interpretation

Compilation usually slows development down; you will need a bit more patience to compile your code before running it. On the other hand, interpretation tends to reduce code performance in favor of developer productivity. That said, in some cases, there is not any noticeable difference between the speed of interpreted and compiled code.

Zephir requires compilation of your code, but functionality is used from PHP, which is interpreted.

Once the code is compiled, it is not necessary to do so again. Interpreted code is interpreted each time it is run. A developer can decide which parts of their application should be in Zephir and which not.

<a name='statically-typed-versus-dynamically-typed-languages'></a>

## Statically Typed Versus Dynamically Typed Languages

Generally speaking, in a statically typed language, a variable is bound to a particular type for its lifetime. Its type can't be changed and it can only reference type-compatible instances and operations. Languages like C/C++ were implemented with this scheme:

```zephir
int a = 0;
a = "hello"; // not allowed
```

In dynamic typing, the type is bound to the value, not the variable. So, a variable might refer to a value of one type, then be reassigned later to a value of an unrelated type. Javascript/PHP are examples of a dynamically typed languages:

```zephir
var a = 0;
a = "hello"; // allowed
```

Despite their productivity advantages, dynamic languages may not be the best choices for all applications, particularly for very large code bases and high-performance applications.

Optimizing the performance of a dynamic language like PHP is more challenging than for a static language like C. In a static language, optimizers can exploit the type information attached to variables themselves to make decisions. In a dynamic language, fewer such clues are available for the optimizer, making optimization choices harder.

While recent advancements in optimizations for dynamic languages are promising (like JIT compilation), they lag behind the state of the art for static languages. So, if you require very high performance, static languages are probably a safer choice.

Another small benefit of static languages is the extra checking the compiler performs. A compiler can't find logic errors, which are far more significant, but a compiler can find errors in advance that in a dynamic language only can be found in runtime.

Zephir is both statically and dynamically typed, allowing you to take advantage of both approaches where possible.

<a name='compilation-scheme'></a>

## Compilation Scheme

Zephir offers native code generation (currently via compilation to C). A compiler like gcc/clang/vc++ optimizes and compiles the code down to machine code. The following graph shows how the process works:

![compilation scheme](/assets/content/scheme.png)

In addition to the ones provided by Zephir, over time, compilers have implemented and matured a number of optimizations that improve the performance of compiled applications:

* [GCC optimizations](http://gcc.gnu.org/onlinedocs/gcc-4.1.0/gcc/Optimize-Options.html)
* [LLVM passes](http://llvm.org/docs/Passes.html)
* [Visual C/C++ optimizations](http://msdn.microsoft.com/en-us/library/k1ack8f1.aspx)

<a name='code-protection'></a>

## Code Protection

In some circumstances, the compilation does not significantly improve performance. This may be because the bottleneck is located in the I/O bound portion(s) of the application (quite likely) rather than compute/memory bound. However, compiling code could also bring some level of intellectual protection to your application. With Zephir, producing native binaries, you also get the ability to "hide" the original code to users or customers.

<a name='conclusion'></a>

## Conclusion

Zephir was not created to replace PHP or C. Instead, we think it is a complement to them, allowing PHP developers to venture into code compilation and static typing. Zephir is an attempt to join good things from the C and PHP worlds, looking for opportunities to make applications faster.
