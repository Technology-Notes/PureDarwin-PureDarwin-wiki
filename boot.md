boot
====
The boot project contains the bootloader that is used to boot the system on generic hardware through BIOS. 
<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** What is boot](boot.html#TOC-What-is-boot)
2.  [**2** Requirements](boot.html#TOC-Requirements)
3.  [**3** Installation](boot.html#TOC-Installation)
4.  [**4** Troubleshooting](boot.html#TOC-Troubleshooting)
    1.  [**4.1** Non-System Disk](boot.html#TOC-Non-System-Disk)
    2.  [**4.2** Wrong system booted by default](boot.html#TOC-Wrong-system-booted-by-default)

### What is boot
boot is a series of files that collectively act as bootloader for BIOS-based hardware.
-   <span style="font-weight:bold">boot</span> (sometimes referred to as "boot2") [HFS+ startup file](http://developer.apple.com/technotes/tn/tn1150.html#StartupFile), lives in the HFS+ file system structure (not a regular file). (Could also be used as a regular file; using a special version of boot1h that would no longer need it to be HFS+ the startup file.)
-   <span style="font-weight:bold">boot0 </span><span>[Master Boot Record](http://en.wikipedia.org/wiki/Master_boot_record) (MBR), lives in the first 440 bytes of the disk (not identical to the MBR from other OSes)</span>
-   <span style="font-weight:bold">boot1h</span> stage 1 booter (bootsector) for HFS+ partitions, lives in the first 512 bytes of the partition
-   <span style="font-weight:bold"><span style="color:rgb(102,102,102)">boot1u</span></span><span style="color:rgb(102,102,102)"> (not used by PureDarwin)</span>
-   <span style="font-weight:bold"><span style="color:rgb(102,102,102)">boot1u0</span></span><span style="color:rgb(102,102,102)"> (not used by PureDarwin)</span>
-   <span style="font-weight:bold">cdboot</span> booter for CD-ROMs (only works with valid Extensions.mkext due to a bug)
-   <span style="font-weight:bold"><span style="color:rgb(102,102,102)">chain0 </span><span><span style="color:rgb(102,102,102)">(</span></span></span><span><span style="color:rgb(102,102,102)">tbd; pro</span></span><span style="color:rgb(102,102,102)">bably not used by PureDarwin)</span>
Since Intel-based Macintosh computers use EFI instead of BIOS, they need to use a different bootloader, [efiboot](efiboot.html).
Another alternative to the above two bootloaders, is the new XNU loading support in [grub2](http://www.gnu.org/software/grub/grub-2.en.html), which works on both platforms (EFI and BIOS based) . For more information, check out [this](http://grub.enbug.org/XNUSupport) page.
### Requirements
To use boot, you need:
-   A MBR-formatted HFS+ volume containing a Darwin system
-   startupfiletool ([source](http://www.opensource.apple.com/darwinsource/projects/other/DarwinTools-1/))
-   boot-132 ([patched](http://tgwbd.org/darwin/boot.html) to include the EFI tables the Darwin system expects to be present)
### Installation
The disk needs to contain a HFS+ volume, and it must not be mounted.

<span style="font-family:courier new"></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">launchctl unload /System/Library/LaunchDaemons/com.apple.diskarbitrationd.plist</span></span>
[<span style="font-size:small"><span style="font-family:courier new,monospace">startupfiletool</span></span>](../../downloads.html)<span style="font-size:small"><span style="font-family:courier new,monospace"> ${RDEVICE}s1 boot</span></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">sleep 3</span></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">dd if=boot1h of=${RDEVICE}s1 bs=512 count=1</span></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">sleep 3</span></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">dd if=boot0 of=${DEVICE} bs=440 count=1</span></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">sync</span></span>
<span style="font-size:small"><span style="font-family:courier new,monospace">launchctl load /System/Library/LaunchDaemons/com.apple.diskarbitrationd.plist</span></span>


The chameleon boot loader (a fork of boot-132) has a greatly simplified installation procedure, since it does NOT need the disk to be unmounted, and it does NOT need startupfiletool:


<span style="font-family:courier new,monospace"><span style="font-size:small">fdisk -f boot0 -u -y /dev/rdiskX    # Install boot0 to the MBR</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">dd if=boot1h of=/dev/rdiskXsY       # Install boot1h to the partition's bootsector</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">cp boot /Volumes/XXX                # Install boot to the volume's root directory</span></span>

### Troubleshooting
#### Non-System Disk
<span style="font-weight:bold">Problem: </span>You get the error
<span style="font-family:courier new,monospace"><span style="font-size:small">Boot0: MBR</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">Boot0: Done</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">Non-System Disk</span></span>

<span style="font-weight:bold">Solution:</span> You need to set the partition bootable ("boot flag", "set partition active"), e.g., using fdisk:

<span style="font-family:courier new,monospace"><span style="font-size:small">sh-3.2# fdisk -e /dev/rdisk1</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">Enter 'help' for information  </span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">fdisk: 1&gt; f 1</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">Partition 1 marked active.</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">fdisk:*1&gt; w</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">Device could not be accessed exclusively.</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">A reboot will be needed for changes to take effect. OK? [n] y</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">Writing MBR at offset 0.</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">fdisk: 1&gt; q</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">sh-3.2#</span></span> 
#### Wrong system booted by default
<span style="font-weight:bold">Problem:</span> On a system with multiple OSes, boot wants to boot the wrong (non-PureDarwin) one by default. This happens when a non-HFS+ partition (e.g., FAT32 or NTFS) has the bootable ("active") flag and comes before the HFS+ partition that contains PureDarwin.

<span style="font-weight:bold">Solution:</span> Modify boot so that it uses the first HFS+ partition instead of the first partition with the bootable ("active") flag:

In boot-132/i386/lib/libsaio/sys.c, comment the following line:

<span style="font-family:courier new;font-size:12px"></span>
    for ( bvr = chain; bvr; bvr = bvr-&gt;next ) 
    { 
        if ( bvr-&gt;flags & kBVFlagNativeBoot ) bvr1 = bvr; 
        <span style="font-weight:bold">// if ( bvr-&gt;flags & kBVFlagPrimary )    bvr2 = bvr; </span>
    }


To be continued...

