---
title: Adding Custom File Extension Support
---

IXM supports injecting support for additional file formats when added to a
target during a call to [target_sources][1]. This allows additional languages
to possibly generate additional files *before* they are added to the target.
One example of this would be protobuf or cython. The way this is supported is
by requiring a user to define a command name `target_sources_<ext>`. Thus,
adding support for protobuf files (which have extension `.proto`) requires that
a command named `target_sources_proto` exist.

## Generating Additional Source

As an example, we'll show how IXM supports Cython for generating sources.
Cython is a python-like language that can ingest a cython source file, and then
generate python bindings for the source as necessary. IXM supports it as long
as Python and Cython are available for use.

To add support for the sources, we do the following:

```cmake
function (target_sources_pyx target visibility)
  set(interface PUBLIC;INTERFACE)
  set(private PRIVATE;PUBLIC)

  if (visibility IN_LIST interface)
    set_property(TARGET ${target} APPEND
      PROPERTY INTERFACE_CYTHON_SOURCES "${ARGN}")
  endif()
  if (visibility IN_LIST private)
    set_property(TARGET ${target} APPEND PROPERTY CYTHON_SOURCES "${ARGN}")
  endif()
endfunction ()
```

You'll notice that this does not actually generate the Cython files themselves.
To do this, we simply have to setup an `add_custom_command` call. We can do
this explicitly or have it be called by another command. Regardless, the actual
setup is as simple as doing:

```cmake
# ${name} is the name of the target we were working with
set(output-file ${CMAKE_CURRENT_BINARY_DIR}/cython/${name}.cxx)
add_custom_command(
  COMMAND Python::Cython
    $<$<CONFIG:Debug>:--gdb>
    -${Python_VERSION_MAJOR}
    --output-file ${output-file}
    --cplus
    $<TARGET_PROPERTY:${name},CYTHON_SOURCES>
  DEPENDS $<TARGET_PROPERTY:${name},CYTHON_SOURCES>
  COMMENT "Generating ${output-file} for '${name}'"
  OUTPUT "${output-file}"
  COMMAND_EXPAND_LISTS
  VERBATIM)
target_sources(${name} PRIVATE ${output-file})
```

As you can see, the actual generation of the code is quite simple.
Additionally, we stick to the *proper* modern CMake approach by relying on
targets *and* their properties for keeping track of state.

[1]: #
