efiboot
=======
The efiboot project contains boot.efi, the bootloader that is used in Intel Macs to boot the system. 
<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** What is boot.efi](efiboot.html#TOC-What-is-boot.efi)
2.  [**2** Requirements](efiboot.html#TOC-Requirements)
3.  [**3** Installation](efiboot.html#TOC-Installation)
    1.  [**3.1** Blessing the volume](efiboot.html#TOC-Blessing-the-volume)
    2.  [**3.2** Moving boot.efi to UEFI-specified location](efiboot.html#TOC-Moving-boot.efi-to-UEFI-specified-location)
4.  [**4** Using boot.efi in VirtualBox](efiboot.html#TOC-Using-boot.efi-in-VirtualBox)

### What is boot.efi {style="color:rgb(0,0,0);margin-top:10px;margin-right:10px;margin-bottom:10px;margin-left:0px"}
boot.efi is a Universal EFI binary with 2 architectures, i386 and x86_64.
<span style="font-size:13px;font-weight:normal">It can be used to boot Intel-based Macintosh computers with both 32-bit (e.g., first-generation MacBook Pro) and 64-bit firmwares (e.g., Santa Rosa MacBook Pro).</span>
### Requirements
efiboot requires EFI to work. Computers that do not have EFI need to use other bootloaders, such as boot. To use efiboot, you need:
-   An Intel-based Macintosh (or another computer with EFI that can read HFS+; no such computer is known)
-   A HFS+ volume containing a Darwin system
-   efiboot
-   bless
### Installation
To make a volume bootable through EFI, efiboot needs to be installed. Simply copying boot.efi, however, is not sufficient. To make it work, there are basically two options: Blessing the volume and moving boot.efi to a special, UEFI-specified location (to be verified).
#### Blessing the volume
Assuming your Darwin volume is mounted to $MOUNT, do:


<span style="font-family:courier new,monospace">"$MOUNT/usr/sbin/bless" -verbose -folder "$MOUNT/System/Library/CoreServices" -bootinfo -bootefi</span>
<span style="font-family:inherit">
</span>
<span><span style="font-family:inherit">The above command implements an Xinfo cache for PowerPC processors which is not needed in the case of an Intel-based Macintosh.</span></span>
<span style="font-family:inherit">
</span>
<span><span style="font-family:inherit">For PureDarwin a simplified command can be used that will bless for only EFI based Macintosh computers.</span></span>
<span style="font-family:courier new">
</span>
<span style="font-family:courier new">"$MOUNT/usr/sbin/bless" --folder "$MOUNT/System/Library/CoreServices" -bootefi --verbose</span>

<span style="font-family:inherit">The volume should now be bootable.</span>
#### Moving boot.efi to UEFI-specified location
The firmware of Intel-based Macintosh computers implement sections 3.1 and 3.2 of the UEFI 2.0 specification.
UEFI 2.0 defines that the firmware looks for an EFI bootloader at the following locations:
-   /EFI/BOOT/BOOTX64.EFI
-   /EFI/BOOT/...
of FAT16-formatted volumes. Whether this can also be used to boot a Darwin system from a HFS+ volume needs to be verified.

### Using boot.efi in VirtualBox
EFI emulation in [VirtualBox](../virtualbox.html) 4 is capable of loading and executing boot.efi as the bootloader. Hence boot-132 or its derivatives are no longer needed in VirtualBox. We were able to boot PureDarwinNano.iso this way.


[![](../../_/rsrc/1295181011428/developers/booting/efiboot/boot.efi.png)](efiboot/boot.efi.png%3Fattredirects=0)


