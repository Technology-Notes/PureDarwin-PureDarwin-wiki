otool
=====
otool is a tool that, among other things, can show which shared libraries an application needs to run (similar to "ldd" on Linux).

<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** When to use otool](otool.html#TOC-When-to-use-otool)
2.  [**2** How to use otool](otool.html#TOC-How-to-use-otool)
3.  [**3** Use chroot](otool.html#TOC-Use-chroot)


### When to use otool
otool is useful when you want to run an application on PureDarwin, but are not sure what libraries it needs.
### How to use otool
Assume we want to know which libraries are needed to run bash.
Simply run:

<span style="font-family:courier new">otool -L /Volumes/PureDarwin/bin/bash</span>

It will give us

<span style="font-family:courier new"></span>
<span style="white-space:pre"> </span>/usr/lib/libncurses.5.4.dylib (compatibility version 5.4.0, current version 5.4.0)
<span style="white-space:pre"> </span>/usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
<span style="white-space:pre"> </span>/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 111.0.0)
<span style="white-space:pre"> </span>/usr/lib/libgcc_s.1.dylib (compatibility version 1.0.0, current version 1.0.0)


So, we need to have these libaries installed in our PureDarwin system in order to be able to run bash.

### Use chroot

In order to further test, you can use chroot. (To be described)
