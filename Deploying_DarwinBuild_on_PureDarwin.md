Deploying DarwinBuild on PureDarwin
===================================
This page is about attempting to run DarwinBuild on a PureDarwin system (consequently, making PureDarwin a bit more self-hosted).
<span style="color:rgb(255,0,0)">This a work in progress.</span>

<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:350px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** Get Darwinbuild](darwinbuildonpuredarwin.html#TOC-Get-Darwinbuild)
    1.  [**1.1** SVN](darwinbuildonpuredarwin.html#TOC-SVN)
    2.  [**1.2** MacPorts](darwinbuildonpuredarwin.html#TOC-MacPorts)
    3.  [**1.3** DarwinBuild](darwinbuildonpuredarwin.html#TOC-DarwinBuild)
2.  [**2** Compiling Darwinbuild in a PureDarwin chroot](darwinbuildonpuredarwin.html#TOC-Compiling-Darwinbuild-in-a-PureDarwin-chroot)


### Get Darwinbuild
#### SVN
<span style="font-family:courier new,monospace"><span style="font-size:small">svn co http://svn.macosforge.org/repository/darwinbuild/trunk darwinbuild</span></span>
#### MacPorts
<span style="font-family:courier new;font-size:12px">port archive darwinbuild</span>
#### DarwinBuild
Unfortunately, there is no darwinbuild project in darwinbuild:

<span style="font-family:courier new,monospace"><span style="font-size:small">darwinbuild darwinbuild</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">ERROR: project not found: darwinbuild</span></span>

Somebody needs to fix that (<span style="font-weight:bold">TODO:</span> add darwinbuild)
### Compiling Darwinbuild in a PureDarwin chroot
See [Deploying MacPorts on PureDarwin](../macports/macportsonpuredarwin.html) for some prerequisites (gcc stuff and some usefull dev tools).


<span style="font-family:courier new,monospace"><span style="font-size:small">cd /Volumes/PureDarwin
</span></span>
These dependencies will be needed:

<span style="font-family:courier new,monospace"><span style="font-size:small">tar xzvf /Volumes/Builds/9G55/Roots/.DownloadCache/libdyld.root.tar.gz                    </span></span>

<span style="font-family:courier new,monospace"><span style="font-size:small">tar xzvf /Volumes/Builds/9G55/Roots/.DownloadCache/Libsyscall.root.tar.gz</span></span>


We will temporary grab an old trick used by darwinbuild previously in order to avoid some project to raise this error: 

<span style="font-family:courier new,monospace"><span style="font-size:small">/bin/sh: dsymutil: command not found</span></span>


<span style="font-family:courier new,monospace"><span style="font-size:small">chroot .</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">cp /usr/bin/true /usr/bin/dsymutil</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">LDFLAGS=-L/opt/local/lib make</span></span>

<span style="font-family:courier new,monospace"><span style="font-size:small">make install</span></span>

<span style="font-family:courier new,monospace"><span style="font-size:small">cd somewhere</span></span>
<span style="font-family:courier new,monospace"><span style="font-size:small">darwinbuild -init &lt;build version&gt;</span></span>
<span style="font-family:courier new;font-size:12px">
</span>
o//


[![](../../_/rsrc/1234069590942/developers/darwinbuild/darwinbuildonpuredarwin/osxpddbmp_interact.png)](darwinbuildonpuredarwin/osxpddbmp_interact.png%3Fattredirects=0)

