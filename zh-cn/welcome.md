---
layout: default
language: 'zh-cn'
version: '0.10'
---

# 欢迎!

Zephir，一种开源的高级语言，旨在简化PHP扩展的创建和可维护性，重点关注类型和内存安全性。

<a name='some-features'></a>

## 一些特征

Zephir 的主要特点是:

| 属性   | 说明             |
| ---- | -------------- |
| 类型系统 | 动态/静态          |
| 内存安全 | 不允许使用指针或直接内存管理 |
| 编译模型 | 预编译            |
| 内存模型 | 任务本地垃圾回收       |

<a name='a-small-taste'></a>

## 一个小尝试

下面的代码使用筛选变量的方法注册一个类, 返回它们的字母字符:

```zephir
namespace MyLibrary;

/**
 * Filter
 */
class Filter
{
    /**
     * 过滤一个字符串，返回它的alpha字符
     *
     * @param string str
     */
    public function alpha(string str)
    {
        char ch; string filtered = "";

        for ch in str {
           if (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') {
              let filtered .= ch;
           }
        }

        return filtered;
    }
}
```

该类可从 php 中使用, 如下所示:

```php
<?php

$filter = new MyLibrary\Filter();
echo $filter->alpha("01he#l.lo?/1"); // prints hello
```

<a name='external-links'></a>

## 外部链接

下面我们收集了您可能感兴趣的外部资源链接:

- [类型系统](https://en.wikipedia.org/wiki/Type_system)
- [内存安全](https://en.wikipedia.org/wiki/Memory_safety)
- [预编译](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)
- [内存管理](https://en.wikipedia.org/wiki/Memory_management)
