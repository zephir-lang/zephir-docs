Built-In Methods
================
As mentioned before, Zephir promotes object-oriented programming, variables related to static types can be also handled as objects.

Compare these two methods:

.. code-block:: zephir

    public function binaryToHex(string! s) -> string
    {
        var o = "", n; char ch;

        for ch in range(0, strlen(s)) {
            let n = sprintf("%X", ch);
            if strlen(n) < 2 {
                let o .= "0" . n;
            } else {
                let o .= n;
            }
        }
        return o;
    }

And:

.. code-block:: zephir

    public function binaryToHex(string! s) -> string
    {
        var o = "", n; char ch;

        for ch in range(0, s->length()) {
            let n = ch->toHex();
            if n->length() < 2 {
                let o .= "0" . n;
            } else {
                let o .= n;
            }
        }
        return o;
    }

They both have the same functionality, but the second one uses object-oriented programming. Calling methods on static-typed variables
does not have any impact on performance since Zephir internally transforms the code from the object-oriented version to the procedural version.

String
^^^^^^

The following string built-in methods are available:

+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| OO                | Procedural                                          | Description                                                                      |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| s->length()       | strlen(s)                                           | Get string length                                                                |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| s->trim()         | trim(s)                                             | Strip whitespace (or other characters) from the beginning and end of a string    |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| s->index("foo")   | strpos(s, "foo")                                    | Find the position of the first occurrence of a substring in a string             |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| s->lower()        | strtolower(s)                                       | Make a string lowercase                                                          |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| s->upper()        | strtoupper(s)                                       | Make a string uppercase                                                          |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+

Array
^^^^^

The following array built-in methods are available:

+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| OO                | Procedural                                          | Description                                                                      |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| a->join(" ")      | join(" ", a)                                        | Join array elements with a string                                                |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+
| a->reverse()      | array_reverse(a)                                    | Return an array with elements in reverse order                                   |
+-------------------+-----------------------------------------------------+----------------------------------------------------------------------------------+

Char
^^^^

The following char built-in methods are available:

+-------------------+-----------------------------------------------------+
| OO                | Procedural                                          |
+-------------------+-----------------------------------------------------+
| ch->toHex()       | sprintf("%X", ch)                                   |
+-------------------+-----------------------------------------------------+

Integer
^^^^^^^

The following integer built-in methods are available:

+-------------------+-----------------------------------------------------+
| OO                | Procedural                                          |
+-------------------+-----------------------------------------------------+
| i->abs()          | abs(i)                                              |
+-------------------+-----------------------------------------------------+

