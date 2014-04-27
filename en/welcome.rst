Welcome!
========
Welcome to Zephir, an open source, high-level/domain specific language
designed to ease the creation and maintainability of extensions for PHP
with a focus on type and memory safety.

Some features
-------------
Zephir's main features are:

+-------------------+-----------------------------------------------------+
| Type system       | dynamic/static                                      |
+-------------------+-----------------------------------------------------+
| Memory safety     | pointers or direct memory management aren't allowed |
+-------------------+-----------------------------------------------------+
| Compilation model | ahead of time                                       |
+-------------------+-----------------------------------------------------+
| Memory model      | task-local garbage collection                       |
+-------------------+-----------------------------------------------------+

A small taste
-------------
The following code registers a class with a method that filters variables returning its
alphabetic characters:

.. code-block:: zephir

    namespace MyLibrary;

    /**
     * Filter
     */
    class Filter
    {
        /**
         * Filters a string returning its alpha characters
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

The class can be used from PHP as follows:

.. code-block:: php

  <?php

  $filter = new MyLibrary\Filter();
  echo $filter->alpha("01he#l.lo?/1"); // prints hello
