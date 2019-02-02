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

Due: Sunday, Feb 10, 23:30 PST

## And ... Here We Go

The CSV data file to use (`201830-Subject_Course Timetables - ttbl0010.csv`) is in 
our D2L content tab. Do not store it in a public repo!
Note the prefix - that designates the term the data pertains to.

Your job:
- design & create a DTD for a student timetable XML document for a program (ACIT?)
- your app should take a command line parameter, namely the four-letter program code
to extract data for.
- build the XML document programmatically from the CSV
- provide an HTML "report" from your XML document, with heading 2s for each block in your data, and
an unordered or comma-separated list of the courses that block has
- the blocks and courses within each are to be ordered alphabetically
- your code should be readable
- save your XML document with the same term as the CSV data, plus the program
you have extracted data for, separated by a hyphen; example: `201830-acit.xml`

Data hiccups:
- some of the data will need cleaning, i.e. leading asterisks removed, and whitespace trimmed
- we only want "active" courses
- we only want daytime courses, on weekdays (Mon-Fri)

Breaking news:
- validate your document manually, not through code like originally planned!
