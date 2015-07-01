---
layout: article
title: "Turning Multiple Files into a Module Using Tarcel"
tags:
  - Tarcel
  - Tcl/Tk
author:
  name: Lawrence Woodman
  url: /profile/lawrencewoodman/
---
One of the reasons for creating [Tarcel](/projects/tarcel/) was to make it easier to create modules out of multiple files.  Modules have definite benefits: they are faster to load and are simpler to deploy.

As an example, if you have the following files that you would like to put together to form a module called _translator-0.1.tm_:

    translator.tcl
    lib/deutsch.tcl
    lib/english.tcl
    lib/italiano.tcl


_translator.tcl_ contains:

{% highlight tcl %}
namespace eval translator {
  variable translations [dict create]
}

proc translator::addTranslations {language _translations} {
  variable translations
  dict set translations $language $_translations
}

source lib/deutsch.tcl
source lib/english.tcl
source lib/italiano.tcl

proc translator::englishTo {language englishWord} {
  variable translations
  dict get $translations $language $englishWord
}
{% endhighlight %}

_lib/deutsch.tcl_ contains:

{% highlight tcl %}
translator::addTranslations deutsch {
  welcome willkommen
} 
{% endhighlight %}

_lib/english.tcl_ contains:

{% highlight tcl %}
translator::addTranslations english {
  welcome welcome
}
 
{% endhighlight %}

_lib/italiano.tcl_ contains:

{% highlight tcl %}
translator::addTranslations italiano {
  welcome benvenuti
} 
{% endhighlight %}


You can create a module, _translator-0.1.tm_, from these files using the following _translator.tarcel_ file:
{% highlight tcl %}
set files [list \
  translator.tcl \
  [file join lib deutsch.tcl] \
  [file join lib english.tcl] \
  [file join lib italiano.tcl]
]

set version 0.1
set baseDir translator-$version.vfs
set outputFilename translator-$version.tm

import [file join $baseDir app] $files

config set version $version
config set homepage "http://example.com/project/translator"

set initScript {
  source [file join @baseDir app translator.tcl]
}

config set init [string map [list @baseDir $baseDir] $initScript]
config set outputFilename $outputFilename
{% endhighlight %}

Wrap the files into a module using the following:

{% highlight bash %}
$ tclsh tarcel.tcl wrap translator.tarcel
{% endhighlight %}

Now create a test file, _translator.test.tcl_:

{% highlight tcl %}
package require tcltest
namespace import tcltest::*

::tcl::tm::path add [file normalize .]
package require translator

test englishTo-1 {Ensure translates from English to German} -body {
  translator::englishTo deutsch welcome
} -result {willkommen}

test englishTo-2 {Ensure translates from English to Italian} -body {
  translator::englishTo italiano welcome
} -result {benvenuti}

cleanupTests
{% endhighlight %}

Finally you can now run the test and see that it treats the module just as if it was originally created as a single file:

{% highlight bash %}
$ tclsh translator.test.tcl
{% endhighlight %}


### Find out More ###
To find out more have a look at the [Tarcel Project](/projects/tarcel/) page and the following articles:<br />
  <ul id="briefPosts">
    {% for post in site.posts %}
      {% if post.tags contains 'Tarcel' and post != page %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
    {% endfor %}
  </ul>
