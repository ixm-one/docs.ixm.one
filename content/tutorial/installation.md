---
title: Installation
weight: 2
---

IXM is currently intended to be installed via the [FetchContent][] CMake module.
what this means is that you'll *always* be able to make sure you have the same
IXM installation as everyone else. Technically you can pin a revision or use a
custom fork of IXM, however this is neither recommended, nor encouraged.

## Bootstrapping CMake

{{<note>}}
If you already have CMake installed, feel free to skip this section
{{</note>}}

While not all platforms supply a modern version of CMake, they do typically
provide a recent version of Python 3.5 or later, and the Python package
manager `pip`. If this is available on your system, you can quickly install a
recent version of CMake and Ninja via `pip` by running the following command:

```bash
<pip-command> install --upgrade cmake ninja
```

This will install the minimum set of modern CMake tools needed for you to use
IXM. After this, just make sure these versions of `cmake` and `ninja` are on
your system `PATH` environment variable.

TIP: Not all platforms have their python3 installation come with `pip` by
default. Additionally, its name might have the version attached to it (e.g.,
`pip3.6`).

### Debian

As of 3.14, KitWare has been kind enough to provide an APT repository.
Directions on installing CMake in this way can be found [here][]. However,
these instructions are a bit out of date for modern tooling.

Instead, perform the following operations (Replace `bionic` with your Ubuntu
distribution of choice):

```bash
$ curl -L https://apt.kitware.com/keys/kitware-archive-latest.asc | \
sudo apt-key add -
$ echo 'deb https://apt.kitware.com/ubuntu/ bionic main' | \
sudo tee -a /etc/apt/sources.list.d/cmake.list
$ sudo apt update
$ sudo apt install cmake
```

### RHEL

RHEL 7 based systems do not come with a recent enough version of python by
default. (This has *not* been fixed in RHEL 8). However, thanks to the SCL
collection, one *can* install Python 3.6. To use IXM on these platforms,
perform the following operations:

```bash
yum install rh-python
scl enable rh-python36 bash
python3 -m pip install --upgrade setuptools pip cmake ninja
```

If this does not work, the `.bin` installation script provided by Kitware is
binary safe and *will* work just fine on RHEL based systems.

## Acquiring IXM

To *install* IXM for a project, simply place the following at the top of your
root `CMakeLists.txt`:

```cmake
cmake_minimum_required(VERSION 3.14)
include(FetchContent)
FetchContent_Declare(ixm URL https://get.ixm.one)
FetchContent_MakeAvailable(ixm)
```

[FetchContent]: https://cmake.org/cmake/help/v3.14/module/FetchContent.html
[here]: https://apt.kitware.com/