---
layout: default
title: "Specification 0.1-dev"
---

# Introduction

Progsnap is a specification for data on student work on programming exercises and assignments.

<div class="callout">
This specification is at a very early stage of development.  If you'd like to contribute, see the <a href="index.html">home page</a> for contact info.
</div>

# Version

This is version **0.1-dev**.

Version numbers that end in "-dev" are under development, and should be expected to change.

# Data set, file encoding

A progsnap data set is a collection of files representing data from a single course.  In this context, "course" implies a single course at a single institution, possibly with multiple assignments, and possibly with multiple sections and/or instructors.   Studies involving multiple courses should use separate progsnap data sets for each course.

All of the files in a data set are relative to a common base directory, which this specification will refer to as *BaseDir*.  The files may be stored in a filesystem directory, or may be stored in a zip file (such that *BaseDir* is the root directory of the zip file).  Implementations of tools to read progsnap data sets should support reading from both directories and zip files.

Each file is a text file using the UTF-8 character encoding.

Each file (with the exception of README.txt) is encoded as a sequence of lines.  Each line has the following format:

<pre>
{ "tag" : "<i>tagname</i>", "value" : <i>JsonValue</i> }
</pre>

Each line is a tagged data value, where *tagname* is the name of the tag, and *JsonValue* is a single [JSON](http://www.json.org/) value.  Note that the encoding of *JsonValue* must not contain any newline characters.  In general, it is guaranteed that each line of the file encodes a single JSON object with two fields, `tag` and `value`.

Each file specifies that lines with specified tags will occur in a specified order.  Each type of line is specified with a number of occurrences:

Occurrences | Meaning
----------- | -------
1           | A line with this tag must occur exactly one time
0..1        | A line with this tag is optional: there may be either 0 or 1 occurrences
0..\*       | There may be any number of occurrences (0 or more) of lines with this tag

Tag names starting with "x-" are guaranteed not to conflict with any official tag name, and lines containing such tags may be used by creators of progsnap data sets to store extra information.  Lines with custom tags may occur anywhere within any file in a progsnap data set.  Readers of progsnap data should ignore lines with such tags, or allow custom processing of them.

# Basic data types

This section describes the basic data types used in files in progsnap data sets.

Note that these are just the basic data types.  In the description of the different kinds of files in progsnap data sets, compound data types will be defined which are built on (directly or indirectly) the basic data types.

Each basic data type is encoded using a JSON data type.

Basic data type | JSON data type | Notes
--------------- | -------------- | -----
*Int*           | number         | Signed, 64 bit
*Boolean*       | boolean        | 
*Real*          | number         | Floating point, 64 bit
*String*        | string         | 
*Timestamp*     | number         | Floating point, 64 bits <sup>\*</sup>

<sup>\*</sup> *Timestamp* is number of milliseconds since midnight, January 1, 1970, UTC; timestamps are floating point, so precision greater than milliseconds is possible

# Complex data types

This section describes the complex data types used in the various types of files in a progsnap data set.

All of the complex data types are represented as JSON objects. The order of the fields is not specified, and when encoded in JSON, the fields may appear in any order.

All implementations of readers of progsnap data should be prepared to accept fields not mentioned in this specification.  For all complex data types, field names beginning with "x-" are guaranteed not to conflict with any "official" fields, and may be used by the creator of a progsnap data set to store extra information.  The value of a custom ("x-") field can be any JSON value.

As mentioned above related to file encoding, the JSON encoding of any value (belonging to a complex data type or a basic data type) must not contain any newline characters, so that the encoded value is guaranteed not to span multiple lines of the file that contains it. 

## *Assignment*

An *Assignment* value is an object specifying the location of an assignment file.

Field name | Type of value | Required? | Comment
---------- | ------------- | --------- | -------
path       | *String*      | yes       | path (relative to *BaseDir*) of the assignment file

Note that additional information about an assignment, such as metadata and information about tests, is stored in the assignment file (referenced by the path field of the *Assignment*.)

## *Test*

A *Test* value is an automated test (such as a unit test) associated with an assignment.

Because of the great diversity of programming languages and testing approaches that could be used in an assignment in a progsnap data set, *Test* values are not intended to precisely capture the exact semantics of a test.

Field name | Type of value | Required? | Comment
---------- | ------------- | --------- | -------
number     | *Int*         | yes       | test number
name       | *String*      | yes       | name of the test (e.g., unit test name)
input      | *String*      | no        | test input (e.g., argument values or input data)
output     | *String*      | no        | expected output (e.g., result value, output data, regexp, etc.)
opaque     | *Boolean*     | no        | if true, test input/output is not revealed to students
invisible  | *Boolean*     | no        | if true, the existence of the test is not revealed to students

If either the opaque or invisible field is not present, the implied default value is false.

## *Student*

A *Student* value is an object containing anonymized information about a student.

Field name | Type of value | Required? | Comment
---------- | ------------- | --------- | -------
number     | *Int*         | yes       | This student's student number
instructor | *Boolean*     | yes       | Whether student is actually an instructor or teaching assistant<sup>\*</sup>
gender     | *String*      | no        | Student's gender<sup>&dagger;</sup>
experience | *Int*         | no        | Self-reported prior experience: 0=none, 1=some, 2=prior coursework
major      | *String*      | no        | Student's major field of study
finished   | *Boolean*     | no        | Whether the student completed the course (false if student dropped or withdrew)
finalgrade | *Real*        | no        | Final grade in the course

<sup>\*</sup> Instructors and teaching assistants will often have accounts for testing purposes; this field is used to identify those accounts

<sup>&dagger;</sup> Possible values include "m" and "f", but implementations should accept other values

## *Edit*

TODO

## *Submission*

TODO

## *Compilation*

TODO

## *TestResults*

TODO

## TODO: other complex data types

TODO

# Types of files

This section describes the types of files in a progsnap data set.  For each file type, there is a particular naming scheme that instances of the file type must follow, and there are specific tagged values which may appear in the file, in a specified order.

## README.txt

Each progsnap data set should have a file whose path (relative to *BaseDir*) is `/README.txt`.  This file is a free-form text file with descriptive information about the data set.  If the data set contains any custom tags or fields (starting with "x-"), their meaning should be documented here.  This file is also a good place to document how to interpret values whose meanings may vary by course/institution, such as the numeric range for final course grades.

## Dataset file

Each progsnap data set contains a single dataset file, whose path (relative to *BaseDir*) is `/dataset.dat`.

The dataset file specifies general information about the data set.  It contains the following lines, in the following order.

Tag name | Type of value | Occurrences | Comment
-------- | ------------- | ----------- | -------
psversion | *String*     | 1           | the version of the progsnap specification the data set conforms to, e.g., "0.1"
name     | *String*      | 1           | name of data set, e.g., "CS 101, Spring 2015, Unseen University"
contact  | *String*      | 1           | name of person to contact regarding the data set
email    | *String*      | 1           | email address of person to contact regarding the data set
courseurl | *String*      | 0..1        | optional URL of web page for the course

## Assignments file

Each progsnap data set contains a single assignments file, whose path (relative to *BaseDir*) is `/assignments.dat`.

The assignments file specifies the assignments that are included in the data set.  It contains the following lines in the following order:

Tag name | Type of value | Occurrences | Comment
-------- | ------------- | ----------- | -------
assignment | *Assignment* | 0..\*      | Reference to an assignment file

Note that any useful progsnap data set will contain at least one assignment, since work history files are associated with assignments.

## Students file

A progsnap data set may optionally contain a single students file, whose path (if present, relative to *BaseDir*) is `/students.dat`.

The students file specifies anonymized information about students in the course the data set represents.  It contains the following lines in the following order:

Tag name | Type of value | Occurrences | Comment
-------- | ------------- | ----------- | -------
student  | *Student*     | 0..\*       | Information about a student

## Assignment file

A progsnap data set must contain at least one assignment file, and may contain multiple assignment files.  An assignment file has a path (relative to *BaseDir*) of the form <code>/assignment\_<i>NNNN</i>.dat</i></code>, where *NNNN* is an integer assignment number.  It is recommended (but not required) that the assignment number is padded with leading zeroes as necessary so that all assignment filenames in a data set have the same length.

An assignment file contains the following lines in the following order:

Tag name   | Type of value | Occurrences | Comment
---------- | ------------- | ----------- | -------
name       | *String*      | 1           | the assignment name, e.g., "Assignment 1: Tic-Tac-Toe"
url        | *String*      | 0..1        | URL of a web page describing the assignment
assigned   | *Timestamp*   | 0..1        | timestamp indicating when the assignment was made available to students)
due        | *Timestamp*   | 0..1        | timestamp indicating when the assignment was due
test       | *Test*        | 0..\*       | test cases for the assignment

## Work history file

A work history file represents one student's work on one assignment.  Each progsnap data set will typically have many work history files.  Work history files have paths (relative to *BaseDir*) of the form <code>/history\_<i>NNNN</i>\_<i>XXXX</i>.dat</code>, where *NNNN* is an assignment number, and *XXXX* is a student number.  It is recommended (but not required) that the student number and assignment number are padded with leading zeroes as necessary so that all work history filenames in a dataset have the same length.

Each line in a work history file represents an event.  One common feature of each event is that the value of the line is guaranteed to have a field called "ts" whose value is a *Timestamp*, which records the time when the event occurred.  The lines in a work history file (with the possible exception of lines with custom tags) are ordered by nondecreasing event timestamp values.

The types of lines (representing events) are the following (note that this table does *not* indicate a required order, since events are ordered by timestamp):

Tag name    | Type of value | Comment
----------- | ------------- | -------
edit        | *Edit*        | Code edit
submission  | *Submission* | Submission for grading and/or automated testing
compilation | *Compilation* | Code compilation attempt
testresults | *TestResults* | Results of automated testing

<!-- vim:set wrap: ­-->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
