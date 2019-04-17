---
title: fetch()
---
# fetch()

  
The `fetch()` command is used as an advanced wrapper around the`FetchContent` module included with CMake. `fetch()` takes a special _dependency variable reference_, which is in the form of`<PROVIDER>{<name|recipe>,<options:...>}`. The exact syntax of both `recipe` and `options` is different based on the given `PROVIDER`. In some cases, a recipe cannot be used, and only a name may be given

The currently available  `fetch()`  providers are:

 -   `HUB`  (GitHub)
 -   `LAB`  (GitLab)
 -   `BIT`  (BitBucket)
 -   `BIN`  (BinTray) – not yet implemented
 -   `URL`  (Download Archive)
 -   `ADD`  (Add from  _subproject_  directory)
 -   `USE` (cmake -P \<script>)
 -   `GIT` (git clone)
 -   `SVN`  (Subversion)
 -   `CVS`  (CVS)
 -   `HG` (Mercurial)

