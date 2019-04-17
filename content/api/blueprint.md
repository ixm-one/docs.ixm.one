---
title: Project Blueprints
---

IXM provides support for setting what are known as *project blueprints*. This
means that by simply placing directories and files in specific places, a
blueprint implementation file will automatically generate all possible targets
for a project before anything else is declared. This allows users to reduce the
amount of CMake code they have to write, while also permitting them to focus on
more difficult problems such as file generation, environment checking, or
dependency fetching.

Currently, IXM only provides two project blueprints:

 * Coven
 * Brujeria

The latter of which is intended for use with the [brujeria][1] python library,
though users can still use it with *some* effort.

{{<note>}}
More blueprints are planned for the 1.0 release, such as Node, Qt, and others.
{{</note>}}

## Coven

The Coven project blueprint is a forward compatible project layout for users
who eventually wish to move to the coven build system. While this build system
is still in development, and is not currently available at this time, users of
this layout will be able to move to it with little to no work added.

### File Locations

<pre>
  ▾ include/
  ▾ src/
    main.cxx
    ▾ bin/
      *.cxx
    ▾ */
      main.cxx
  ▾ examples/
    *.cxx
    ▾ */
      main.cxx
  ▾ tests/
    *.cxx
    ▾ */
      main.cxx
  ▾ benches/
    *.cxx
    ▾ */
      main.cxx
</pre>

### Targets

### Testing

### Benchmarks

### Examples

### Packaging

### Features

## Brujeria


[1]: #
