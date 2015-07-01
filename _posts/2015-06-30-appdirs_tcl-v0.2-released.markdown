---
layout: article
title: "AppDirs v0.2 Released for Tcl"
tags:
  - AppDirs_tcl
  - Tcl/Tk
author:
  name: Lawrence Woodman
  url: /profile/lawrencewoodman/
---
This is a long overdue release which finally adds support for Darwin.  It was thanks to a request for help with Mac OS X that was answered by Steve Havelka.  With his initial code and advice on where Mac OS X stores files, this release ensures that AppDirs now supports the big three operating systems.

<h3>The New Locations</h3>
To see the full list of new directories run the _bin/listdirs.tcl_ script included in the repo which will show the following when from from Linux:

<pre style="font-size:95%">
The following shows the current values being used by AppDirs.


Current Operating System:
    dataHome:    /home/lorry/.local/share/myApp
    configHome:  /home/lorry/.config/myApp
    dataDirs:    /usr/share/myApp
    configDirs:  /etc/xdg/myApp


Below are listed the default directories used by the module AppDirs for
various operating systems.  The listings are using 'myUser' as the user
'myBrand' as the brand and 'myApp' as the name of the application.

Please note that these directories can change depending on how
environmental variables are set.


Linux:
    dataHome:    /home/myUser/.local/share/myApp
    configHome:  /home/myUser/.config/myApp
    dataDirs:    /usr/local/share/myApp /usr/share/myApp
    configDirs:  /etc/xdg/myApp


Windows 2000:
    dataHome:    C:\Documents and Settings\myUser\Application Data\myBrand\myApp
    configHome:  C:\Documents and Settings\myUser\Application Data\myBrand\myApp
    dataDirs:    {C:\ProgramData\myBrand\myApp}
    configDirs:  {C:\ProgramData\myBrand\myApp}


Windows Vista:
    dataHome:    C:\Documents and Settings\myUser\Application Data\myBrand\myApp
    configHome:  C:\Documents and Settings\myUser\Application Data\myBrand\myApp
    dataDirs:    {C:\ProgramData\myBrand\myApp}
    configDirs:  {C:\ProgramData\myBrand\myApp}


Windows XP:
    dataHome:    C:\Documents and Settings\myUser\Application Data\myBrand\myApp
    configHome:  C:\Documents and Settings\myUser\Application Data\myBrand\myApp
    dataDirs:    {C:\Documents and Settings\All Users\myBrand\myApp}
    configDirs:  {C:\Documents and Settings\All Users\myBrand\myApp}


Darwin:
    dataHome:    /Users/myUser/Library/Application Support/myBrand/myApp
    configHome:  /Users/myUser/Library/Application Support/myBrand/myApp
    dataDirs:    {/Library/Application Support/myBrand/myApp}
    configDirs:  {/Library/Application Support/myBrand/myApp}
</pre>
