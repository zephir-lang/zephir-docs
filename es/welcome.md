# Bienvenido!

Bienvenido a Zephir, un lenguaje de código abierto de alto nivel/dominio específico, diseñado para facilitar la creación y mantenimiento de extensiones para PHP con un enfoque de tipo y cuidado de memoria.

<a name='some-features'></a>

## Algunas características

Las principales características de Zephir son:

| Características       | Descripción                                             |
| --------------------- | ------------------------------------------------------- |
| Tipo de sistema       | dinámica/estática                                       |
| Ahorro de memoria     | punteros o gestión de memoria directa no está permitido |
| Modelo de compilación | adelantado                                              |
| Modelo de memoria     | task-local garbage collection                           |

<a name='a-small-taste'></a>

## A small taste

The following code registers a class with a method that filters variables, returning their alphabetic characters:

    namespace MyLibrary;
    
    /**
     * Filter
     */
    class Filter
    {
        /**
         * Filters a string, returning its alpha charactersa
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

    <?php
    
    $filter = new MyLibrary\Filter();
    echo $filter->alpha("01he#l.lo?/1"); // prints hello