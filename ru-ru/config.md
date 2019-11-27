* * *

layout: default language: 'ru-ru' version: '0.10' menu:

- text: 'api' url: '#api'
- text: 'author' url: '#author'
- text: 'backend' url: '#backend'
- text: 'constants-sources' url: '#constants-sources'
- text: 'description' url: '#description'
- text: 'destructors' url: '#destructors'
- text: 'extension-name' url: '#extension-name'
- text: 'external-dependencies' url: '#external-dependencies'
- text: 'extra' url: '#extra'
- text: 'extra-cflags' url: '#extra-cflags'
- text: 'extra-classes' url: '#extra-classes'
- text: 'extra-libs' url: '#extra-libs'
- text: 'extra-sources' url: '#extra-sources'
- text: 'globals' url: '#globals'
- text: 'info' url: '#info'
- text: 'initializers' url: '#initializers'
- text: 'name' url: '#name'
- text: 'namespace' url: '#namespace'
- text: 'optimizations' url: '#optimizations'
- text: 'optimizer-dirs'  
    url: '#optimizer-dirs'
- text: 'package-dependencies' url: '#package-dependencies'
- text: 'prototype-dir' url: '#prototype-dir'
- text: 'requires' url: '#requires'
- text: 'silent' url: '#silent'
- text: 'stubs' url: '#stubs'
- text: 'verbose' url: '#verbose'
- text: 'version' url: '#version'
- text: 'warnings' url: '#warnings'

* * *

# Конфигурационный файл

Каждое расширение Zephir имеет файл конфигурации, называемый `config.json`. Этот файл читается Zephir каждый раз, когда вы создаете или создаете расширение, и это позволяет разработчику изменять расширение или поведение компилятора.

Этот файл использует формат [JSON](http://en.wikipedia.org/wiki/JSON) в качестве формата конфигурации:

```json
{
    "namespace": "test",
    "name": "Test Extension",
    "description": "My amazing extension",
    "author": "Tony Hawk",
    "version": "1.2.0"
}
```

Параметры, определенные в этом файле, переопределяют любые заводские настройки, предоставляемые Zephir.

Поддерживаются следующие параметры:

<a name='api'></a>

## api

Используется для настройки автоматически сгенерированной HTML-документации для вашего расширения. `path` указывает, где создать документацию относительно корня проекта. `base-url` используется для генерации файла `sitemap.xml` для вашей документации. `theme` используется для установки темы, используемой в сгенерированной документации (с помощью настройки `name`), и любые опции, поддерживаемые темой (с помощью настройки `options`). И наконец, `theme-directories` используются для предоставления дополнительных путей поиска вашей темы:

```json
{
    "api": {
        "path": "doc/%version%",
        "base-url": "http://example.local/api/",
        "theme": {
            "name"   : "zephir",
            "options": {
                "github":           null,
                "analytics":        null,
                "main_color":       "#3E6496",
                "link_color":       "#3E6496",
                "link_hover_color": "#5F9AE7"
            }
        },
        "theme-directories": [
            "my/api/themes"
        ]
    }
}
```

<a name='author'></a>

## author

Компания, разработчик, учреждение и т.д., которые разработали расширение:

```json
{
    "author": "Tony Hawk"
}
```

<a name='backend'></a>

## backend

Предоставляет способ настройки бэкэнда Zend Engine, используемого в вашем расширении. На данный момент поддерживается только`templatepath`, который позволяет выбрать между `ZendEngine2` и `ZendEngine3`:

```json
{
    "backend": {
        "templatepath": "ZendEngine3"
    }
}
```

<a name='constants-sources'></a>

## constants-sources

Чтобы импортировать только константы из исходного файла C в ваш проект, укажите путь к файлу в этой настройке:

```json
{
    "constants-sources": [
        "utils/math_constants.h"
    ]
}
```

<a name='description'></a>

## description

Описание расширения — любой текст, описывающий ваше расширение:

```json
{
    "description": "My amazing extension"
}
```

<a name='destructors'></a>

## destructors

Этот параметр позволяет предоставить одну или несколько функций C, которые будут выполняться для определенных событий жизненного цикла расширения, в частности, `RSHUTDOWN` (`request`), `PRSHUTDOWN` (`post-request`), `MSHUTDOWN` (`module`), и `GSHUTDOWN` (`globals`). Для получения более подробной информации обратитесь к главе [Хуки времени выполнения](/0.10/en/lifecycle).

```json
{
    "destructors": [
        {
            "request": [
                {
                    "include": "my/awesome/library.h",
                    "code": "c_function_for_shutting_down(TSRMLS_C)"
                },
                {
                    "include": "my/awful/library.h",
                    "code": "some_other_c_function_than_the_other_ones(TSRMLS_C)"
                }
            ],
            "post-request": [
                {
                    "include": "my/awesome/library.h",
                    "code": "c_function_for_cleaning_up_after_the_response_is_sent(TSRMLS_C)"
                }
            ],
            "module": [
                {
                    "include": "my/awesome/library.h",
                    "code": "release_module_deps(TSRMLS_C)"
                }
            ],
            "globals": [
                {
                    "include": "my/awesome/library.h",
                    "code": "release_globals_deps(TSRMLS_C)"
                }
            ]
        }
    ]
}
```

<a name='extension-name'></a>

## extension-name

Имя базового файла расширения. Оно должно следовать тем же правилам, что и настройка `namespace`, которая используется как запасной вариант в случае, если эта не указана.

```json
{
    "extension-name": "test"
}
```

<a name='external-dependencies'></a>

## external-dependencies

Вы можете включить класс из другого пространства имён/расширения непосредственно в вашем собственном расширении, настроив его как показано здесь:

```json
{
    "external-dependencies": {
        "My\\Awesome": "my/awesome/class.zep",
        "My\\Awful": "my/awful/class.zep"
    }
}
```

<a name='extra'></a>

## extra

Содержит дополнительные параметры, которые также могут быть переданы также в командной строке. В настоящее время это — `export-clases` (генерирует заголовочные файлы для доступа к вашим классам из другого C кода), и `indent` (использование `табуляции` или `пробелов` для отступов в коде генерируемых файлов):

```json
{
    "extra": {
        "export-classes": true,
        "indent": "tabs"
    }
}
```

<a name='extra-cflags'></a>

## extra-cflags

Любые дополнительные флаги, которые вы хотите добавить в процесс компиляции:

```json
{
    "extra-cflags": "-I/usr/local/Cellar/libevent/2.0.21_1/include"
}
```

<a name='extra-classes'></a>

## extra-classes

Если у вас уже есть PHP-класс, реализованный в C, Вы можете включить его непосредственно в ваше расширение, как показано ниже:

```json
{
    "extra-classes": [
        {
            "header": "utls/old_c_class/class.h",
            "source": "utils/old_c_class/class.c",
            "init": "old_c_class",
            "entry": "old_c_class_ce"
        }
    ]
}
```

<a name='extra-libs'></a>

## extra-libs

Любые дополнительные библиотеки, которые вы хотите добавить в процесс компиляции:

```json
{
    "extra-libs": "-L/usr/local/Cellar/libevent/2.0.21_1/lib -levent"
}
```

<a name='extra-sources'></a>

## extra-sources

Любые дополнительные файлы, которые вы хотите добавить в процесс компиляции. Каталог поиска находится относительно папки `ext` вашего проекта:

```json
{
    "extra-sources": [
        "utils/pi.c"
    ]
}
```

<a name='globals'></a>

## globals

Доступные для расширения глобальные параметры. Обратитесь к главе [Глобальные параметры расширения](/0.10/en/globals) для получения дополнительной информации.

```json
{
    "globals": {
        "my_setting_1": {
            "type": "bool",
            "default": true
        },
        "my_setting_2": {
            "type": "int",
            "default": 10
        }
    }
}
```

<a name='info'></a>

## info

Секции `phpinfo()`. Обратитесь к главе [Секции phpinfo()](/0.10/en/phpinfo) для получения дополнительной информации.

```json
{
    "info": [
        {
            "header": ["Directive", "Value"],
            "rows": [
                ["setting1", "value1"],
                ["setting2", "value2"]
            ]
        }
    ]
}
```

<a name='initializers'></a>

## initializers

Этот параметр позволяет предоставить одну или несколько функций C для выполнения при определенных событиях жизненного цикла расширения - в частности, `GINIT` (`globals`), `MINIT` (`module`), и `RINIT` (`request`). Для получения более подробной информации обратитесь к главе [Хуки времени выполнения](/0.10/en/lifecycle).

```json
{
    "initializers": [
        {
            "globals": [
                {
                    "include": "my/awesome/library.h",
                    "code": "setup_globals_deps(TSRMLS_C)"
                }
            ],
            "module": [
                {
                    "include": "my/awesome/library.h",
                    "code": "setup_module_deps(TSRMLS_C)"
                }
            ],
            "request": [
                {
                    "include": "my/awesome/library.h",
                    "code": "some_c_function(TSRMLS_C)"
                },
                {
                    "include": "my/awful/library.h",
                    "code": "some_other_c_function(TSRMLS_C)"
                }
            ]
        }
    ]
}
```

<a name='name'></a>

## name

Имя расширения, используемое в скомпилированном Си коде. Может содержать только ASCII-символы:

```json
{
    "name": "test"
}
```

<a name='namespace'></a>

## namespace

Пространство имен расширения. Это должен быть простой идентификатор, соответствующий регулярному выражению `[a-zA-Z0-9\_]+`:

```json
{
    "namespace": "test"
}
```

<a name='optimizations'></a>

## optimizations

Оптимизации компилятора, которые должны быть включены или отключены в текущем проекте:

```json
{
    "optimizations": {
        "static-type-inference": true,
        "static-type-inference-second-pass": true,
        "local-context-pass": false
    }
}
```

<a name='optimizer-dirs'></a>

## optimizer-dirs

Каталоги, где находятся ваши собственные оптимизаторы. Каталог поиска находится относительно корневой папки вашего проекта:

```json
{
    "optimizer-dirs": [
        "optimizers"
    ]
}
```

<a name='package-dependencies'></a>

## package-dependencies

Определение зависимостей библиотеки (ограничения версии будут проверяться с помощью `pkg-config`, и может использовать один из операторов `=`, `>=`, `<=`, или `*`):

```json
{
    "package-dependencies": {
        "openssl": "*",
        "libpng": ">= 0.1.0",
        "protobuf": "<= 2.6.1"
    }
}
```

<a name='prototype-dir'></a>

## prototype-dir

Позволяет вам предоставить файлы прототипов, описывающие другие расширения, необходимые для сборки, которые не обязательно устанавливать на этапе сборки:

```json
{
    "prototype-dir": {
        "igbinary": "prototypes",
        "session": "prototypes"
    }
}
```

<a name='requires'></a>

## requires

Позволяет вам перечислить другие расширения, необходимые для создания/использования вашего собственного:

```json
{
    "requires": {
        "extensions": [
            "igbinary",
            "session"
        ]
    }
}
```

<a name='silent'></a>

## silent

Подавляет почти весь вывод `zephir` команд (аналогично `-w`):

```json
{
    "silent": false
}
```

<a name='stubs'></a>

## stubs

Эта настройка позволяет настроить способ генерации IDE заглушек. `path` указывает, где должны быть созданы заглушки, а `stubs-run-after-generate` указывает, следует ли автоматически (повторно) создавать заглушки, когда ваш код компилируется в C:

```json
{
    "stubs": {
        "path": "ide/%version%/%namespace%/",
        "stubs-run-after-generate": false
    }
}
```

<a name='verbose'></a>

## verbose

Выводит более подробную информацию в сообщениях об ошибках из сгенерированых исключений используя `zephir` команды (также можно включить с помощью `-v`, или отключить с помощью `-V`):

```json
{
    "verbose": false
}
```

<a name='version'></a>

## version

Версия расширения. Должна соответствовать регулярному выражению `[0-9]+\.[0-9]+\.[0-9]+`:

```json
{
    "version": "1.2.0"
}
```

<a name='warnings'></a>

## warnings

Предупреждения компилятора, которые должны быть включены или отключены в текущем проекте:

```json
{
    "warnings": {
        "unused-variable": true,
        "unused-variable-external": false,
        "possible-wrong-parameter": true,
        "possible-wrong-parameter-undefined": false,
        "nonexistent-function": true,
        "nonexistent-class": true
    }
}
```