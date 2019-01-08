---
layout: default
language: 'zh-cn'
version: '0.11'
---
# 优化

因为 Zephir 中的代码有时非常高级, 所以 c 编译器可能无法足够地优化此代码。

由于其 AOT (ahead-of-time) 编译器, Zephir能够在编译时优化代码, 有可能缩短其执行时间, 或减少程序所需的内存。

您可以通过传递 `-f` 前缀的名称来启用优化:

```bash
zephir -fstatic-type-inference -flocal-context-pass
```

可以通过传递 `-fno-` 前缀的名称来禁用优化:

```bash
zephir -fno-static-type-inference -fno-call-gatherer-pass
```

使用最新版本的zephir解析器，可以在配置文件`config.json`中配置优化。

支持以下优化:

<a name='call-gatherer-pass'></a>

## call-gatherer-pass

这个遍历计算在同一个方法中调用一个函数或方法的次数。 这允许编译器引入内联缓存, 以避免方法或函数查找:

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

在编译过程中, 这个标志将强制检查类型来检测无效的读取。 这可确保使用默认值 (以及内部指针) 正确定义和初始化所有变量。 一个例子:

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

与之比较：

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

常量折叠是在编译时对常量表达式进行简化的过程。 启用此优化时, 将简化以下代码:

```zephir
public function getValue()
{
    return (86400 * 30) / 12;
}
```

转换为:

```zephir
public function getValue()
{
    return 216000;
}
```

<a name='internal-call-transformation'></a>

## internal-call-transformation

`internal-call-transformation` 需要根据其等效的 php 方法生成内部方法, 从而允许绕过这些内部方法调用的 php 用户空间。 默认情况下, 此优化处于关闭状态。

此优化为每个方法生成2个实现, 一个在 php 中公开, 一个在内部公开。

上述规定的例外情况是:

- 只允许替换 php 方法 (例如。 我们不能这样做的 phalcon 的方法)
- 不支持关闭 (`__invoke`) 和 `__construct`
- 所需参数的数量必须与实际参数的数量完全匹配
- 不能在 ZendEngine2中起作用的 (PHP 5.6)

<a name='local-context-pass'></a>

## local-context-pass

此编译传递将在堆中分配的变量移动到堆栈。 这种优化可以减少程序必须做的内存间接数。

<a name='static-constant-class-folding'></a>

## static-constant-class-folding

此优化将替换编译时的类常量值:

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

转换为:

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

这个编译过程非常重要，因为它寻找的是可能被转换为静态/基本类型的动态变量，底层编译器可以更好地对其进行优化。

下面的代码使用一组动态变量来执行一些数学计算:

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

变量`a`， `b`， `i`仅用于数学运算，因此可以利用其他编译通道转换为静态变量。 在此传递之后, 编译器会自动将此代码重写为:

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

通过禁用此编译过程, 所有变量都将维护最初声明它们的类型, 而不进行优化。

<a name='static-type-inference-second-pass'></a>

## static-type-inference-second-pass

这将启用第二个类型推断传递, 从而改进基于第一个静态类型推断传递所收集的数据所做的工作。
