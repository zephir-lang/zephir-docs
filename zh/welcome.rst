欢迎!
========

欢迎使用Zephir, Zephir是一个开源的，类型与内存安全的高级语言。Zephir设计的设计目的是方便开发与维护
PHP的扩展。

一些特性
-------------
Zephir的主要特性(由于表格中的中文无法对齐所以此段略过不翻译):

+-------------------+-----------------------------------------------------+
| Type system       | dynamic/static                                      |
+-------------------+-----------------------------------------------------+
| Memory safety     | pointers or direct memory management aren't allowed |
+-------------------+-----------------------------------------------------+
| Compilation model | ahead of time                                       |
+-------------------+-----------------------------------------------------+
| Memory model      | task-local garbage collection                       |
+-------------------+-----------------------------------------------------+

小试一下
-------------

下面的代码实现了一个类Filter，这个类中的alpha方法接收一个string类型的参数然后返回字母表字符串：

.. code-block:: zephir

    namespace MyLibrary;

    /**
     * Filter
     */
    class Filter
    {
        /**
         * 过滤字符串，返回字母表中的字母
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

这个类在PHP中这样用：

.. code-block:: php

  <?php

  $filter = new MyLibrary\Filter();
  echo $filter->alpha("01he#l.lo?/1"); // 打印输出 hello
