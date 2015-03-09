---
layout: default
title: "Home page"
---

# What is progsnap?

Progsnap is a data format for programming assignment snapshots.

It is intended to allow sharing and analysis of data on student work on programming assignments and exercises.  In particular, it is designed to support fine-grained data such as keystroke-level edit events.

# Resources

The [specification](spec.html) documents the data format.

Coming soon: code to read progsnap data, export progsnap data from various tools/systems.

# Who is this for?

This is for researchers in CS education, software engineering, and programming languages, who are interested in using student programming data in their research.  By defining a common format for programming assignment data, progsnap enables researchers to share data, and facilitates research that spans multiple institutions and courses.

A number of educational programming environments capture fine-grained data as students work.  For example:

* [CloudCoder](http://cloudcoder.org)
* TODO: other systems

Progsnap is intended to be general enough to capture data produced by these systems.  It is worth noting that progsnap is not the native storage format for any system, so an export tool will need to be developed for each system to convert the system's data to progsnap format.  However, once this conversion is done, any tool that understands the progsnap format can make use of the data.

# Status

This isn't ready for prime time yet.  In fact, how did you even find out this was here?

If you would like to be involved, we would love to have your input!  See the Contact section below.

# Contact

People:

* David Hovemeyer, <dhovemey@ycp.edu> (primary contact)
* Jaime Spacco, <jspacco@knox.edu>
* TODO: other people who want to contribute to this specification!

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
