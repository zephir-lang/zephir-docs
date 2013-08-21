Hello World!
============

Code in Zephir must be placed in classes, Zephir is intended to create object-oriented libraries/frameworks:

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

Once this class is compiled it produce the following code:

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

	}
