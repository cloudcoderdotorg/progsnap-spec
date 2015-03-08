---
layout: default
title: "Specification"
---

# Introduction

Progsnap is a specification for data on student work on programming exercises and assignments.

<div class="callout">
This specification is at a very early stage of development.  If you'd like to contribute, see the <a href="index.html">home page</a> for contact info.
</div>

# Version

This is version **0.0-dev**.

Version numbers that end in "-dev" are under development, and should be expected to change.

# Data set, file encoding

A progsnap data set is a collection of files representing data from a single course.  In this context, "course" implies a single course at a single institution, possibly with multiple assignments, and possibly with multiple sections and/or instructors.   Studies involving multiple courses should use separate progsnap data sets for each course.

All of the files in a data set are relative to a common base directory, which this specification will refer to as *BaseDir*.  The files may be stored in a filesystem directory, or may be stored in a zip file.  Implementations of tools to read progsnap data sets should support reading from both directories and zip files.

Each file is a text file using the UTF-8 character encoding.

Each file is encoded as a sequence of lines.  Each line has the following format:

<pre>
{ "tag" : "<i>tagname</i>", "value" : <i>JsonObject</i> }
</pre>

Each line is a tagged data value, where *tagname* is the name of the tag, and *JsonObject* is a single [JSON](http://www.json.org/) object value.  Note that the encoding of *JsonObject* must not contain any newline characters.  In general, it is guaranteed that each line of the file encodes a single JSON object with two fields, `tag` and `value`.

# Basic data types

This section describes the basic data types used in files in progsnap data sets.

Note that these are just the basic data types.  In the description of the different kinds of files in progsnap data sets, compound data types will be defined which are built on (directly or indirectly) the basic data types.

Each basic data type is encoded using a JSON data type.

Basic data type | JSON data type | Notes
--------------- | -------------- | -----
*Int*           | number         | Signed, 64 bit
*Real*          | number         | Floating point, 64 bit
*String*        | string         | 

# Complex data types

This section describes the complex data types used in the various types of files in a progsnap data set.  All of the complex data types are represented as JSON objects.  As mentioned above related to file encoding, the JSON encoding of any value (belonging to a complex data type or a basic data type) must not contain any newline characters, so that the encoded value is guaranteed not to span multiple lines of the file that contains it.

TODO: complex data types.

# Types of files

This section describes the types of files in a progsnap data set.  For each file type, there is a particular naming scheme that instances of the file type must follow, and there are specific tagged values which may appear in the file, in a specified order.

## Dataset file

Each progsnap data set contains a single dataset file, whose path (relative to *BaseDir*) is `/dataset.dat`.

TODO: contents

## Student file

A progsnap data set may optionally contain a single student file, whose path (if present, relative to *BaseDir*) is `/students.dat`.

TODO: contents

## Assignment file

A progsnap data set must contain at least one assignment file, and may contain multiple assignment files.  An assignment file has a path (relative to *BaseDir*) of the form <code>/assignment<i>NNNN</i>.dat</i></code>, where *NNNN* is an integer assignment number.  It is recommended (but not required) that the assignment number is padded with leading zeroes as necessary so that all assignment filenames in a data set have the same length.

TODO: contents

## Work history file

A work history file represents one student's work on one assignment.  Each progsnap data set will typically have many work history files.  Work history files have paths (relative to *BaseDir*) of the form <code>/history\_<i>XXXX</i>\_<i>NNNN</i>.dat</code>, where *XXXX* is a student number, and *NNNN* is an assignment number.  It is recommended (but not required) that the student number and assignment number are padded with leading zeroes as necessary so that all assignment filenames in a dataset have the same length.

TODO: contents

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
