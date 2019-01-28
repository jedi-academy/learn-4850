# Assignment 1 - Create a constrained timetable data file
ACIT4850 - BCIT - Winter 2019

## Assignment Goals

This assignment applies the techniques from seminars 1 through 4,
resulting in a DTD-constrained XML document, with data coming from
a provided CSV file.

## Lab Submission

You can complete this assignment on your own, or as a pair.
Either way, sign up for a "team" on D2L, as the dropbox is a group one.

A marking rubric will be attached to the dropbox.

Submit a compressed copy of your project folder (zip or tarball) to the appropriate D2L dropbox.

Due: Sunday, Feb 10, 17:30 PST

## And ... Here We Go

CSV data file is in D2L content. Do not store it in a public repo!
It is an example only.


Your job:
- design & create a DTD for a student timetable XML document for a program (ACIT)
- build the XML document programmatically from the CSV
- validate it, results to the console
- provide an HTML "report", with heading 2s for each block i your data, and
an unordered or comma-separated list of the courses that block has
- the blocks and courses within each are to be ordered alphabetically
- your code should be readable

Data hiccups:
- some of the data will need cleaning, i.e. leading asterisks removed, and whitespace trimmed
- we only want "active" courses
- we only want daytime courses, on weekdays
