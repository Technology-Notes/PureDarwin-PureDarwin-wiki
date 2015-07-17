PureDarwin Xmas
===============


![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/PD-Xmas.jpg)

The PureDarwin project announces the immediate availability of "PureDarwin Xmas", a developer preview of the upcoming operating system based on Apple's Darwin 9 sources and other Open Source projects such as X11. At the same time, the PureDarwin project would like to invite the community to discuss, participate and contribute. The developer preview is available for download as a pre-configured virtual machine for VMware Fusion 2.0 on Macintosh, and the code used to generate it is available in a Subversion repository.

[Darwin](http://en.wikipedia.org/wiki/Darwin_%28operating_system%29) is the Open Source operating system from Apple that forms the basis for Mac OS X, and PureDarwin is a community project to make Darwin more usable (some people think of it as the informal successor to OpenDarwin). One current goal of this project is to provide a bootable ISO of Darwin 9.x (the version that corresponds to 10.5.x Leopard). Another goal of this project is to provide additional documentation. [More](wiki/About)...

Contents
1.  [**1** Download](xmas.html#TOC-Download)
2.  [**2** Showcase](xmas.html#TOC-Showcase)
3.  [**3** Known issues](xmas.html#TOC-Known-issues)
4.  [**4** Frequently asked questions](xmas.html#TOC-Frequently-asked-questions)
5.  [**5** Screenshots](xmas.html#TOC-Screenshots)

### Download
-   [XZ-compressed TAR archive](http://code.google.com/p/puredarwin/downloads/detail?name=puredarwinxmas.tar.xz) (hosted on Google Code) - SHA-1 checksum:  <span style="font-family:arial,sans-serif;white-space:nowrap">80f88f44cc540ea05aa847bb18b94bdd133b6c07</span>
-   [BZip2-compressed TAR archive](http://xref.puredarwin.org/puredarwinxmas.tar.bz2) (hosted on XRef) - MD5 checksum: fd0ade4da224475e5dc33c2c11d9d0bc
[VMware](wiki/VMware) Fusion 2 virtual machine or [VirtualBox](wiki/VirtualBox)

### Showcase 

This is expected to work.
with VMware
-   Darwin 9 boots in a VMware Fusion 2 virtual machine on a Macintosh
-   DTrace 
-   X11
-   ZFS
with VirtualBox
-   see [VirtualBox](wiki/VirtualBox)
A [screencast](http://video.google.com/videoplay?docid=2258011422088941976) demonstrating some of Xmas's functionality is available on Google Video.

### Known issues

These issues will be addressed in future releases.
-   Works with VMware Fusion 2 on Macintosh, VirtualBox on Linux
-   login does not work, user is working as root
-   Lots of error messages during the boot process
-   X11 does not work with mach_kernel.voodoo
-   WindowMaker will not be the default WM
-   halt doesn't work reliably, "shutdown -h now" doesn't work reliably
-   /var/log gets filled quickly
-   No network
-   Due to a [restriction](http://communities.vmware.com/thread/183426) of VMware Fusion 2.0.1, you cannot run PureDarwin Xmas on 32-bit CPUs such as the Core Duo in the first-generation Intel Macs (Thanks gireesh). However, you can downgrade to VMware Fusion 2.0.0 which seems to run Darwin fine on a 32-bit CPU (thanks Stuart).
Want to help us on these? Check out the code from svn or hg repository and start hacking...

### Frequently asked questions

-   <span style="font-weight:bold">What is PureDarwin Xmas?
     </span>This is a developer preview to get people interested in PureDarwin and to attract developers.
     It is intended to run on VMware Fusion 2 on Macintosh.
     We call this developer preview "Xmas" because it contains X11, and after all it's Christmas time.
-   <span style="font-weight:bold">Is this close to what the final PureDarwin will be?
     </span>Not at all. It is just a proof-of-concept to show that Darwin 9 is alive.
-   <span style="font-weight:bold">Is this an Apple product?
     </span>No. Darwin is an upstream project from Apple, but the PureDarwin project is not affiliated with Apple. We pay attention to follow any Apple and third-party licenses closely, though.
-   <span style="font-weight:bold">Which license is this under?
     </span>This is a distribution that packages various individual parts. What the PureDarwin project has done from scratch is generally licensed under the new BSD license. However, most parts come from upstream (Apple and third-party) projects and are licensed under their respective licenses, such as the APSL, the Apple Driver License, the GPL, and others.
-   <span style="font-weight:bold">Where are the sources?</span>
     Sources for Darwin can be found at [http://opensource.apple.com](http://opensource.apple.com/) and <http://darwinbuild.macosforge.org/>, sources for third-party software can be found at <http://www.macports.org/> , and sources from the PureDarwin project can be found at <http://code.google.com/p/puredarwin/source/checkout> 
-   <span style="font-weight:bold">How do I contact the PureDarwin project?
     </span>We are on #puredarwin on irc.freenode.net most of the time, and this is our preferred communications channel. We also read the Darwin mailing lists, especially darwin-dev and darwinbuild-dev.

### Screenshots

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/puredarwin_Xmas_red_X.png)

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/puredarwin%20Xmas%20green%20dtrace.png)

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/puredarwin_xmas_zfs_blue.png)

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/puredarxin_xmas_UNIX_yellow.png)
