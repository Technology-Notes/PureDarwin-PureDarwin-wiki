Kernel Extensions (KEXTs)
=========================
Darwin kernel extensions (KEXT) are modules that provide aditional functionality to the kernel, e.g., in the form of device drivers.

<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed sites-embed-full-width" style="width:100%;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** Minimal set of kexts needed](kexts.html#TOC-Minimal-set-of-kexts-needed)
2.  [**2** Installing kexts](kexts.html#TOC-Installing-kexts)
3.  [**3** Determining kext dependencies](kexts.html#TOC-Determining-kext-dependencies)
    1.  [**3.1** A visual overview of dependencies](kexts.html#TOC-A-visual-overview-of-dependencies)
4.  [**4** Extensions.mkext](kexts.html#TOC-Extensions.mkext)
    1.  [**4.1** Unpack Extensions.mkext](kexts.html#TOC-Unpack-Extensions.mkext)
5.  [**5** Force cache update](kexts.html#TOC-Force-cache-update)
6.  [**6** Types of kexts](kexts.html#TOC-Types-of-kexts)
    1.  [**6.1** Driver KEXTs](kexts.html#TOC-Driver-KEXTs)
    2.  [**6.2** Family KEXTs](kexts.html#TOC-Family-KEXTs)
    3.  [**6.3** Filesystem KEXTs](kexts.html#TOC-Filesystem-KEXTs)


### Minimal set of kexts needed
The kernel cannot boot at all (crashes) if there is not at least a certain set of KEXTs present.
The minimal set of KEXTs required to boot Darwin 9 from USB appears to be:
-   AppleACPIPlatform.kext
-   AppleAPIC.kext
-   AppleFileSystemDriver.kext
-   AppleRTC.kext
-   AppleSMBIOS.kext
-   IOACPIFamily.kext
-   IOATAFamily.kext
-   IOHIDFamily.kext
-   IOPCIFamily.kext
-   IOSCSIArchitectureModelFamily.kext
-   IOStorageFamily.kext
-   IOUSBFamily.kext
-   IOUSBMassStorageClass.kext
-   System.kext
This should get the system booting from USB (At the very least, it should go beyond the "<span style="font-style:italic">Got boot device</span>" phase, so that no message "<span style="font-style:italic">Still waiting for boot device...</span>" appears).
### Installing kexts
Copy the KEXT to <span style="font-style:italic">$VOLUME/System/Library/Extensions/</span> and fix permissions (important):

<span><span style="font-family:courier new,monospace"><span style="font-size:small">chown -R root:wheel $VOLUME/System/Library/Extensions/</span></span></span>
<span><span style="font-family:courier new,monospace"><span style="font-size:small">chmod -R 755 $VOLUME/System/Library/Extensions/</span></span></span>
### Determining kext dependencies
The minimal set of KEXTs above cannot, for example, boot from a CD, since the CD driver is not there. Hence, if we want to boot from CD, for example, we need to install the CD driver KEXT and its dependencies. Thus, we need to identify the dependencies.

In order to determine the dependencies of a KEXT, we can use [kextlibs](http://developer.apple.com/documentation/Darwin/Reference/ManPages/man8/kextlibs.8.html).
Example:


<span><span style="font-family:courier new,monospace"><span style="font-size:small">kextlibs $VOLUME/System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns/AppleUSBCDC.kext</span></span></span>
<span><span style="font-family:courier new,monospace"><span style="font-size:small"><span style="color:rgb(68,68,68)">com.apple.kpi.libkern = 9.2.2</span></span></span></span>
<span><span style="font-family:courier new,monospace"><span style="font-size:small"><span style="color:rgb(68,68,68)">com.apple.kpi.iokit = 9.2.2</span></span></span></span>
<span><span style="font-family:courier new,monospace"><span style="font-size:small"><span style="color:rgb(68,68,68)">com.apple.iokit.IOUSBFamily = 3.0.8</span></span></span></span>

So in this example, <span style="font-family:courier new,monospace"><span style="font-size:small">AppleUSBCDC.kext</span></span> has three dependencies. To see which KEXT satisfies a dependency (aka dependents), use [kextfind](http://developer.apple.com/documentation/Darwin/Reference/ManPages/man8/kextfind.8.html):

<span><span style="font-family:courier new,monospace"><span style="font-size:small">kextfind -case-insensitive -bundle-id -substring 'com.apple.iokit.IOUSBFamily' -print</span></span></span>
<span><span style="font-family:courier new,monospace"><span style="font-size:small"><span style="color:rgb(68,68,68)">/System/Library/Extensions/IOUSBFamily.kext</span></span></span></span>

Of course, <span style="font-family:courier new,monospace"><span style="font-size:small">IOUSBFamily.kext</span></span> itself has its own dependencies, so we need to make that recursive if we want to find out all dependencies.
#### A visual overview of dependencies
Take a look at [Visualize KEXTs dependencies](kexts/kexts-dependencies-overview.html) page.
### Extensions.mkext
A cache of installed KEXTs is kept in order to speed up boot time.
#### Unpack Extensions.mkext
Extensions.mkext can be unpacked with the [mkextunpack](http://developer.apple.com/documentation/Darwin/Reference/ManPages/man8/mkextunpack.8.html) command:

<span><span style="font-family:courier new,monospace"><span style="font-size:small">mkextunpack -v -d /tmp/Extensions Extensions.mkext</span></span></span>
### Force cache update {style="margin:10px 10px 10px 0px;color:rgb(0,0,0)"}
<span style="font-size:small">Normally, <span style="font-family:courier new,monospace">Extensions.mkext</span> is updated whenever we add or remove extensions using the Finder on a Mac OS X system. However, if you install an extension as a plug-in of another (a subfolder into<span style="font-style:italic"> $VOLUME/System/Library/Extensions</span>), the automatic cache updater is not triggered.</span>

To force cache update, simply change the modification date of <span style="font-style:italic">$VOLUME/System/Library/Extensions</span>:

<span><span style="font-family:courier new,monospace"><span style="font-size:small">touch $VOLUME/System/Library/Extensions</span></span></span>

### Types of kexts
#### Driver KEXTs
(To be written)
#### Family KEXTs
Family KEXTs are not drivers. Family KEXTs generally contain classes that you are to subclass
#### Filesystem KEXTs {style="margin:10px 10px 10px 0px;background-color:transparent;color:rgb(0,0,0);font-family:Arial,Verdana,sans-serif;font-size:14px"}
Filesystem KEXTs are not drivers. 

