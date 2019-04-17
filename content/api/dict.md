---
title: dict()
---

IXM adds a new previously unavailable data type to CMake, the `dict()` command
set. A `dict()` is a simple associative array, much like those typically found
in other programming and scripting languages (though noticably absent from
CMake). Much of `dict()`'s interface was made to mimic that of `list()`, though
some operations must be different, simply due to the nature of the data type.

{{<danger>}}
Due to limitations with CMake, `dict()` instances have what is known in CMake
as *directory scope*. This means that the directory the `dict()` is defined in
and any directories added via [add_subdirectory][1] will be available in all
scopes.

[1]: https://cmake.org/cmake/help/latest/command/add_subdirectory.html
{{</danger>}}

## Interface

[Reading](#reading)
: dict([KEYS](#dict-keys) <var>dict</var> <var>out-var</var>)
: dict([GET](#dict-get) <var>dict</var> <var>key</var> <var>out-var</var>)

[Serialization](#serialization)
: dict([JSON](#dict-json) <var>dict</var> [INTO](#dict-json-into) <var>filename</var>)
: dict([SAVE](#dict-save) <var>dict</var> [INTO](#dict-save-into) <var>filename</var>)
: dict([LOAD](#dict-load) <var>dict</var> [FROM](#dict-from-into) <var>filename</var>)

[Modification](#modification)
: dict([INSERT](#dict-insert) <var>dict</var> <var>key</var> [[STRING](#)|[APPEND](#)|[ASSIGN](#)] <var>value</var>...)
: dict([TRANSFORM](#dict-transform) <var>dict</var> <var>ACTION</var> [<var>SELECTOR</var>] [OUTPUT_VARIABLE <var>output-variable</var>])
: dict([MERGE](#dict-merge) <var>dict</var> [[STRING](#dict-merge-string)|[APPEND](#)|[ASSIGN](#)] {{<arg other-dict>}} [{{<arg otherdict>}}...])
: dict([REMOVE](#dict-remove) <var>dict</var> <var>key</var> [<var>key</var>...])
: dict([CLEAR](#dict-clear) <var>dict</var>)

## Reading

{{<command "dict(KEYS)">}}
The dictionary type keeps track of all keys stored within a `dict()`. At times,
it is necessary to iterate over all the keys and this allows us to get them as
a list whenever possible.
{{</command>}}

{{<command "dict(GET)">}}
It would not make much sense to have a data type that stores values if we could
not get those values back. However unlike [list()][], which permits extracting
several indices from a value, `dict(GET)` des not. Any additional extraction of
the value stored into <var>out-var</var> should be run through the [list(GET)][]
command.

[list(GET)]: https://cmake.org/cmake/help/latest/command/list.html#get
[list()]: https://cmake.org/cmake/help/latest/command/list.html
{{</command>}}

## Serialization

Unlike other data types in CMake, the `dict()` type has a special format for
serializing to disk. The details of this format is discussed elsewhere. In
addition to serializing to this custom format, it can also serialize to JSON.
However, because of limitations with CMake, we cannot then *quickly* and
*effeciently* deserialize JSON to a `dict()`. The `dict()` type keeps support
for its custom format to help caching additional information.

{{<command "dict(JSON)">}}
Serializes <var>dict</var> into <var>filename</var> as JSON. If
<var>dict</var> does not exist, an empty JSON object will be written out.

### Signature

```cmake
dict(JSON <dict> INTO <filename>)
```

{{<param "INTO">}}
The filename to serialize the <var>dict</var> into. If the given filename is
not of the form `filename.json`, it will have the `.json` extension
automatically appended. If <var>filename</var> is a relative path, it will be
placed into `CMAKE_CURRENT_BINARY_DIR/filename.json`. If `filename` already
exists, its contents will be overwritten.
{{</param>}}
{{</command>}}

{{<command "dict(SAVE)">}}
Serializes <var>dict</var> into <var>filename</var>. If <var>dict</var> does
not exist, an empty file will be written out

### Signature

```cmake
dict(SAVE <dict> INTO <filename>)
```

{{<param "INTO">}}
The filename to serialize the <var>dict</var> to. If the given filename is
not of the form `{filename}.ixm`, it will have the `.ixm` extension
automatically appended. If the filename is a relative path, it will be placed
into `CMAKE_CURRENT_BINARY_DIR`. If <var>filename</var> already exists, its
data will be overwritten.
{{</param>}}
{{</command>}}

{{<command "dict(LOAD)">}}
Deserializes {{<arg filename>}} into <var>dict</var>. If <var>dict</var> does
not exist, it will be created.

### Signature

```cmake
dict(LOAD <dict> FROM <filename>)
```

{{<param "FROM">}}
The filename to deserialize the <var>dict</var> from. If the given filename is not of
the form `<filename>.ixm`, it will have the `.ixm` extension automatically
appended before trying to read from the file. If `filename` is a relative path,
it will be read from `CMAKE_CURRENT_BINARY_DIR`. If `filename` does not exist,
no operation will occur.
{{</param>}}
{{</command>}}

## Modification

{{<command "dict(INSERT)">}}
As its name implies, `dict(INSERT)` will insert values into <var>dict</var>
for the given <var>key</var>. If <var>dict</var> or <var>key</var> do not yet
exist, they will be created.

### Signature

```cmake
dict(INSERT <dict> <key> [STRING|APPEND|ASSIGN] <value>...)
```

{{<param "STRING">}}
All values will be concatenated into a single string before being added to
the <var>key</var>. This is equivalent to the `APPEND_STRING` option from
`set_property`.
{{</param>}}

{{<param "APPEND">}}
All values will be appended to the <var>key</var>. If the <var>key</var> does
not exist, it will have the same behavior as `ASSIGN`.
{{</param>}}

{{<param "ASSIGN">}}
All values are assigned directly to the <var>key</var>. If the <var>key</var>
does not exist, it will be created. If the <var>key</var> already exists, its
contents will be overwritten.
{{</param>}}
{{</command>}}

{{<command "dict(TRANSFORM)">}}
Transforms the values stored in <var>key</var> by using the `list(TRANSFORM)`
subcommand. If <var>dict</var> or <var>key</var> does not exist, no operation
will occur and any variables named by `OUTPUT_VARIABLE` will not be defined.

{{<param "OUTPUT_VARIABLE">}}
After the operation completes, the result will be placed into
<var>output-variable</var>. If this option is not present, the <var>key</var>
will be modified in place.
{{</param>}}

### Signature
```cmake
dict(TRANSFORM <dict> <key> <ACTION> [<SELECTOR>] [OUTPUT_VARIABLE <output variable>])
```
{{</command>}}

{{<command "dict(MERGE)">}}
Takes all the keys and values in <var>other-dict</var>... and merges their
values into <var>dict</var>. If <var>dict</var> does not exist, it will be
created. Each action is more or less equivalent to the actions found in
`dict(INSERT)`.

### Signature

```cmake
dict(MERGE <dict> [STRING|APPEND|ASSIGN] <other-dict> [<other-dict>...])
```

{{<param "STRING">}}
All values for similar keys will be concatenated into a single string when
placed into <var>dict</var>. This is equivalent to the `APPEND_STRING` option
from `set_property`.
{{</param>}}

{{<param "APPEND">}}
All values for similar keys will be appended into a single list before being
set into <var>dict</var>. Duplicates are *not* removed.
{{</param>}}

{{<param "ASSIGN">}}
Each {{<arg other-dict>}} given will assign its keys values in the order they
are passed to `dict(MERGE)`. In other words, if two {{<arg other-dict>}} share
the same key, the second one passed will overwrite the values of the first one.
{{</param>}}

{{</command>}}

{{<command "dict(REMOVE)">}}
Given <var>dict</var>, remove all values stored in <var>key</var>. If neither
<var>dict</var>, nor <var>key</var> exist, this command will do nothing.
{{</command>}}

{{<command "dict(CLEAR)">}}
Removes all keys and values from <var>dict</var>. This is semantically
equivalent to:

```cmake
dict(KEYS <dict> keys)
dict(REMOVE <dict> ${keys})
```
{{</command>}}
