---
title: Tips
---

This section is meant to cover some tips to help improve your development
experience when working with IXM.

## Faster Rebuilds

`ccache` and `sccache` are extremely useful tools, (the former especially when
used in conjunction with `distcc`) and using them will give you benefits when working with IXM.

To use them in a non-blueprint based system, simply do the following:

```cmake
find_package(CCache) # Or SCCache)
if (TARGET CCache::CCache)
  get_property(CMAKE_CXX_COMPILER_LAUNCHER
    TARGET CCache::CCache
    PROPERTY IMPORTED_LOCATION)
  get_property(CMAKE_C_COMPILER_LAUNCHER
    TARGET CCache::CCache
    PROPERTY IMPORTED_LOCATION)
endif()
```

`sccache` is a drop in replacement for `ccache` and will also work with the
Visual C++ Compiler without issue. Additionally, with some work, it supports
toolchains, additional storage systems, and a more secure approach than what is
offered with `icecc`.

TIP: The [Coven](/blueprints/coven) blueprint will automatically try to use
`sccache` or `ccache` if possible.

## Safer Unity Builds

One of the primary issues with Unity Builds is making sure that symbols don't
collide. One of the wways to avoid this is to use an *inline* namespace within
a *nameless namespace* based on the unique name for the file. This will prevent
linker errors, however it *does not* prevent lookup issues or ambiguities. Some
compilers will give useful diagnostic information in these cases, allowing you
to select the correct namespace explicitly while ensuring each symbol retains
its internal linkage.

```cpp
namespace { inline namespace something { void test(); }}
namespace { inline namespace nothing { void test(); }}
```

WARNING: Do not use nameless namespaces inside of headers. It will cause issues
if included into other files.
