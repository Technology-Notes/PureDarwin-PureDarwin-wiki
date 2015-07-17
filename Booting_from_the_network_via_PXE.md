Booting from the network via PXE
================================

There are now at least 2 known working ways to get forked versions of boot-132 running via PXE. This page describes the two approaches and provides scripts for you to try it out yourself.

Note that right now this page ends where boot-132 is loaded; the rest can be read on [DFE's blog](http://tgwbd.org/blog/2009/10/17/be-free-little-pxes-be-free/).

### Booting PureDarwin with a PXE-enabled boot-132

Apparently DFE has written the missing Darwin/x86 first-stage network boot program (NBP), a first-stage loader for boot-132.

Basically, booting PureDarwin over the network involves the following steps:

1.  Client network card firmware asks DHCP server
2.  DHCP server asks dnsmasq on the server
3.  dnsmasq sends TFTP information
4.  Client network card firmware downloads boot1pxe.0 file specified by dnsmasq from dnsmasq
5.  boot1pxe.0 loads boot (boot-132) (known to work up to this point)
6.  boot-132 loads mach_kernel and Extensions.mkext from the server (still to be implemented)
7.  mach_kernel loads network kext and uses it to load the root volume from the network (still to be implemented)
8.  Root volume image detects that it is booted via the network and sets up ramdisks etc. as needed (like on Mac OS X network boot)
You can try this variant by:
1.  Run Ubuntu 9.10 Live CD (we are using this as the server for now; later we could use PureDarwin as the server)
2.  `wget "http://sites.google.com/a/puredarwin.org/puredarwin/developers/booting/boot/pxe/boot-boot1pxe.bash?attredirects=0&d=1" -O ./ boot-boot1pxe.bash ; sudo bash ./ boot-boot1pxe.bash`

### Booting PureDarwin via Chameleon via mboot.c32 via pxelinux via PXE

Another route is [pxelinux](http://syslinux.zytor.com/wiki/index.php/PXELINUX) which can boot multiboot kernels via PXE. Since mach_kernel itself is not a multiboot-compliant kernel, we need to use a multiboot-compliant fork of boot-132 (such as Chameleon), and load it with the [mboot.c32](http://syslinux.zytor.com/wiki/index.php/Mboot.c32) pxelinux plug-in.

Basically, booting PureDarwin over the network involves the following steps:

1.  Client network card firmware asks DHCP server
2.  DHCP server asks dnsmasq on the server
3.  dnsmasq sends TFTP information
4.  Client network card firmware downloads pxelinux.0 file specified by dnsmasq from dnsmasq
5.  pxelinux downloads its configuration
6.  pxelinux loads mboot.c32 which is a multiboot loader
7.  mboot.c32 loads boot which is a multiboot-compliant version of boot-132 such as Chameleon (known to work up to this point)
8.  boot-132 loads mach_kernel and Extensions.mkext from a supplied ramdisk (still to be implemented)
9.  mach_kernel loads network kext and uses it to load the root volume from the network (still to be implemented)
10. Root volume image detects that it is booted via the network and sets up ramdisks etc. as needed (like on Mac OS X network boot)
You can try this variant by:
1.  Run Ubuntu 9.10 Live CD (we are using this as the server; later we could use PureDarwin as the server)
2.  `wget "http://sites.google.com/a/puredarwin.org/puredarwin/developers/booting/boot/pxe/boot-chameleon-over-pxelinux.bash?attredirects=0&d=1" -O ./boot-chameleon-over-pxelinux.bash ; sudo bash ./boot-chameleon-over-pxelinux.bash`
