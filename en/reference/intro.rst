Introducing Zephir
==================
Zephir is a language that addresses the major needs of a PHP developer trying to write and compile code that
can be executed by PHP. It is a dynamically/statically typed, some of its features can be familiar to
PHP developers.

The name Zephir is a contraction of the words Zend Engine/PHP/Intermediate. While this suggests that the
pronunciation should be zephyr, the creators of Zephir actually pronounce it zaefire_.

Hello World!
------------
Every language has its own "Hello World!", in Zephir this introductory example showcase some important
features of this language.

Code in Zephir must be placed in classes, Zephir is intended to create object-oriented libraries/frameworks,
so code out of a class is not allowed. Also a namespace is required:

.. code-block:: javascript

	namespace Test;

	/**
	 * This is a sample class
	 */
	class Hello
	{
		/**
		 * This is a sample method
		 */
		public function say()
		{
			echo "Hello World!";
		}
	}

Once this class is compiled it produce the following code that is transparently compiled by gcc/clang/vc++:

.. code-block:: c

	#ifdef HAVE_CONFIG_H
	#include "config.h"
	#endif

	#include "php.h"
	#include "php_test.h"
	#include "test.h"

	#include "kernel/main.h"

	/**
	 * This is a sample class
	 */
	ZEPHIR_INIT_CLASS(Test_Hello) {
		ZEPHIR_REGISTER_CLASS(Test, Hello, hello, test_hello_method_entry, 0);
		return SUCCESS;
	}

	/**
	 * This is a sample method
	 */
	PHP_METHOD(Test_Hello, say) {
		php_printf("%s", "Hello World!");
	}

Actually, it is not expected that a developer that use Zephir must understand or know C,
however if you have any experience with compilers, php internals or the C language itself,
it would provide a more clear sceneario to the developer when working with Zephir.

.. _zaefire: http://translate.google.com/#en/en/zaefire
