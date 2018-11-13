# 优化

因为 Zephir 中的代码有时非常高级, 所以 c 编译器可能无法足够地优化此代码。

由于其 AOT (ahead-of-time) 编译器, Zephir能够在编译时优化代码, 有可能缩短其执行时间, 或减少程序所需的内存。

您可以通过传递 `-f` 前缀的名称来启用优化:

    zephir -fstatic-type-inference -flocal-context-pass
    

可以通过传递 `-fno-` 前缀的名称来禁用优化:

    zephir -fno-static-type-inference -fno-call-gatherer-pass
    

对于最新版本的 zephir-parser, 可以在配置文件 `config.json` 中配置优化。

支持以下优化:

<a name='call-gatherer-pass'></a>

## call-gatherer-pass

This pass counts how many times a function or method is called within the same method. 这允许编译器引入内联缓存, 以避免方法或函数查找:

    class MyClass extends OtherClass
    {
    
        public function getValue()
        {
            this->someMethod();
            this->someMethod(); // This method is called faster
        }
    }
    

<a name='check-invalid-reads'></a>

## check-invalid-reads

在编译过程中, 这个标志将强制检查类型来检测无效的读取。 这可确保使用默认值 (以及内部指针) 正确定义和初始化所有变量。 一个例子:

```zep
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

与之比较：

```zep
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

就Zephir 而言, 这两个例子都是完全有效的。 不同之处在于生成的 c 代码:

```c
zval *n;

// ...

zephir_fetch_params(1, 1, 0, &n);
```

与之比较：

```c
zval *n = NULL;

// ...

zephir_fetch_params(1, 1, 0, &n);
```

对于任何编程语言, 始终使用默认值和类型初始化变量是一种很好的做法。 不这样做, 可能会给应用程序带来意想不到的后果, 并引入错误、内存泄漏等。 通过在`config.json` 中使用 `check-invalid-read`标志我们确保指针和它们各自的C变量被正确初始化。 Zephir 开发人员不会看到他们的代码发生更改。 这将影响生成的C代码。

关于为什么C指针需要在Stack overflow [here ](https://stackoverflow.com/q/12253191/1661465)中无效的更多信息。

<a name='constant-folding'></a>

## constant-folding

Constant folding is the process of simplifying constant expressions at compile time. The following code is simplified when this optimization is enabled:

    public function getValue()
    {
        return (86400 * 30) / 12;
    }
    

Is transformed into:

    public function getValue()
    {
        return 216000;
    }
    

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

    class MyClass
    {
    
        const SOME_CONSTANT = 100;
    
        public function getValue()
        {
            return self::SOME_CONSTANT;
        }
    }
    

Is transformed into:

    class MyClass
    {
    
        const SOME_CONSTANT = 100;
    
        public function getValue()
        {
            return 100;
        }
    }
    

<a name='static-type-inference'></a>

## static-type-inference

This compilation pass is very important, since it looks for dynamic variables that can potentially be transformed into static/primitive types, which are better optimized by the underlying compiler.

The following code uses a set of dynamic variables to perform some mathematical calculations:

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
    

Variables `a`, `b`, and `i` are used exclusively in mathematical operations, and can thus be transformed into static variables taking advantage of other compilation passes. After this pass, the compiler automatically rewrites this code to:

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
    

By disabling this compilation pass, all variables will maintain the type with which they were originally declared, without optimization.

<a name='static-type-inference-second-pass'></a>

## static-type-inference-second-pass

This enables a second type inference pass, which improves the work done based on the data gathered by the first static type inference pass.