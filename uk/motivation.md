# Чому Zephir?

Сьогоднішні програми на PHP повинні збалансувати ряд проблем, таких як стабільність, продуктивність та функціональність. Кожна PHP програма базується на збірці загальних компонентів, які також є основою для багатьох інших програм.

Цими загальними компонентами є бібліотеки, фреймворки або й те й інше в одному флаконі. Після встановлення фреймворки змінюються рідко і будучи основою для програми мусять бути багатофункціональними, а також дуже швидкими.

Пошук швидких та надійних бібліотек може бути складним, як правило через високі рівні абстракції, які здійснюються над ними. Виходячи з того, що базові бібліотеки або фреймворки змінюються рідко, існує можливість побудувати розширення, які нададуть цю ж функціональність, скориставшись швидкістю вже скомпільованої програми та економією ресурсів.

Із Zephir ви можете реалізувати об'єктно-орієнтовані бібліотеки/фреймворки/програми, які можна використовувати з-під PHP, тим самим зберігши дорогоцінні секунди, які можуть зробити вашу програму швидшою та покращити користувацький досвід.

<a name='if-you-are-a-php-programmer'></a>

## Якщо ви PHP-програміст...

PHP є однією з найпопулярніших мов, що використовуються для розробки веб-програм. Мови, з динамічною типізацію та інтерпретацією, от PHP мають дуже високу продуктивність через їхню гнучкість.

Починаючи з версії 4, PHP базується на реалізації Zend Engine. Це віртуальна машина, яка виконує PHP-код з його байт-код представлення. Zend Engine присутній практично у кожній установці PHP у світі. За допомогою Zephir ви можете створювати розширення для PHP, які працюватимуть під управлінням Zend Engine.

PHP is hosting Zephir, so they obviously have a lot of similarities; however, they have important differences that give Zephir its own personality. Для прикладу, Zephir більш суворіший, що в свою чергу може зменшити вашу продуктивність при написанні коду в порівняння з PHP.

<a name='if-you-are-a-c-programmer'></a>

## Якщо ви C-програміст...

C — одна з найпотужніших та найпопулярніших мов програмування, серед мов, коли-небудь створених. Насправді сам PHP написаний на C. Це одна з причин, чому розширення для PHP сумісні із C. C дає вам свободу керування пам'яттю, використання типів низького рівня та навіть виконання підпрограм асемблера прямо з-під C коду.

Однак, розробка великих програм на C може тривати довше, ніж очікувалося в порівнянні з PHP або Zephir. Крім того деякі помилки може бути складно знайти, якщо ви не є досвідченим розробником.

Zephir був розроблений для безпечної роботи з пам'яттю. Тому в ньому не передбачено реалізації вказівників ручного керування пам'яттю. Отже, якщо ви C-програміст то Zephir для вас відчуватиметься дещо урізаним, проте більш дружнім.

<a name='compilation-vs-interpretation'></a>

## Компіляція проти інтерпретації

Компіляція зазвичай гальмує розробку програм; вам буде потрібно трохи терпіння, щоб скомпілювати код перш ніж запустити його. З іншого боку, інтерпретація має тенденцію до зниження швидкодії програми на користь збільшення темпів розробки продукту. Щоправда, в деяких випадках немає ніякої помітної різниці між швидкістю інтерпретованого та скомпонованого коду.

Zephir вимагає компіляції коду, але функціональність використовується з PHP, який інтерпретується.

Після компіляції коду не потрібно робити це знову. Інтерпретований код інтерпретується кожного разу, коли він запускається. Розробники можуть вирішити, які частини їхньої програми мають бути на Zephir, а які ні.

<a name='statically-typed-versus-dynamically-typed-languages'></a>

## Статичнотипізовані мови проти динамічнотипізованих

Generally speaking, in a statically typed language, a variable is bound to a particular type for its lifetime. Its type can't be changed and it can only reference type-compatible instances and operations. Languages like C/C++ were implemented with this scheme:

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