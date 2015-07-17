XNU, the kernel
===============
**Please note: This page has not been updated to take changes made in Darwin 10, or later into account.**
This page is about the XNU kernel.
Both Apple vanilla mach_kernel and Voodoo [xnu-dev](http://code.google.com/p/xnu-dev/) are available and functional in PureDarwin.
It is a work in progress. Please contribute.

<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** What is XNU](xnu.1.html#TOC-What-is-XNU)
2.  [**2** Common myths about XNU](xnu.1.html#TOC-Common-myths-about-XNU)
3.  [**3** Common parts of XNU](xnu.1.html#TOC-Common-parts-of-XNU)
4.  [**4** Creating a prelinked mach_kernel](xnu.1.html#TOC-Creating-a-prelinked-mach_kernel)
5.  [**5** Ressources](xnu.1.html#TOC-Ressources)

### What is XNU
XNU is the kernel used by Darwin and Mac OS X (similar to what GNU/Linux kernel is in a Linux distro).
XNU acronym stands for "X is Not Unix" (although Mac OS X has been Unix certified in 2007).
### Common myths about XNU
(please add some text)

-   <span style="font-style:italic">Not</span> a microkernel (although using parts of the Mach microkernel)
-   <span style="font-style:italic">Not</span> 64-bit (although able to run 64-bit binaries)
-   <span style="font-style:italic">Not</span> so hard to recompile
See [here](http://developer.apple.com/documentation/Darwin/Conceptual/KernelProgramming/Architecture/chapter_3_section_3.html) why the term kernel is used somewhat differently than you might expect.
### Common parts of XNU
-   Mach 3.0
-   BSD
-   I/O Kit
### Creating a prelinked mach_kernel
XNU cannot boot by itself, since it always needs a [certain set](../downloads/puredarwin-nano.html) of kernel extensions (kexts) to be present. However, you can create a mach_kernel file that not only contains XNU itself, but also a defined set of kexts:
    kextcache -a i386 -K  /Volumes/PureDarwin/mach_kernel 
    -c /tmp/mach_kernel.prelinked /Volumes/PureDarwin/System/Library/Extensions
The result is that you should have a file in /tmp/mach_kernel_prelinked that contains both XNU and the kexts.


Another example, fast boot and kext autoloaded:


<span style="font-family:courier new,monospace"><span style="font-size:small">kextcache -a i386 -s -l -n -c "$MOUNT/System/Library/Caches/com.apple.kernelcaches/kernelcache.DEADBEEF" -k -K "$MOUNT/mach_kernel" -m "$MOUNT/System/Library/Extensions.mkext" "$MOUNT/System/Library/Extensions"</span></span>
### Ressources
<http://en.wikipedia.org/wiki/XNU> 
<http://osxbook.com/book/bonus/ancient/whatismacosx/arch_xnu.html> 
<http://code.google.com/p/xnu-dev/>

