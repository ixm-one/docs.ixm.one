---
title: IXM Specific Properties
---

IXM provides a large number of properties at the `GLOBAL` and `TARGET` scope.
This section covers the IXM specific properties that affect behavior at the
`GLOBAL` scope. Additional properties can be found for various tools, code
generators, and package generation in their specific locations.

## Project Specific Properties

{{<param "IXM_CURRENT_BLUEPRINT_FILE">}}
When set, this property presents the path to the `BLUEPRINT` file that was
discovered in the call to `project()`.
{{</param>}}

{{<param "IXM_CURRENT_BLUEPRINT_NAME">}}
When set, this property represents the value passed to the `BLUEPRINT`
parameter for the call to the `project()` override.
{{</param>}}

{{<param "IXM_CURRENT_BLUEPRINT_FILE">}}
This value is always set, and will be true if the call to `project()` was the
very first one made. i.e., it is the "root" `{CMakeLists.txt}` for a given
build
{{</param>}}
