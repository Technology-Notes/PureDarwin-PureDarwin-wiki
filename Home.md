Welcome
=======

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/PD-Opennow.jpg)


[Darwin](http://en.wikipedia.org/wiki/Darwin_%28operating_system%29) is the Open Source operating system from Apple that forms the basis for Mac OS X, and PureDarwin is a community project to make Darwin more usable (some people think of it as the informal successor to OpenDarwin).

One current goal of this project is to provide a useful bootable ISO of Darwin 10.x and Darwin 9.x.
Another goal of this project is to provide additional documentation. [More](welcome/about.html)...

Documentation and quick hints
-----------------------------
This pseudo-wiki is globally divided in 3 parts:
-   [For Users](users.html)
-   [For Developers](developers.html)
-   [For the Curious](curious.html)
A [mailing list](https://groups.google.com/forum/?fromgroups#!forum/puredarwin) for interested developers, and users of PureDarwin is available on Google Groups.

*NEW!* Getting the code
-------------------------
We have just finished migrating the project repository from [Google Code](https://code.google.com/p/puredarwin), to [GitHub](https://github.com/PureDarwin/PureDarwin), and encourage visitors to use the GitHub repository for both contribution, and checking out the latest build source. 

Additionally, as an interim measure, a [version of PureDarwin Xmas](https://puredarwin.googlecode.com/files/NewBootEnvironment-XMas-1.7z) with a fixed boot sector, which is compatible with QEMU is also available. 

Status
------

MacPorts is running on PureDarwin 9, potentially giving us thousands of open source software titles.

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/macports_on_pd.jpg)

The screenshot above shows a PureDarwin 9 system created from the scripts in our repository, running on real hardware, using TightVNC and [XFCE](users/xfce.html) from MacPorts.

This page shows where we currently are (see the [current blockers</span>](blockers.md)).

As of now, you can either build a PureDarwin system yourself based on the information on this site and our and Apple's Open Source and DarwinBuild source code repositories, or download "[PureDarwin Xmas](downloads/xmas.html)", a developer preview of the operating system based on Apple's Darwin 9 sources and other Open Source projects.

At the same time, the PureDarwin project would like to invite the community to discuss, participate and contribute.

The developer preview is available for download as a pre-configured virtual machine for [VMware](developers/vmware.html) Fusion 2.0 on Macintosh, and the code used to generate it is available in a Subversion/Mercurial repository form.

A minimal PureDarwin system known as "[PureDarwin nano](downloads/puredarwin-nano.html)" is also available, where only one process is running (a shell).
Credits
-------

We would like to thank
-   Apple, Inc. for releasing Darwin as Open Source 
-   kvv and _wms for their continuing help
-   David Elliott for his work on boot-132
-   The Chameleon team for their work on boot-132
-   The xnu-dev team for their work on the XNU kernel
-   Stuart Crook for his work on PureFoundation
-   Guillaume Verdeau for his work on X.Org
-   Rafirafi for his work on Generic Platform kexts
-   Mac OS Forge 
-   The DarwinBuild project 
-   The MacPorts project
-   The folks at #macosforge, #macports, #macdev, #opendarwin, #puredarwin, 
-   Everyone else contributing to Darwin 