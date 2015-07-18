Deploying DarwinBuild on PureDarwin
===================================
This page is about attempting to run DarwinBuild on a PureDarwin system (consequently, making PureDarwin a bit more self-hosted).

### Get Darwinbuild

#### SVN

```
svn co http://svn.macosforge.org/repository/darwinbuild/trunk darwinbuild
```

#### MacPorts

```
port archive darwinbuild
```

#### DarwinBuild

Unfortunately, there is no `darwinbuild` project in darwinbuild:

```
darwinbuild darwinbuild
ERROR: project not found: darwinbuild
```

Somebody needs to fix that (**TODO:** add `darwinbuild`)

### Compiling Darwinbuild in a PureDarwin chroot

See [Deploying MacPorts on PureDarwin](../macports/macportsonpuredarwin.html) for some prerequisites (gcc stuff and some usefull dev tools).

```
cd /Volumes/PureDarwin
# These dependencies will be needed:
tar xzvf /Volumes/Builds/9G55/Roots/.DownloadCache/libdyld.root.tar.gz                    
tar xzvf /Volumes/Builds/9G55/Roots/.DownloadCache/Libsyscall.root.tar.gz
```

We will temporary grab an old trick used by darwinbuild previously in order to avoid some project to raise this error: 

```
/bin/sh: dsymutil: command not found

chroot .
cp /usr/bin/true /usr/bin/dsymutil
LDFLAGS=-L/opt/local/lib make

make install

cd somewhere
darwinbuild -init <build version>
```

![](https://raw.github.com/wiki/PureDarwin/PureDarwin/images/osxpddbmp_interact.png)