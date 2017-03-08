---
layout: default
title: "Home page"
---

# What is Progsnap?

Progsnap is a data format for programming assignment snapshots.

It is intended to allow sharing and analysis of data on student work on programming assignments and exercises.  In particular, it is designed to support fine-grained data such as keystroke-level edit events.

# Specification

Each Progsnap specification has a version number.

The current official version of the Progsnap specification is:

<div style="margin-left: 40px;"><a style="font-size: 120%;" href="spec-0.1.html">Progsnap specification 0.1</a></div>

The current development version of the Progsnap specification is:

<div style="margin-left: 40px;"><a style="font-size: 120%;" href="spec-0.2-dev.html">Progsnap specification 0.2-dev</a></div>

Note that development versions (with version numbers that end in "-dev") are under development, and should be expected to change.

# Tools

A Python library for reading Progsnap data is available:

> <https://github.com/cloudcoderdotorg/progsnap>

The intent of the Python library is to make it easy to automate analysis of Progsnap datasets.  See the [example scripts](https://github.com/cloudcoderdotorg/progsnap-examples).

# Who is this for?

This is for researchers in CS education, software engineering, and programming languages who are interested in using student programming data in their research.  By defining a common format for programming assignment data, Progsnap enables researchers to share data, and facilitates research that spans multiple institutions and courses.

A number of educational programming environments capture data as students work.  For example:

* [CloudCoder](http://cloudcoder.org)
* [PCRS](https://mcs.utm.utoronto.ca/~pcrs/pcrs/)
* TODO: other systems

Progsnap is intended to be general enough to capture data produced by these systems.  It is worth noting that Progsnap is not the native storage format for any system, so an export tool will need to be developed for each system to convert the system's data to Progsnap format.  However, once this conversion is done, any tool that understands the Progsnap format can make use of the data.

# Status

Version 0.1 of the specification, which is the first "official" version, is [available now](spec-0.1.html)!

We presented a poster at [SIGCSE 2017](http://sigcse2017.sigcse.org).  It presents a fairly complete overview of the project and what we're trying to accomplish:

> [Progsnap poster, SIGCSE 2017 \[PDF\]](media/progsnap-poster-sigcse2017.pdf)

If you would like to be involved in creating future versions of the specification, we would love to have your input!  See the Contact section below.

# Contact

People:

* David Hovemeyer, <dhovemey@ycp.edu> (primary contact)
* Jaime Spacco, <jspacco@knox.edu>
* Andrew Petersen, <andrew.petersen@utoronto.ca>
* Arto Hellas, <arto.hellas@cs.helsinki.fi>
* TODO: other people who want to contribute to this specification!

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
