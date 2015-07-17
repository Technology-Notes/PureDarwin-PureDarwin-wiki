Booting from CD-ROM
===================
This page describes how to produce a CD-ROM (or DVD, for that matter) which can boot Darwin on both EFI- and BIOS-based systems.

<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:400px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** What we want to do](booting-from-cd-rom.html#TOC-What-we-want-to-do)
2.  [**2** How hybrid images work](booting-from-cd-rom.html#TOC-How-hybrid-images-work)
3.  [**3** How to create a hybrid image using pd_makedmg](booting-from-cd-rom.html#TOC-How-to-create-a-hybrid-image-using-pd_makedmg)
4.  [**4** How pd_makedmg works](booting-from-cd-rom.html#TOC-How-pd_makedmg-works)
5.  [**5** kexts needed for booting from CD-ROM](booting-from-cd-rom.html#TOC-kexts-needed-for-booting-from-CD-ROM)
6.  [**6** To do](booting-from-cd-rom.html#TOC-To-do)


### What we want to do {style="color:rgb(0,0,0);margin-top:10px;margin-right:10px;margin-bottom:10px;margin-left:0px"}
Master a CD from which both EFI- (Intel Mac) and BIOS-based (PC) systems can boot.


[![](../../_/rsrc/1212162663696/developers/booting/booting-from-cd-rom/bothboots.jpg)](booting-from-cd-rom/bothboots.jpg%3Fattredirects=0)

An Intel-based Macintosh will actually be able to boot the volume either through EFI (right-hand side) or through its Compatibility Support Module ("Boot Camp") BIOS emulation (left-hand side, misleadingly labeled "Windows").
### How hybrid images work
The key is to use a HFS+ filesystem for the image (thus, making a ".dmg" rather than a ".iso") and "injecting" parts of an El Torito ISO into it, This is called a "hybrid image". 

### How to create a hybrid image using pd_makedmg {style="color:rgb(0,0,0);margin-top:10px;margin-right:10px;margin-bottom:10px;margin-left:0px"}
<span style="font-size:13px;font-weight:normal">To create a hybrid image, you can use the [pd_makedmg](../../downloads.html) script. It is based on [darwinmaster.sh](http://darwinbuild.macosforge.org/trac/browser/trunk/darwinbuild/darwinmaster.sh), and has been extended to support EFI booting in addition to BIOS booting. Its syntax is as follows:</span>


<span style="font-family:courier new,monospace">pd_makedmg /Volumes/PureDarwinDisk /tmp/puredarwin.dmg PureDarwin</span>
<span style="font-family:courier new">
</span>
<span style="font-family:inherit">This would take the files located at /Volumes/PureDarwinDisk (which should contain a working Darwin installation) and turn them into a disk image located at /tmp/puredarwin.dmg with the name of "PureDarwin", which could be burned to a CD e.g., using Disk Utility.</span>

Note that pd_makedmg currently needs a Mac to run, since it makes use of commands like hdiutil which are not part of Darwin.
### How pd_makedmg works
Here is what it essentially does:
-   Creates Extensions.mkext from the kexts in the source tree
-   Makes a temporary ISO that contains the boot-132 files and that has cdboot as its El Torito boot image 
    (this allows BIOS-based machines to boot)
-   Creates, initializes and mounts a HFS+ disk image (dmg)
-   "Injects" parts of the temporary El Torito ISO into the dmg using
    <span style="font-family:courier new">dd if="$ELTORITOISO" of=$rdev skip=64 seek=64 bs=512
    <span style="font-family:Arial">(Do not actually execute this command. It is ran automatically by the script.)</span></span>
-   Copies the contents of the source tree to the dmg
-   Blesses boot.efi (this allows EFI-based machines to boot)
### kexts needed for booting from CD-ROM

Following the instructions from above should produce a CD that boots at least to the point "Still waiting for root device...". For the kernel to be able to access the CD, the necessary kexts need to be installed of course. These are, in addition to the [minimal set of kexts](../kexts.html):
-   IOATAPIProtocolTransport.kext
-   IOCDStorageFamily.kext
-   IODVDStorageFamily.kext
-   <span>IOBDStorageFamily.kext</span><span style="color:rgb(255,0,0)"> <span style="color:rgb(0,0,0)">(Blu-ray Disc driver; available for Darwin as binary only)</span></span>
### To do {style="color:rgb(0,0,0);margin-top:10px;margin-right:10px;margin-bottom:10px;margin-left:0px"}
-   Remove any tools that are not part of Darwin
-   replace hdiutil create with a simple dd if=/dev/zero and hdid -nomount with losetup ... and it would appear to work on Linux then (dfe)
