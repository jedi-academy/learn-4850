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

## Update - Mar 4/2019

- Structure your XML documents, rather than simply replicating the CSV data like you were
building a spreadsheet. For the student timetable data, I suggest sessions within courses within blocks/sections.
For the instructor timetable data, I suggest sessions within courses within instructors.
- I want to see data ordered, eg by block or alphabetically by instructor. 
This will make the XML easier for humans to "read", which is part of the reason
to use XML in the first place. You can do this with a somewhat clever function to add a
new "row" (aka a session or booking) to the DOM of the XML document currently being constructed ...
traverse the document until you find either the entry you want to add to, or the one past it; if
the entry of interest, drill down and repeat; if the next one, then you need to insert a new DOM
element before it and then drill down.
- Check the rubric! If it awards marks for an enumerated constraint, then you need to add an
enumerated constraint to your XSD.
- Items in the CSV data that suit constraining: block (4 alpha 1 digit 1alpha), CRN (always 5 digits),
course (4 alpha + 4 digits) day (mon-fri), begin time (>= 830), end time (<= 1720), start & end date
(dd mmm yyyy). 
- Not all sessions will have an instructor - some might be unsupervised, others might not have one
assigned yet.
- Use `xs:annotation` elements to describe each field (element or attribute), with a helpful comment
(eg class capacity) and not just an obvious one (eg holds an integer).
Remember to grab the `Annotations.xsl` file from the examples today, and bind your XSD to it.
- Clean up the cruft in your submission. I don't want to see burger code or data, or multiple versions
of your XSD, or a ton of debugging code in your app.
- Help keep your project "clean", by having your data in the `data` folder. It is disconcerting to find the
same named data file in both your project root as well as your data folder, for instance.
- DO test your schemas and validation. About 20% of the lab 5 and assignment1 submissions would not
validate :( That's marks off. Big marks off.
- If there is any doubt about how to run your app, eg because it expect camelcase names (don't do it) or
is running on a port other than 8080 (the default); include a readme file. In fact, a readme file
should be standard - how to run it, which npm modules you expect to be present.
- Be aware of case-sensitivity. I was looking at your projects in class today using Windows on my laptop.
Monday afternoon & sometimes Tuesday morning are the only times you can count on me using Windows to
check submissions. At all other times, I will be using Linux. If you have a file "Apple.xml", Windows
will find it even if you ask for "apple.xml". Linux won't. Your code then breaks. I get sad. Marks get deducted.
You get sad. Avoid all the tears by being careful with your filename case sensitivity.
In the real world, developers test for this using continuous integration (on a Linux host) or
by firing up a Linux VM to run/test their app, so there are no surprises when code is deployed
to production.
- A wise man once said that documentation is the root of all evil. He probably didn't like
reams of paper to wade through. A wiser man once said that a bit of carefully crafted documentation
and/or code structure will improve the readability and the perceived worth of a project. Some
of your projects are getting very obtuse (hard to read/follow), for yourselves as well as an outsider.
You said in class today that you are familiar with jsdoc-style commenting. _Ding!_ (or whatever
sound a lightbulb coming on makes) -> you can use jsdoc-style block or inline comments to separate chunks of your code :)

### Reminder: you can ask for help

If you get stuck, ask for help! Post questions on our slack channel! Ask for a zoom session!

I have previous commitments Thursday morning, and all day Saturday, but can be available
Weds/Thu/Fri afternoon or late Weds/Thu evenings
.