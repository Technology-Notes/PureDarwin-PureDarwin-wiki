Bochs, the Open Source IA-32 emulation project
==============================================

<div style="display:inline;float:right;margin-top:5px;margin-right:10px;margin-bottom:5px;margin-left:10px">

<div style="display:inline;float:right;margin-top:5px;margin-right:10px;margin-bottom:5px;margin-left:10px">
[![](../_/rsrc/1226177739818/developers/bochs/Bochs%20logo.gif%3Fheight=96&width=87)](bochs/Bochs%20logo.gif%3Fattredirects=0)
Some Bochs attempts.
<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:300px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** Boot sequence](bochs.html#TOC-Boot-sequence)
    1.  [**1.1** The Plex86/Bochs VGABios](bochs.html#TOC-The-Plex86-Bochs-VGABios)
    2.  [**1.2** DFE bootloader](bochs.html#TOC-DFE-bootloader)
        1.  [**1.2.1** Decompressing extensions](bochs.html#TOC-Decompressing-extensions)
        2.  [**1.2.2** CPU:FSB multiplier detection fails](bochs.html#TOC-CPU:FSB-multiplier-detection-fails)
    3.  [**1.3** Chameleon bootloader](bochs.html#TOC-Chameleon-bootloader)
        1.  [**1.3.1** Kernel Panic!](bochs.html#TOC-Kernel-Panic-)
2.  [**2** Ressources](bochs.html#TOC-Ressources)

Boot sequence
-------------
### <span>T</span>he Plex86/Bochs VGABios

[![](../_/rsrc/1226178199483/developers/bochs/Bochx%20plex86%20vga%20bios.png%3Fheight=250&width=420)](bochs/Bochx%20plex86%20vga%20bios.png%3Fattredirects=0)
### DFE bootloader

[![](../_/rsrc/1226178191099/developers/bochs/Bochs%20dfe%20bootloader.png%3Fheight=249&width=420)](bochs/Bochs%20dfe%20bootloader.png%3Fattredirects=0)
#### Decompressing extensions

[![](../_/rsrc/1226177560822/developers/bochs/xnu%20decompression%20in%20Bochs.png%3Fheight=253&width=420)](bochs/xnu%20decompression%20in%20Bochs.png%3Fattredirects=0)
#### CPU:FSB multiplier detection fails

<span style="color:rgb(255,0,0)"><span style="font-weight:bold">Please, let us know if you have a solution.
</span></span>[![](../_/rsrc/1226178194045/developers/bochs/Bochs%20fails%20on%20CPUFSB%20detection.png%3Fheight=251&width=420)](bochs/Bochs%20fails%20on%20CPUFSB%20detection.png%3Fattredirects=0)
### Chameleon bootloader

[![](../_/rsrc/1226198887089/developers/bochs/Bochs%20chameleon%20bootloader.png%3Fheight=251&width=420)](bochs/Bochs%20chameleon%20bootloader.png%3Fattredirects=0)
#### Kernel Panic!

[![](../_/rsrc/1226198925227/developers/bochs/Bochs%20panic%20Voodoo.png%3Fheight=164&width=420)](bochs/Bochs%20panic%20Voodoo.png%3Fattredirects=0)


Far..


[![](../_/rsrc/1226673275719/developers/bochs/Bochs%20trying%20to%20boot%20PD%20on%20machbochs%20debug_on.png%3Fheight=323&width=420)](bochs/Bochs%20trying%20to%20boot%20PD%20on%20machbochs%20debug_on.png%3Fattredirects=0)


Then it stalls.

Ressources
----------
<http://bochs.sourceforge.net/> 
<http://code.google.com/p/xnu-dev> (Voodoo, A fork of the XNU kernel)
