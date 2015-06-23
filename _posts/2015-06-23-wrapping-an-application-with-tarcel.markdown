---
layout: article
title: "Wrapping an Application with Tarcel"
tags:
  - Tarcel
author:
  name: Lawrence Woodman
  url: /profile/lawrencewoodman/
---

Wrapping an application with [Tarcel](/projects/tarcel/) makes it much easier to distribute by allowing you to include all the files for the application along with the packages that it requires in a single file.  As an example we'll wrap Tarcel itself using the _tarcel.tarcel_ file in the [repo](https://github.com/LawrenceWoodman/tarcel).

Looking at the _tarcel.tarcel_ file, we first define the files that make up the utility:

{% highlight tcl %}
set appFiles [list \
  tarcel.tcl \
  [file join lib commands.tcl] \
  [file join lib parameters.tcl] \
  [file join lib xplatform.tcl] \
  [file join lib config.tcl] \
  [file join lib compiler.tcl] \
  [file join lib tvfs.tcl] \
  [file join lib tar.tcl] \
  [file join lib tararchive.tcl] \
  [file join lib embeddedchan.tcl]
]
{% endhighlight %}

Then define the modules it requires.  In this case it needs the `configurator` module and uses `find module` to locate it.

{% highlight tcl %}
set modules [list \
  [find module configurator]
]
{% endhighlight %}

We set `version` and `baseDir` for use later in the script.  `baseDir` will be the root of the _tarcel_ to which all other files in the _tarcel_ will be relative to.

{% highlight tcl %}
set version 0.1
set baseDir tarcel-$version.vfs
{% endhighlight %}

Now we can import the files defined above for the utility and put them in the `app` directory off of `$baseDir` while keeping their relative directory structure.

{% highlight tcl %}
import [file join $baseDir app] $appFiles
{% endhighlight %}

And fetch the modules defined above and place them directly in the `modules` directory off of `$baseDir` without keeping any relative directory structure.

{% highlight tcl %}
fetch [file join $baseDir modules] $modules
{% endhighlight %}

We'll set the `version` and `homepage` within the config to provide useful information via the `info` command of `tarcel.tcl`.

{% highlight tcl %}
config set version $version
config set homepage "https://github.com/LawrenceWoodman/tarcel"
{% endhighlight %}

Finally we set the script which will start the application.  Note that it adds the modules directory to the list of paths that are searched for modules.

{% highlight tcl %}
set initScript {
  ::tcl::tm::path add [file join @baseDir modules]
  source [file join @baseDir app tarcel.tcl]
}

config set init [string map [list @baseDir $baseDir] $initScript]
{% endhighlight %}


Once you have the _.tarcel_ file you can run _tarcel.tcl_ on it to wrap the files into a single package.  We'll create a _tarcel_ called _t.tcl_:
{% highlight bash %}
$ tclsh tarcel.tcl wrap -o t.tcl tarcel.tarcel
{% endhighlight %}


You can test that it worked by querying itself:
{% highlight bash %}
$ tclsh t.tcl info t.tcl
{% endhighlight %}

Which will give something like the following:

    Information for tarcel: t.tcl
    Created with tarcel.tcl version: 0.1

      Homepage: https://github.com/LawrenceWoodman/tarcel
      Version: 0.1
      Files:
        tarcel-0.1.vfs/app/lib/commands.tcl
        tarcel-0.1.vfs/app/lib/compiler.tcl
        tarcel-0.1.vfs/app/lib/config.tcl
        tarcel-0.1.vfs/app/lib/embeddedchan.tcl
        tarcel-0.1.vfs/app/lib/parameters.tcl
        tarcel-0.1.vfs/app/lib/tar.tcl
        tarcel-0.1.vfs/app/lib/tararchive.tcl
        tarcel-0.1.vfs/app/lib/tvfs.tcl
        tarcel-0.1.vfs/app/lib/version.tcl
        tarcel-0.1.vfs/app/lib/xplatform.tcl
        tarcel-0.1.vfs/app/tarcel.tcl
        tarcel-0.1.vfs/modules/configurator-0.1.tm

