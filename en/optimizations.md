---
layout: default
language: 'en'
version: '0.10'
---
# Optimizations
Because the code in Zephir is sometimes very high-level, a C compiler might not be able to optimize this code enough.

Zephir, thanks to its AOT (ahead-of-time) compiler, is able to optimize the code at compile time, potentially improving its execution time, or reducing the memory required by the program.

You can enable optimizations by passing the name prefixed by `-f`:

```bash
zephir -fstatic-type-inference -flocal-context-pass
```

Optimizations can be disabled by passing the name prefixed by `-fno-`:

```bash
zephir -fno-static-type-inference -fno-call-gatherer-pass
```

With recent versions of zephir-parser, optimizations can be configured in the config file `config.json`.

The following optimizations are supported:

<a name='call-gatherer-pass'></a>
## call-gatherer-pass
This pass counts how many times a function or method is called within the same method. This allows the compiler to introduce inline caches to avoid method or function lookups:

```zephir
class MyClass extends OtherClass
{

    public function getValue()
    {
        this->someMethod();
        this->someMethod(); // This method is called faster
    }
}
```

<a name='check-invalid-reads'></a>
## check-invalid-reads
This flag will force checking types to detect for invalid reads during the compilation process. This ensures that all variables are properly defined and initialized with their default values (as well as the internal pointers). An example is:

```zephir
namespace Acme;

class ForInRange
{
    public static function forEmpty(var n)
    {
        var i;
        for i in range(1, n) {
            // Do something
        }
    }
}
```

compared to:


```zephir
namespace Acme;

class ForInRange
{
    public static function forEmpty(var n)
    {
        var i = null;
        for i in range(1, n) {
            // Do something
        }
    }
}
```

Both examples are perfectly valid as far as Zephir is concerned. The difference is in the generated C code:

```c
zval *n;

// ...

zephir_fetch_params(1, 1, 0, &n);
```

compared to:

```c
zval *n = NULL;

// ...

zephir_fetch_params(1, 1, 0, &n);
```

It is a good practice to always initialize variables with default values and types for any programming language. Not doing so, could potentially have unintended consequences for the application, and introduce bugs, memory leaks etc. By using the `check-invalid-reads` flag in `config.json` we ensure that pointers are properly initialized along with their respective C variables. Zephir developers will not see a change in their code. This affects the generated C code.

More information concerning on why C pointers need to be nullified in Stack overflow [here](https://stackoverflow.com/q/12253191/1661465).

<a name='constant-folding'></a>
## constant-folding
Constant folding is the process of simplifying constant expressions at compile time. The following code is simplified when this optimization is enabled:

```zephir
public function getValue()
{
    return (86400 * 30) / 12;
}
```

Is transformed into:

```zephir
public function getValue()
{
    return 216000;
}
```

<a name='internal-call-transformation'></a>
## internal-call-transformation
The `internal-call-transformation` is required to generate internal methods, based on their equivalent PHP ones, allowing for the bypass of the PHP userspace for those internal method calls. By default, this optimization is turned off.

This optimization generates 2 implementations per method, one that is exposed in PHP and an internal one.

Exceptions to the above are:

- Only PHP methods allowed to be replaced (eg. we can't do this for Phalcon's methods)
- Closures (`__invoke`) and `__construct` are not supported
- The number of required parameters must exactly match the number of actual parameters
- Does not work for ZendEngine2 (PHP 5.6)

<a name='local-context-pass'></a>
## local-context-pass
This compilation pass moves variables that will be allocated in the heap to the stack. This optimization can reduce the number of memory indirections a program has to do.

<a name='static-constant-class-folding'></a>
## static-constant-class-folding
This optimization replaces values of class constants in compile time:

```zephir
class MyClass
{

    const SOME_CONSTANT = 100;

    public function getValue()
    {
        return self::SOME_CONSTANT;
    }
}
```

Is transformed into:

```zephir
class MyClass
{

    const SOME_CONSTANT = 100;

    public function getValue()
    {
        return 100;
    }
}
```

<a name='static-type-inference'></a>
## static-type-inference
This compilation pass is very important, since it looks for dynamic variables that can potentially be transformed into static/primitive types, which are better optimized by the underlying compiler.

The following code uses a set of dynamic variables to perform some mathematical calculations:

```zephir
public function someCalculations(var a, var b)
{
    var i = 0, t = 1;

    while i < 100 {
        if i % 3 == 0 {
            continue;
        }
        let t += (a - i), i++;
    }

    return i + b;
}
```

Variables `a`, `b`, and `i` are used exclusively in mathematical operations, and can thus be transformed into static variables taking advantage of other compilation passes. After this pass, the compiler automatically rewrites this code to:

```zephir
public function someCalculations(int a, int b)
{
    int i = 0, t = 1;

    while i < 100 {
        if i % 3 == 0 {
            continue;
        }
        let t += (a - i), i++;
    }

    return i + b;
}
```

By disabling this compilation pass, all variables will maintain the type with which they were originally declared, without optimization.

<a name='static-type-inference-second-pass'></a>
## static-type-inference-second-pass
This enables a second type inference pass, which improves the work done based on the data gathered by the first static type inference pass.
