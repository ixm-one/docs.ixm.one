---
title: Finding System Dependencies
---

IXM provides an alternative to the littany of `find_XXX` commands provided by
CMake. These alternatives were written to simplify writing `find_package`
files. This module does so in a *modern* style, and will automatically import
libraries, programs, and components as desired.

{{<tip>}}
The following commands may *only* be called inside fo a `Find{Package}.cmake`
file. Attempting to do so outside of one will result in a hard error
{{</tip>}}


## Interface

[Targets](#targets)
: **find([FRAMEWORK](#find-framework) \<find-framework> [HEADER \<name>] [COMPONENT \<component>] [HINTS \<hints>...])**
: **find([PROGRAM](#find-program) \<find-program> [FLAGS \<flags>...] [VERSION \<version>] [COMPONENT \<component>] [HINTS \<hints>...])**
: **find([LIBRARY](#find-library) \<find-library> [COMPONENT \<component>] [HINTS \<hints>...])**

[Properties](#properties)
: **find([INCLUDE](#) \<find-include> \<name>... [COMPONENT \<component>] [HINTS \<hints>...])**

## Targets

{{<command "find(FRAMEWORK)">}}
Attempts to find a framework and import it as a library into global scope. By
default, it will attempt to find the header *name/name.h*. If `HEADER` is
passed as a parameter, it will instead look for *name/${HEADER}.h*.

{{<param "HEADER">}}
Name of a header to look for within a framework's directory.
{{</param>}}
{{</command>}}

{{<command "find(PROGRAM)">}}

### Signature

```cmake
find(PROGRAM <name>... [...] [VERSION <version>] [FLAGS <flags>...])
```
TODO
: Need to remove this table and replace it with something better

Parameter | Description
----------|------------
 name     | Name of the program to search for. Multiple names can be passed to the command

{{<param "VERSION">}}
Regex used to determine the version of the program if found. By default, this
is `[^0-9]+([0-9]+)[.]([0-9]+)?[.]?([0-9]+)?[.]?([0-9]+)?`. All regexes pass
will have `.*` appended to them.
{{</param>}}
{{<param "FLAGS">}}
Command line flag(s) to add to the program when trying to determine its version. This is `--version` by default.
{{</param>}}
{{</command>}}

## Properties

{{<command "find(INCLUDE)">}}
Attempts to find a header in the path given. It's path will be added via `target_include_directories` to the `find_package` target, unless <var>component</var> was passed, in which case the target will be <var>component</var>.

### Signature

```cmake
find(INCLUDE <name>... [...])
```

Parameter | Description 
----------|------------
 name     | Full name of include file, as though it were used in `#include <name>`
 component | Optional parameter that will state what *target* the <var>name</var> is for
 hints    | Additional paths for `find()` to search through
{{</command>}}
