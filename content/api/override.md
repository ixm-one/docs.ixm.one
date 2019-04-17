---
title: CMake Command Overrides
---

IXM provides overrides for several builtin CMake commands. All of these
overridden commands are automatically imported by IXM. Special care has been
taken to make sure each override still acts correctly when used with existing
function signatures.

{{<warning>}}
*Do not* override IXM's command overrides.  They will break as CMake only
permits calling the *previously defined* command, and it does not support
chaining these override operations.
{{</warning>}}

{{<command "project()">}}

In addition to the default `project()` calls provided by CMake, an additional
optional argument is permitted for both existing signature. If `BLUEPRINT` is
passed to the signature, IXM will attempt to load and set the *project
blueprint*. If the blueprint cannot be found, CMake will error and the project
configuration and generation steps will be incomplete.

In addition to setting the `IXM_CURRENT_BLUEPRINT_FILE` and
`IXM_CURRENT_BLUEPRINT_NAME` properties, it also sets the
`<PROJECT-NAME>_SUBPROJECT_DIR` and `PROJECT_SUBPROJECT_DIR` variables. These
are then available for use in the `BLUEPRINT` script. Additionally,
`<PROJECT-NAME>_STANDALONE` property is set, as well as the
`<PROJECT-NAME>_STANDALONE` and `PROJECT_STANDALONE` variables.  Lastly, if
`CMAKE_BUILD_TYPE` is not set, it will be set to ``Debug``.  This should reduce
the number of issues when users forget to manually set the ``CMAKE_BUILD_TYPE``
value when first configuring a project.

{{<param "BLUEPRINT">}}
This must be a name for a blueprint. IXM uses a predetermined lookup path,
which allows users to override or add their own blueprints for projects.  The
current lookup path is as follows

<pre>
${root-path}/Blueprints/&lt;blueprint>.cmake
${root-path}/Blueprints/&lt;blueprint>.cmake
${root-path}/&lt;blueprint>/blueprint.cmake
${root-path}/&lt;blueprint>.cmake
</pre>

Where `${root-path}` is an entry found in one of `IXM_PROJECT_ROOT_PATH`,
`CMAKE_MODULE_PATH`, and `IXM_ROOT`.  By default, `IXM_PROJECT_ROOT_PATH` is
not defined, and thus will be skipped unless set by a user *prior* to a call to
`project`
{{</param>}}

### Signature

```cmake
project(<PROJECT-NAME> [BLUEPRINT <blueprint>] ...)
```

{{</command>}}

{{<command "add_executable(<name>)">}}

This version of `add_executable()` provides the ability to declare an
executable as a `GUI`, `CONSOLE`, or `SERVICE`.

### Signature
```cmake
add_executable(<name> [GUI|CONSOLE|SERVICE] [EXCLUDE_FROM_ALL] [source...])
```

{{<param "CONSOLE">}}
Setting this option has no effect on any platform except Linux.  This allows
users to still get the above benefits of generating a `{OUTPUT_NAME}.appimage`
despite creating a console application.
{{</param>}}

{{<param "SERVICE">}}

This option means that your executable is intended to be treated
as a daemon or background service. On Linux, this means IXM will
generate a :file:`{SERVICE_NAME}.service` file for use with
*systemd*, and a :file:`{SERVICE_NAME}.plist` for macOS. Windows
support is still in development.

{{<note>}}
This feature is currently not implemented in the alpha release of IXM.
{{</note>}}
{{</param>}}

{{<param "GUI">}}
Declaring an executable with this does not actually automatically handle
anything regarding generation of code for frameworks such as Qt. However, it is
a superset of calling `add_executable()` with the `MACOSX_BUNDLE` and
`WIN32_EXECUTABLE` properties set on the resulting target. However, several
properties regarding `APPIMAGE` *will* be set on the target, and for some
blueprints, a `{OUTPUT_NAME}.appimage` file will attempt to be generated when
the ``package`` target is built. Additionally, a `{OUTPUT_NAME}.desktop` file
will be generated.
{{</param>}}
{{</command>}}

{{<command "define_property">}}

### Signature

```cmake
define_property(<property-name> SCOPE <scope> [PRIVATE] [HELP <help>]
```

This is a superset of the default `define_property()` call. Unlike the default
call, this one is vastly reduced in the number of parameters it takes. Due to
the `BRIEF_DOCS` and `FULL_DOCS` typically not being set (as there is no way to
easily generate information on them if they are defined by users), we simply
specify a single *optional* `HELP` parameter. Additionally, this command will
generate *both* a property named `<{property-name}>`, as well as a property
named `INTERFACE_<{property-name}>`.

{{<param "PRIVATE">}}
When this boolean flag is given, the property will not have the ``INHERITED``
flag attached to it.
{{</param>}}

{{<param "SCOPE">}}
This is the scope that the property will be defined at. This can be any of the
scopes one might typically pass to `define_property()`.  *Typically* IXM's
properties will be defined at the `TARGET` scope.
{{</param>}}

{{<param "HELP">}}
A string that will be passed to underlying `BRIEF_DOCS` and `FULL_DOCS`
parameters in the default `define_property()`.  If this flag is not passed, a
string of `"<none>"` will be used.
{{</param>}}

{{</command>}}
