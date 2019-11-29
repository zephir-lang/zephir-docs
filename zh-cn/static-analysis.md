---
layout: default
language: 'en'
version: '0.10'
---

# 静态分析

Zephir 的编译器提供对已编译代码的静态分析。 此功能背后的想法是在运行时之前帮助开发人员发现潜在的问题, 避免意外的行为 。

<a name='conditional-unassigned-variables'></a>

## 条件未分配的变量

工作分配的静态分析尝试确定变量在赋值之前是否已使用:

```zephir
class Utils
{
    public function someMethod(b)
    {
        string a; char c;

        if b == 10 {
            let a = "hello";
        }

        //a could be unitialized here
        for c in a {
            echo c, PHP_EOL;
        }
    }
}
```

上面的示例说明了一种常见情况。 只有当 `b` 等于 10时, 才会分配变量 `a`, 然后需要使用此变量的值--但它可能未初始化。 Zephir 检测到这一点, 自动将变量初始化为空字符串, 并生成警告开发人员:

```bash
警告:第一次在条件分支中分配变量a，
考虑在声明中初始化它

/home/scott/test/test/utils.zep on 21 [conditional-initialization]

    for c in a {
```

发现这样的错误有时是很棘手的, 但是静态分析可以帮助程序员提前发现错误。

<a name='dead-code-elimination'></a>

## 死码消除

Zephir 通知开发人员代码中无法访问的分支, 并执行死代码消除, 这意味着它将从生成的二进制文件中删除所有代码, 因为它无论如何都无法执行:

```zephir
class Utils
{
    public function someMethod(b)
    {
        if false {
            // This is never executed
            echo "hello";
        }
    }
}
```
