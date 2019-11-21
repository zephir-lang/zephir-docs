---
layout: default
language: 'es-es'
version: '0.12'
---

# Phpinfo() Secciones

Al igual que la mayoria de las extensiones, las extensiones de Zephir pueden mostrar información en la salida de [phpinfo()](http://php.net/manual/en/function.phpinfo.php). Esta información está usualmente relacionada con directivas, datos de entorno, etc.

Por defecto, cada extensión de Zephir automáticamente agrega una tabla básica de datos a la salida de `phpinfo()` mostrando la versión de la extensión, y cualquier opción INI que soporte la extensión.

Usted puede agregar más directivas agregando la siguiente configuración al archivo `config.json`:

```json
"info": [
    {
        "header": ["Directive", "Value"],
        "rows": [
            ["setting1", "value1"],
            ["setting2", "value2"]
        ]
    },
    {
        "header": ["Directive", "Value"],
        "rows": [
            ["setting3", "value3"],
            ["setting4", "value4"]
        ]
    }
]
```

Esta información se mostrará de la siguiente manera:

![](/assets/content/info.png)