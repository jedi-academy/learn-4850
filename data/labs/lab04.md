# Lab04 - Constrain Your Sushi Menu With a DTD
ACIT4850 - BCIT - Winter 2019

## Lab Goals

This lab applies the techniques from seminar 04, to add simple constraints to
your sushi menu XML document.

## UPDATES

I have been unable to resolve the issue we had in class, trying to get our app
to validate an XML document against a DTD in nodejs. Support for DTDs is
very spotty in this world.

Accordingly, I am removing that expectation for your lab, and will give
everyone full marks for that rubric item.

I will be validating your XML+DTD inside NetBeans. I suggest you do the same inside
your IDE/editor. Just an FYI: Notepad++ (for those of you using Windows) has
an [XML Tools plugin](https://sourceforge.net/projects/npp-plugins/files/XML%20Tools/)
available. The link references v2.4.9, but you night be able to install a newer
version, 2.4.11, from the plugins manager inside NPP.

## Lab Submission

This is an individual lab.

A marking rubric will be attached to the dropbox.

Submit a compressed copy of your project folder (zip or tarball) to the appropriate D2L dropbox.

Due: Sunday, Feb 3, 17:30 PST

## And ... Here We Go

Enhance your Node app from last week, adding a DTD to constrain your
sushi menu XML document, and binding the XML document to it.
Your app should continue to present the menu as an HTML page,
on request, but it should also validate the document against
the DTD, and display the validation results to the console.

Use the enhanced menu from last week ("newmenu.xml") as
the menu to process. You do not need to add to it further.
