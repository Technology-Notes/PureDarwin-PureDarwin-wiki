Live CD
=======
Live CDs are a very popular way to distribute operating systems.
<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** How Live CDs work](live-cd.html#TOC-How-Live-CDs-work)
2.  [**2** How Mac OS X Installation DVDs work](live-cd.html#TOC-How-Mac-OS-X-Installation-DVDs-work)
3.  [**3** How a PureDarwin Live CD might work](live-cd.html#TOC-How-a-PureDarwin-Live-CD-might-work)
    1.  [**3.1** Alternative 1: Uncompressed from disc ](live-cd.html#TOC-Alternative-1:-Uncompressed-from-disc-)
    2.  [**3.2** Alternative 2: Using imageboot](live-cd.html#TOC-Alternative-2:-Using-imageboot)
    3.  [**3.3** Alternative 3: Using a ZFS file](live-cd.html#TOC-Alternative-3:-Using-a-ZFS-file)
4.  [**4** Shadow files](live-cd.html#TOC-Shadow-files)
5.  [**5** References](live-cd.html#TOC-References)

### How Live CDs work
In order to make a PureDarwin Live CD, we essentially need to do two things:
1.  <span style="font-weight:bold">Hardware detection</span>. Linux Live CDs configure themselves to use the correct drivers. While this is a major breaktrough for Linux distributions, Darwin has been working like this all the time. What this means for PureDarwin is that we essentially have to ensure that we ship a broad range of kexts, and the system does the rest "for free".
2.  <span style="font-weight:bold">Compressed main filesystem</span>. Linux Live CDs contain, besides the kernel, an initrd (a small filesystem) which loads the necessary drivers (roughly comparable to Extensions.mkext), mounts a 700 MB compressed filesystem (roughly comparable to a .dmg) that contains around 2GB of compressed software, switches over ("chroot") to that filesystem, and continues to boot from there. For PureDarwin, we can probably achieve this by using <span style="font-style:italic">NetBoot disk images</span> locally. 
3.  <span style="font-weight:bold">Union filesystem</span>. Because the main compressed filesystem is read-only, write accesses must be redirected to an overlay/union filesystem. Most Linux Live CDs use unionfs/aufs for this purpose. Darwin can use <span style="font-style:italic">shadow volumes</span> for the same purpose. A PureDarwin Live CD would <span style="font-style:italic">attach a shadow volume</span> that is stored in a local ramdisk to the root disk image. Mac OS X does this using the [nbdst](http://developer.apple.com/documentation/Darwin/Reference/ManPages/man8/nbdst.8.html) tool.
If anyone knows more about how to use NetBoot images locally, please join work on the Live CD.
### How Mac OS X Installation DVDs work
Mac OS X Installation DVDs are capable of running Mac OS X from a read-only medium. Maybe we can learn from that, hence we are looking at the procedure in this paragraph.

As of Snow Leopard, Mac OS X Installation DVDs contain the launchd LaunchDaemon plist file /System/Library/LaunchDaemons/com.apple.install.cd.plist with the following content:


<span style="font-size:small">&lt;?xml version="1.0" encoding="UTF-8"?&gt;</span>
<span style="font-size:small">&lt;!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd"&gt;</span>
<span style="font-size:small">&lt;plist version="1.0"&gt;</span>
<span style="font-size:small">&lt;dict&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;key&gt;Label&lt;/key&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;string&gt;com.apple.install.cd&lt;/string&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;key&gt;OnDemand&lt;/key&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;false/&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;key&gt;ProgramArguments&lt;/key&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;array&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;string&gt;/bin/sh&lt;/string&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;string&gt;/etc/rc.install&lt;/string&gt;</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">&lt;/array&gt;</span>
<span style="font-size:small">&lt;/dict&gt;</span>
<span style="font-size:small">&lt;/plist&gt;</span>


Hence, when the system is booted, the script <span style="font-family:courier new,monospace;font-size:small">/etc/rc.install <span style="font-family:Arial,Verdana,sans-serif;font-size:13px">is executed. This script mainly launches the Installer. However, there is also a script </span>/etc/rc.cdrom<span style="font-family:Arial,Verdana,sans-serif;font-size:13px"> which does the main magic of mounting ramdisks to </span></span>


<span style="font-size:small">#!/bin/sh</span>
<span style="font-size:small"># (...)</span>
<span style="font-size:small">
</span>
<span style="font-size:small">#</span>
<span style="font-size:small"># Disable prebinding-on-the-fly while we're CD booted</span>
<span style="font-size:small">#</span>
<span style="font-size:small">export DYLD_NO_FIX_PREBINDING=1</span>
<span style="font-size:small">
</span>
<span style="font-size:small">#</span>
<span style="font-size:small"># mount root_device to update vnode information</span>
<span style="font-size:small">#</span>
<span style="font-size:small">mount -u -o ro /</span>
<span style="font-size:small">
</span>
<span style="font-size:small"># (...)</span>
<span style="font-size:small">
</span>
<span style="font-size:small">#</span>
<span style="font-size:small"># Create a RAM disk with same perms as mountpoint</span>
<span style="font-size:small">#</span>
<span style="font-size:small">RAMDisk()</span>
<span style="font-size:small">{</span>
<span style="font-size:small">  mntpt=$1</span>
<span style="font-size:small">  rdsize=$2</span>
<span style="font-size:small">  echo "Creating RAM Disk for $mntpt"</span>
<span style="font-size:small">  dev=`hdik -drivekey system-image=yes -nomount ram://$rdsize`</span>
<span style="font-size:small">  if [ $? -eq 0 ] ; then</span>
<span style="font-size:small">    newfs_hfs $dev</span>
<span style="font-size:small">    # save & restore fs permissions covered by the mount</span>
<span style="font-size:small">    eval `/usr/bin/stat -s $mntpt`</span>
<span style="font-size:small">    mount -t hfs -o union -o nobrowse $dev $mntpt</span>
<span style="font-size:small">    chown $st_uid:$st_gid $mntpt</span>
<span style="font-size:small">    chmod $st_mode $mntpt</span>
<span style="font-size:small">  fi</span>
<span style="font-size:small">}</span>
<span style="font-size:small">
</span>
<span style="font-size:small">**RAMDisk /Volumes 1024**</span>
<span style="font-size:small">**RAMDisk /var/tmp 1024**</span>
<span style="font-size:small">**RAMDisk /var/run 1024**</span>
<span style="font-size:small">**
**</span>
<span style="font-size:small">**RAMDisk /var/db 1024**</span>
<span style="font-size:small">mkdir -m 1777 /var/db/mds</span>
<span style="font-size:small">
</span>
<span style="font-size:small"># language prefs, colorsync need to be able to write some preferences (5424449)</span>
<span style="font-size:small">**RAMDisk**</span><span style="white-space:pre"><span style="font-size:small"> **** </span></span><span style="font-size:small">**/Library/Preferences 1024**</span>
<span style="font-size:small">**RAMDisk /Library/ColorSync/Profiles/Displays 2048**</span>
<span style="font-size:small">
</span>
<span style="font-size:small"># use or create the boot cache playlist, and allow B&I to force 32-bit playlist generation</span>
<span style="font-size:small">FORCETHIRTYTWO="false"</span>
<span style="font-size:small">if nvram boot-args | grep "no64exec" ; then</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">FORCETHIRTYTWO="true"</span>
<span style="font-size:small">fi</span>
<span style="font-size:small">
</span>
<span style="font-size:small">SIXTYFOURBIT=`sysctl -n hw.cpu64bit_capable`</span>
<span style="font-size:small">
</span>
<span style="font-size:small">if [ $SIXTYFOURBIT = "0" -o $FORCETHIRTYTWO = "true" ] ; then</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">echo "using 32-bit bootcache playlist"</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">BootCacheControl -f /var/db/BootCache.playlist32 start</span>
<span style="font-size:small">elif [ $SIXTYFOURBIT = "1" ] ; then</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">echo "using 64-bit bootcache playlist"</span>
<span style="white-space:pre"><span style="font-size:small"> </span></span><span style="font-size:small">BootCacheControl -f /var/db/BootCache.playlist start</span>
<span style="font-size:small">fi</span>
<span style="font-size:small">
</span>
<span style="font-size:small"># tell launchd to commence with loading the system.</span>
<span style="font-size:small"># for the OS Install environment only, /etc/rc.install is included in this process.</span>
<span style="font-size:small">launchctl load -D system</span>
<span style="font-size:small">
</span>
<span style="font-size:small"># this script sleeps forever; the installer or startup disk will always reboot the system. </span>
<span style="font-size:small">sleep 9999999</span>

How does this script get called? **How can hdik be replaced with what is in Darwin? **Please let us know. Perhaps we could use a [MacFUSE](macfuse.html) based ramdisk from http://thebends.googlecode.com/svn/trunk/ramdisk/

So far, when replacing the above RAMDisk function with one based on MacFUSE's mount_ramdisk, we get
launchctl error: launch_msg(): Socket is not connected
<span style="color:rgb(255,0,0)">Please let us know if you know how to solve this.</span>
### How a PureDarwin Live CD might work
#### Alternative 1: Uncompressed from disc 
Run the system uncompressed from the CD medium (like PureDarwin nano or like the Mac OS X Install DVD). 

This is not a workable option for two reasons: It's slow (since more data needs to be read from the slow optical disc) and it's too small (700 MB is not much space).
#### Alternative 2: Using imageboot
The XNU kernel contains some references to "imageboot", e.g., here:
<http://fxr.watson.org/fxr/source/bsd/kern/imageboot.c?v=xnu-1228> 

Apparently, one should be able to boot using a boot argument like <span style="font-family:courier new;font-size:12px">-v rp=file:///some.dmg</span>
then the system would mount /some.dmg as the root device and continue booting from there.

How this works is largely undocumented. Below is a proof-of-concept for imagebooting PureDarwin nano on a Mac host.

What follows are proof-of-concept instructions that show how to boot into puredarwin.iso (from the PureDarwin nano image on the download page) that is located at / on a <span style="font-style:italic">Mac</span> running Mac OS X 10.5.4. <span style="font-weight:bold">Please do not try this on a production machine. Only try this if you have a spare Mac.</span>
Download PureDarwin nano, it contains puredarwin.iso which we will use as the imageboot image
On the Mac running Mac OS X (tested with 10.5.4), copy puredarwin.iso to /
Change IOHDIXController.kext and its plug-ins so that they are loaded at boot time
(normally, this is only the case for NetBoot):
-   <span style="font-family:courier new,monospace"><span style="font-size:small">cd /System/Library/Extensions/IOHDIXController.kext/</span></span>
-   <span style="font-family:courier new,monospace"><span style="font-size:small">find . -type f | xargs perl -i -p -e 'print STDERR 
        "changed $val values on line $. of $ARGVn" if($val = s@Network-Root@Root@g)'</span></span>
-   <span style="font-family:courier new,monospace"><span style="font-size:small">touch /System/Library/Extensions</span></span>
Edit /Library/Preferences/SystemConfiguration/com.apple.Boot.plist so that it contains:
<span style="font-family:courier new,monospace"><span style="font-size:small">&lt;key&gt;Kernel Flags&lt;/key&gt;
&lt;string&gt;<span style="font-weight:bold">-v r</span><span style="font-weight:bold">p=file:///puredarwin.iso</span>&lt;/string&gt;</span></span>
Reboot the Mac. It should now boot into PureDarwin, running from puredarwin.iso

You have now performed an ImageBoot of PureDarwin nano on a Mac OS X host. 

To undo, remove the extra Kernel Flags from your Boot.plist.

There are (unverified) news reports that cite "ImageBoot" as an official feature in the not-yet-released Snow Leopard:
<http://www.macnn.com/articles/08/10/20/more.snow.leopard.changes> 


However, all attempts to make this work on a Darwin host so far resulted in <span style="font-family:courier new;font-size:12px">di_root_image failed<span style="font-family:Arial;font-size:13px"> as it appears that IOHDIXController.kext (a kernel extension from Mac OS X that deals with disk images) and its respective plug-ins must be present and loaded for imageboot to work. Unfortunately, this kext seems not to be available for Darwin at this time (even though [this posting](http://lists.apple.com/archives/darwin-development/2002/Oct/msg00302.html) suggests that it might be available as a binary root under the Driver License).</span></span>

According to Singh, the kernel can be forced to use the BSD vndevice instead of hdix by specifying vndevice=1 as a boot argument. In this case, the function netboot_setup() from netboot.c is supposed to mount the file system contained in the vndevice node. But we can't get it to work yet (and it would only work for uncompressed images).
[![](../_/rsrc/1248650808188/developers/live-cd/imageboot%20failed%20ro.png)](live-cd/imageboot%20failed%20ro.png%3Fattredirects=0)

<span style="color:rgb(255,0,0)">Please let us know if you know if you are aware of a working solution that does not involve IOHDIXController.kext, or where  a recent version of IOHDIXController.kext can be downloaded under a suitable license. (It used to be available under the Apple Binary License, but it made its last appearance in Darwin 6...)</span>
<span style="color:rgb(255,0,0)">
</span>
As a totally desperate worst-case fallback scenario: The source code for mounting filesystem images is available, and there is source code for uncompressing compressed DMGs available. Maybe it would be feasible to write a minimal IOHDIXController.kext replacement that is able to mount compressed DMGs. <span style="color:rgb(255,0,0)">Volunteers?</span>
<span style="color:rgb(255,0,0)">
</span>
A set of NetBoot files created by System Image Utility looks like this:
<span style="font-family:courier new,monospace"><span style="font-size:small">
</span></span>

<span style="font-family:courier new,monospace"><span style="font-size:small">./i386</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./i386/booter</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./i386/mach.macosx</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./i386/mach.macosx.mkext</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./NBImageInfo.plist</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./NetBoot.dmg</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./ppc</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./ppc/booter</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./ppc/mach.macosx</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">./ppc/mach.macosx.mkext</span></span>


<span style="font-family:courier new,monospace"><span style="font-size:small">file NetBoot.dmg </span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">NetBoot.dmg: Apple Partition data block size: 2048, first type: Apple_partition_map, name: Apple, number of blocks: 63, second type: Apple_Driver43, name: Macintosh, number of blocks: 56, third type: Apple_Free, name: , number of blocks: 0, fourth type: Apple_Driver_ATAPI, name: Macintosh, number of blocks: 56</span></span>
<span style="font-family:courier new;font-size:12px">
</span>
<span style="font-family:courier new;font-size:12px"></span>
file i386/booter
i386/booter: Universal EFI binary with 2 architectures, i386, x86_64



(to be continued)
#### Alternative 3: Using a ZFS file
Maybe using a compressed ZFS file and manual trickery...
### Shadow files
This is a work in progress.


<span style="font-family:courier new,monospace"><span style="font-size:small">#!/bin/bash -e -x</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">
</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">mount -uw /</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">
</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">touch /shadowfile</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">
</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">/usr/libexec/vndevice attach /dev/vn0 /uncompressed.dmg</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">/usr/libexec/vndevice shadow /dev/vn0 /shadowfile</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">
</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">mount -t hfs /dev/vn0 /live/</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">
</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small"># /usr/libexec/vndevice detach /dev/vn0</span></span>

### References
-   System Image Utility.app (part of [Server Admin Tools](http://www.apple.com/downloads/macosx/apple/macosx_updates/serveradmintools105.html) for Mac OS X) can create NetBoot images; probably the same can be achieved with lower-level command line tools
-   [/etc/rc.netboot](http://www.opensource.apple.com/darwinsource/10.5/launchd-257/launchd/src/rc.netboot) ([mailing list entry](http://lists.apple.com/archives/Macos-x-server/2007/Dec/msg00150.html))
-   [BootMania](http://www2.tba.t-com.ne.jp/beanz/BootManiaDoc/index_e.html), a software that supposedly can boot Darwin (8) with NetBoot
-   [Server Admin Tools 10.5](http://www.apple.com/support/downloads/serveradmintools105.html), contains <span style="font-style:italic">System Image Utility</span> which is used in Mac OS X to make NetBoot images

