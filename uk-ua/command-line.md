---
layout: default
language: 'uk-ua'
version: '0.12'
---

# The Zephir Command Line

Once Zephir is installed, you'll use the `zephir` command to manage the Zephir compiler for your projects. This chapter, and the ones following, are about the command itself, how to use it, and how to understand the things it outputs.

As of Zephir 0.11.7, the compiler makes use of `stderr` for displaying error messages. This means you can handle error outputs separately from normal ones, like so:

```bash
zephir generate 2> errors.log 1> /dev/null
```

<a id="zephir-api"></a>

## `zephir api`

Generates an HTML API based on the classes exposed in the extension

-   `--backend=BACKEND`:                 Backend used to generate HTML API (default: `ZendEngine3`)
-   `--path=PATH` (or `-p PATH`):        The API theme to be used
-   `--output=OUTPUT` (or `-o OUTPUT`):  Output directory to generate theme
-   `--options=OPTIONS`:                 Theme options
-   `--url=URL`:                         The base URL to be used when generating links

<a id="zephir-build"></a>

## `zephir build`

This is a meta command that just calls the [`generate`](#zephir-generate), [`compile`](#zephir-compile), and [`install`](#zephir-install) commands. Check those commands for more information on supported options and behaviors for each.

<a id="zephir-clean"></a>

## `zephir clean`

Cleans any object files created by the extension

<a id="zephir-compile"></a>

## `zephir compile`

Compile a Zephir extension

-   `--backend=BACKEND`:                 Backend used to build extension (default: `ZendEngine3`)
-   `--dev`:                             Build the extension in development mode
-   `--no-dev`:                          Build the extension in production mode

Using `--dev` option will force building and installing the extension in development mode (debug symbols and no optimizations). An extension compiled with debugging symbols means you can run a program or library through a debugger and the debugger's output will be user friendlier. These debugging symbols also enlarge the program or library significantly.

NOTE: Zephir development mode will be enabled silently if your PHP binary was compiled in a debug configuration.

In some cases, we would like to get production ready extension even if the PHP binary was compiled in a debug configuration. Use `--no-dev` option to achieve this behavior.

Additionally, any of the options available [under `extra` in the configuration file](/{{ page.version }}/{{ page.language }}/config#extra) can also be passed as options, here, such as `--export-classes` and `--indent=tabs`.

<a id="zephir-fullclean"></a>

## `zephir fullclean`

Cleans any object files created by the extension (including files generated by phpize)

<a id="zephir-generate"></a>

## `zephir generate`

Generates C code from the Zephir code

-   `--backend=BACKEND`:                 Backend used to build extension (default: `ZendEngine3`)

<a id="zephir-help"></a>

## `zephir help`

Displays help for a command

<a id="zephir-init"></a>

## `zephir init`

Initializes a Zephir extension `zephir init <namespace>`

-   `namespace`:                         The extension namespace
-   `--backend=BACKEND`:                 Backend used to create extension (default: `ZendEngine3`)

<a id="zephir-install"></a>

## `zephir install`

Installs the extension in the extension directory (may require root password)

-   `--dev`:                             Install the extension in development mode
-   `--no-dev`:                          Install the extension in production mode

<a id="zephir-list"></a>

## `zephir list`

Lists commands

<a id="zephir-stubs"></a>

## `zephir stubs`

Generates stubs that can be used in a PHP IDE

-   `--backend=BACKEND`:                 Backend used to generate stubs (default: `ZendEngine3`)
