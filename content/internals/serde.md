---
title: Serialization Format
---

IXM's `dict()` serialization format is one that might not typically be seen in
the wild outside of this library. Regardless, we document this file format so
that other tools may rely on it to acquire information from the configure step
during the build step.

## Serialization Trick

IXM utilizes a small "trick" to make working with is `dict()` serialization
faster under CMake. The best way to *store* a dictionary in CMake is to treat
it as a list of lists. However, escaping semicolons in said lists of lists is
error prone and troublesome. What we can do instead is rely on the classic ASCII/Unicode control codes for File Separator, Group Separator, Record Separator, and Unit Separator.

This approach *could* allow us to dump *several* `dict()` instances into the
same file. Currently, in the alpha version of IXM, we do not (and hence is the
reason `dict(SAVE)` takes only *one* dictionary at the moment.

## File Layout

Currently, the file layout takes the following steps:

 * Get all keys stored in the `dict()`
 * Replace separator in value for each key with the unit separator control code
 * Prepend the key and the record separator control code to the value.
 * Join each key-value string with the group separator.

As one might notice, we don't currently use the file separator control code.
This is because it *might* be needed in the future, but we currently do not.
If it is ever used in the future, it will be done to separate `dict()`
instances in a single file.

An example of how the file might look follows:

```
OPTIONS␞BUILD_TESTING␟OFF␟BUILD_EXAMPLES␟ON␝key␞value
```

Some additional fields and metadata might be inserted before the 1.0 release.
This will result in a different layout of the file, as well as permitting some
metadata. The actual final layout is still to be determined.
