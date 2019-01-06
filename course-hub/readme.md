# Course Hub Website

This is a course hub webapp that would suit BCIT courses.
It is currently used for 
- [COMP4711 Introduction to Internet Software Development](https://comp4711.jlparry.com)
- [ACIT 2910 - Projects](https://acit2910.jlparry.com)

The app is a [Flipped LMS](http://hibbittsdesign.org/blog/posts/2015-12-18-flipped-lms-using-an-open-and-collaborative-platform), 
with deep links into the D2L
instance for a course, instead of the conventional LMS,
where it is often awkward to re-arrange content or link
to an external source for it.

The site is XML-driven, abstracting as much as possible so that the
webapp can be used for other courses. XML was chosen over an RDB
because of the rich data structures it can support.

Basically, course material used to generate pages is put into an
appropriate subfolder inside /data, while images that are part
of the resulting pages would go underneath /public, for instance
inside /public/pix.

There are a few course metadata files directly inside /data, and
then subfolders for some common kinds of content.

What the end-user experiences is managed through the organizer.xml
document. 

Currently supported content formats include XML (to suit the S5
presentation framework) or MD (markdown).

## Play With It

You can just download the project and run it.
Configure your virtual host to use the project's `public` folder
as its document root.

Contribute to this project by making a pull request to the develop branch.

### Use It For Your Course

See the [setup doc](docs/setup.md) for directions to use this app
for one of your courses.

## Project Folders

/application	The course hub engine
/data           XML & markdown content for learning activities
/docs           Some helpful writeups
/public         public-facing website
/system		CodeIgniter 3.1.x framework
/xml            DTDs that cab be used to validate XML content

## Acknowledgement

This webapp was written by [James Parry](mailto:jim_parry@bcit.ca), Instructor in Computer Systems
Technology at the British Columbia Institute of Technology,
and Project Lead for CodeIgniter.

The Parsedown library comes from https://github.com/erusev/parsedown, and has an MIT license.

CodeIgniter is a project of B.C.I.T.
