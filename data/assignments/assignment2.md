# Assignment 2 - Create schema-constrained timetable data file(s)
ACIT4850 - BCIT - Winter 2019

## Assignment Goals

This assignment applies the techniques from seminars 3 through 6,
resulting in schema-constrained XML documents, with data coming from
a provided CSV file.

## Lab Submission

Use the same groups as for assignment 1.

A marking rubric will be attached to the dropbox.

Submit a compressed copy of your project folder (zip or tarball) to the appropriate D2L dropbox.

Due: Sunday, Mar 10, 23:30 PST

## And ... Here We Go

The CSV data file to use (`201830-Subject_Course Timetables - ttbl0010.csv`) is in 
our D2L content tab. Do not store it in a public repo!
Note the prefix - that designates the term the data pertains to.

Your job:
- design & create schemas for a student timetable XML document for a program (ACIT?),
and for an instructor timetable XML document for that same program
- the student timetable will be similar to the one from assignment 1
- the instructor timetable will be more complicated - it will need one pass through
the CSV data to identify the instructors for the desired program, and then
a second pass through the CSV data looking for rows that apply to one of
the selected instructors
- your app should take a command line parameter, namely the four-letter program code
to extract data for.
- build the XML documents programmatically from the CSV
- provide an HTML "report" from your XML document, with heading 2s for each block in your data, and
an unordered or comma-separated list of the courses that block has
- the blocks and courses within each are to be ordered alphabetically
- your code should be readable
- save your XML document with the same term as the CSV data, plus the program
you have extracted data for, separated by a hyphen; example: `201830-acit-students.xml`
and `201830-acit-instructors.xml`

Data hiccups:
- some of the data will need cleaning, i.e. leading asterisks removed, and whitespace trimmed
- we only want "active" courses
- we only want daytime courses, on weekdays (Mon-Fri)

Breaking news:
- validate your document manually, not through code like originally planned!
