---
layout: article
title: "configurator v0.2 Released"
tags:
  - configurator
  - Tcl/Tk
author:
  name: Lawrence Woodman
  url: /profile/lawrencewoodman/
---
This release adds support for using command prefixes with slave commands.  The requirement came about from the [Tarcel](/projects/tarcel/) project which needed to call an object's method using the `my` command.  This change also makes the functionality of slave commands more similar to master commands.

### Using a Command Prefix with a Slave Command ###
The use of a command prefix with a slave command is demonstrated in the following code snippet:

{% highlight tcl %}
namespace eval TestHelpers {}

proc TestHelpers::setaplusx {x int value} {
  $int invokehidden set a [expr {$value + $x}]
}

test parseConfig-20 {Ensure that slave commands work as a command prefix with multiple parts} -setup {
  set script {
    setaplus3 7
    title "a is set to: $a"
  }

  set slaveCmds {
    setaplus3 {TestHelpers::setaplusx 3}
  }

} -body {
  parseConfig -slaveCmds $slaveCmds $script
} -result {title {a is set to: 10}}
{% endhighlight %}
