---
title: Overview
---

IXM is a CMake library that abstracts away a majority of the common operations
CMake requires all users to perform as a project grows. Each of the modules
found in IXM abstracts away the common complexity of the following actions that
nearly *every* CMake based project might need to perform:

 * Find System Packages
 * Acquiring Remote or Local dependencies
 * Automatic target generation based on directory layout
 * C++ focused build environment checks
 * Unity Build and PCH support

And much much more. Read [Getting Started][] for
information on how to install IXM as a dependency in your project.

Additionally, check out the [API][] for detailed
information on the commands provided by IXM, as well as its customization and
configuration hooks.

Lastly, for those that would like to know more about the dark magic found within
IXM, some of the internals are discussed in details [here][].

To see what is current blocking the 1.0 release of IXM, see the [Roadmap][]


[Getting Started]: {{<relref "tutorial">}}
[API]: {{<relref "api">}}
[here]: {{<relref "internals">}}
[Roadmap]: https://github.com/ixm-one/ixm/projects/1