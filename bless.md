bless
=====
bless is the command used on Mac OS X to make volumes bootable.
This page discusses the bless command and its applicability in a PureDarwin system.
It is a work in progress; please contribute.

<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** What is bless](bless.html#TOC-What-is-bless)
2.  [**2** "Worlds" supported by bless](bless.html#TOC-Worlds-supported-by-bless)
    1.  [**2.1** efiboot](bless.html#TOC-efiboot)
    2.  [**2.2** boot](bless.html#TOC-boot)
    3.  [**2.3** BootX](bless.html#TOC-BootX)
3.  [**3** Using bless](bless.html#TOC-Using-bless)
4.  [**4** bless alternatives](bless.html#TOC-bless-alternatives)
5.  [**5** References](bless.html#TOC-References)


### What is bless
bless is a command line tool on Mac OS X and Darwin to make volumes bootable.
It also includes libbless, a library which is used by other applications (such as installers) to make volumes bootable.
The bless project is available as Open Source from Apple.
### "Worlds" supported by bless
#### <span style="font-size:18px"><span style="font-size:14px">efiboot</span></span>
The bless command is used to set up the "efiboot" boot loader on Intel-based Macintosh systems which use EFI as their firmware.
(to be written)
#### boot
The bless command was used to set up the "boot" boot loader on generic BIOS-based systems in older versions of Darwin.

bless-24 was known to work with BIOS-based machines.
It appears that bless-37.1.4 from 2006 was the latest version that contained BIOS code.
Later versions of bless are no longer capable of blessing a volume so that it works on BIOS-based machines.
<span style="border-collapse:separate;font-family:Times;font-size:16px"></span>
``` {style="word-wrap:break-word;white-space:pre-wrap"}
 *  Revision 1.21  2006/01/02 22:27:28  ssen
 *  <rdar://problem/4395370> bless should not support BIOS systems
 *  For RC_RELEASE=Leopard, keep BIOS support, but preprocess it out
 *  for Herbie and the open source build
```
(Maybe the BIOS-related portions of the code should be patched back in by PureDarwin.)

(to be written)
#### BootX
Was used to set up the "BootX" boot loader on "New World" and "Old World" PowerPC computers which used OpenFirmware as their firmware. Not relevant for PureDarwin.
### Using bless
The usage of the bless command varies depending on which "world" you are targeting. Please refer to the bless manpage that comes with the bless command.
### bless alternatives
A method to make a disk bootable on BIOS and EFI machines without using the bless command is described on the [boot](boot.html) page.
### References
-   http://src.gnu-darwin.org/DarwinSourceArchive/expanded/bless/bless-37.1.4/libbless/BIOS/
