Bonjour
=======
<div style="display:inline;float:right;margin-top:5px;margin-right:10px;margin-bottom:5px;margin-left:10px">
[![](http://upload.wikimedia.org/wikipedia/en/9/91/Apple_Bonjour_Icon.png)](http://upload.wikimedia.org/wikipedia/en/9/91/Apple_Bonjour_Icon.png)
Bonjour is Apple's implementation of the [Zeroconf](http://en.wikipedia.org/wiki/Zeroconf) service discovery protocol. mDNSResponder is the open source software from Apple that implements Bonjour in both Mac OS X and Darwin.
<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** Announcing services](bonjour.html#TOC-Announcing-services)
    1.  [**1.1** Manually](bonjour.html#TOC-Manually)
    2.  [**1.2** Automatically](bonjour.html#TOC-Automatically)
2.  [**2** Browsing services](bonjour.html#TOC-Browsing-services)
3.  [**3** References](bonjour.html#TOC-References)

### Announcing services
#### Manually
To manually announce services on the local network, you can use the mDNS command.

<span style="font-size:small"># Announce HTTP server</span>

<span style="font-size:small">mDNS -R . _http._tcp . 80</span>
<span style="font-size:small">
</span>
<span style="font-size:small"># Announce AFP server</span>
<span style="font-size:small">mDNS -R . _afpovertcp._tcp. 548 </span>

More service types are defined at http://www.dns-sd.org/ServiceTypes.html
#### Automatically
Servers on Darwin systems are usually launched through launchd. You can use the <span style="font-size:small">Bonjour</span> key in LaunchDaemons plist files to have launchd automatically announce services.
### Browsing services
To browse announced services on the Mac, you can use [Bonjour Browser](http://www.tildesoft.com/Programs.html#BonjourBrowser).
### References
-   <http://developer.apple.com/opensource/internet/bonjour.html>
-   [DNS SRV (RFC 2782) Service Types](http://www.dns-sd.org/ServiceTypes.html)

