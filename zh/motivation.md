# 为什么选择 Zephir?

今天的 php 应用程序必须平衡许多问题, 包括稳定性、性能和功能。 每个 php 应用程序都基于一组公共组件, 这些组件也是许多其他应用程序的基础。

这些常见组件是库、框架或两者的组合。 一旦安装, 框架很少改变, 并且作为应用程序的基础, 它们必须具有很高的功能, 而且速度也非常快。

获得快速和可靠的库可能会很复杂, 因为通常在它们上实现了高水平的抽象。 考虑到基础库或框架很少更改的限制, 可以利用编译来提高性能和资源消耗, 构建提供此功能的扩展。

使用 Zephir, 您可以实现可从 php 使用的面向对象的库/框架/应用程序, 使您的应用程序更快, 同时改善用户体验。

<a name='if-you-are-a-php-programmer'></a>

## 如果你是一个 php 程序员..。

PHP是web应用程序开发中最流行的语言之一。 像PHP这样的动态类型和解释语言由于其灵活性提供了非常高的生产力。

由于是第4版，PHP基于Zend引擎实现。 这是一个虚拟机，它通过字节码表示来执行PHP代码。 Zend 引擎几乎出现在世界上所有PHP安装中。 使用Zephir，您可以为在Zend引擎下运行的PHP创建扩展。

PHP是Zephir的宿主，显然它们有很多相似之处; 然而，它们有重要的区别，使得Zephir有了自己的个性。 例如，Zephir更加严格，由于编译步骤的原因，它可能使您的生产率低于PHP。

<a name='if-you-are-a-c-programmer'></a>

## 如果你是一个 C 程序员..。

C语言是有史以来最强大、最流行的语言之一。 实际上，PHP是用C编写的，这也是PHP扩展可用的原因之一。 C允许您自由地管理内存，使用低级类型，甚至内联程序集例程。

然而，与PHP或Zephir相比，在C中开发大型应用程序所花费的时间要比预期的长得多，而且如果您不是经验丰富的开发人员，有些错误可能很难发现。

Zephir是为了安全而设计的，因此它不实现指针或手动内存管理，因此如果您是C程序员，您会觉得Zephir不如C程序员强大，但比C程序员更友好。

<a name='compilation-vs-interpretation'></a>

## 编译 vs 解释

编译通常会减慢开发速度; 在运行代码之前，您需要更多的耐心来编译代码。 另一方面，解释倾向于降低代码性能，以提高开发人员的工作效率。 也就是说，在某些情况下，解释代码和编译代码的速度没有明显的区别。

Zephir需要编译您的代码，但其功能是从PHP中使用的，PHP是经过解释的。

一旦编译了代码，就不需要再这样做了。 每次运行解释代码时都会对其进行解释。 开发人员可以决定其应用程序的哪些部分应位于 Zephir 中, 而哪些部分不应该在 Zephir 中。

<a name='statically-typed-versus-dynamically-typed-languages'></a>

## 静态类型化 vs 动态类型化的语言

一般来说, 在静态类型化语言中, 变量在其生存期内绑定到特定类型。 Its type can't be changed and it can only reference type-compatible instances and operations. Languages like C/C++ were implemented with this scheme:

    int a = 0;
    a = "hello"; // not allowed
    

In dynamic typing, the type is bound to the value, not the variable. So, a variable might refer to a value of one type, then be reassigned later to a value of an unrelated type. Javascript/PHP are examples of a dynamically typed languages:

    var a = 0;
    a = "hello"; // allowed
    

Despite their productivity advantages, dynamic languages may not be the best choices for all applications, particularly for very large code bases and high-performance applications.

Optimizing the performance of a dynamic language like PHP is more challenging than for a static language like C. In a static language, optimizers can exploit the type information attached to variables themselves to make decisions. In a dynamic language, fewer such clues are available for the optimizer, making optimization choices harder.

While recent advancements in optimizations for dynamic languages are promising (like JIT compilation), they lag behind the state of the art for static languages. So, if you require very high performance, static languages are probably a safer choice.

Another small benefit of static languages is the extra checking the compiler performs. A compiler can't find logic errors, which are far more significant, but a compiler can find errors in advance that in a dynamic language only can be found in runtime.

Zephir is both statically and dynamically typed, allowing you to take advantage of both approaches where possible.

<a name='compilation-scheme'></a>

## Compilation Scheme

Zephir offers native code generation (currently via compilation to C). A compiler like gcc/clang/vc++ optimizes and compiles the code down to machine code. The following graph shows how the process works:

![compilation scheme](/images/content/scheme.png)

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