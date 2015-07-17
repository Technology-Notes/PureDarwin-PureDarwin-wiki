Apache
======
<div style="display:inline;float:right;margin-top:5px;margin-right:10px;margin-bottom:5px;margin-left:10px">
[![](../_/rsrc/1263721596996/users/apache/apache_logo.png)](apache/apache_logo.png%3Fattredirects=0)
Apache is the most popular HTTP server software in use. It can be installed on PureDarwin, e.g., from the MacPorts project.
<div class="sites-embed-align-left-wrapping-off">
<div class="sites-embed-border-off sites-embed" style="width:250px;">
<div class="sites-embed-content sites-embed-type-toc">
<div class="goog-toc sites-embed-toc-maxdepth-6">
Contents
1.  [**1** Installing apache](apache.html#TOC-Installing-apache)
2.  [**2** Configuring apache](apache.html#TOC-Configuring-apache)
3.  [**3** Announcing apache in the local network](apache.html#TOC-Announcing-apache-in-the-local-network)

Installing apache
-----------------
Apache can be installed as <span style="font-size:small">apache2</span> port from MacPorts.
Configuring apache
------------------
The configuration files can be found in <span style="font-size:small">/opt/local/apache2/conf </span>if you have installed the apache2 package from MacPorts.

To load apache2 on every boot, run
<span style="font-size:small">sudo launchctl load -w /Library/LaunchDaemons/org.macports.apache2.plist</span>
Announcing apache in the local network
--------------------------------------
Running
<span style="font-size:small">dns-sd -R . _http._tcp. . 80</span>
should announce the web server in the local network via [Bonjour](bonjour.html).
The service then appears in the Safari Bonjour bookmarks folder. Using a '.' as the service name is equivalent to no service name, telling mDNS to use the computer name.
However, this appears not to work properly, since the announced domain name has the ".local." suffix which Apache apparently does not recognize?
To be continued.

