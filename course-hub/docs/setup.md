# Course Hub Setup

## Installation

1. Create an empty github repository (with a readme or license file, so you can clone it).

2. Inside your local clone:

    git submodule add -b master https://github.com/jedi-academy/course-hub

3. Your project will now have a `course-hub` subfolder, incorporating the "course-hub" app.
It can be updated at any time with

    git submodule update --remote --merge

4. Copy the `data` and `public` folders from `course-hub` to your project root.
These will be the starters for your course.

5. Inside `public`, replace `index.php` (setup for standalone) with `data/index_submodule.php`
(setup for use as a course hub).

6. On your hosting platform, setup a virtual host or subdomain for your course,
and configure its document root to be the `public` folder inside your project.

7. Configure your course metadata, in the root of `data`.

8. Author your course content inside the subfolders of `data`, using `organizer.xml`
to control what your students see.

Note: The course hub does not use any RDB, nor does it have any sub-dependencies -
it is good to go at this point.

## Deployment

I have a deployment app available. It is meant to run on the same server as your course(s).
If you configure a "webhook" inside your course repo, the deployment app will
automatically pull your updated course content whenever you push or merge to
the "master" branch of your course repo.

Ask me for details, if interested.

## Course metadata (in /data)

Constraining documents for the course data files are found in the `public/xml` folder.
These have explanations for all the fields in your XML documents.

### course.xml

`data/course.xml` provides the basic data for your course.

The `ou` element provides the link to the D2L instance for your course!

Any of the elements inside this, for instance `ou`, can be referenced inside
your generated content, by surrounding the element name with braces,
as in `{ou}`.

### materials.md

`data/materials.md` is an XML fragment which describes the different icons your 
students will see in your course organizer.
It has an accompanying DTD, but is not meant to be validated.

### news.md

`data/news.md` is a markdown-formatted view fragment, whose contents
are presented at the top of your course organizer in a "News" panel.
It can be whatever suits your purposes.

### organizer.xml

`data/organizer.xml` is the XML document that defines the structure and
materials visible to your students. This is explained in more detail
in a [separate document](./organizer.md).

### resources.md

`data/resources.md`  is a list of resources to share with your students.
The sample shows several sets of them, organized in groups.

### sidebar.md

`data/sidebar.md` contains whatever you would like shown in the sidebar,
to the right of the organizer panel on your course homepage.

The sample data shown presents a reasonable and consistent portal to D2L.

Note that the "D2L LMS" links all incorporate the `ou` element
form your course metadata. They will only work for students
formally registered in your course, and signed up to D2L.

The "BCIT" links are all generic, and link to different points inside
the main institute website(s).

The "Course Related" links include personalization from your course
metadata, to link to the formal course outline and the student support
channel (Slack assumed) for a specific offering of your course.

### syllabus.md

`data/syllabus.md` is intended to hold your course organizer and schedule,
or as much of that as you wish to share with your students.

It is not "official" ... that comes from the `outline` element inside `course.xml`.

The sample syllabus included here is for COMP4711, one of the courses
using this course hub.

## Course media (in /public)

`public/index.php` is the front controller for the course hub webapp.
It automatically chooses a "development" environment if your site is served
using a virtual domain including the string ".local", or a "production"
environment otherwise. It also sets the folder locations for the application
code and the framework behind it.

`public/.htaccess` is for Apache, remapping incoming requests so they don't need
to include any mention of "index.php" - more SEO-friendly, as it were.
Do not delete it!

The `public/assets` folder contains CSS, Javascript, fints, and images used by the
course hub app, but that are not part of your course data.
Only touch these if you are comfortable doing so, for instance to change
the colour scheme used for your course.

`public/pix` is totally yours to play with, for instance for media you
wish to reference in your generated content, or for downloadable files.

`public/pix/logo.png` holds your 50x50ish course logo, shown top left
in the navbar as branding.

## Course Management

If you are the sole author of your course content, you can keep
everything in the "master" branch of your repo.

If there are other authors or content providers, I suggest
that you have them contribute through pull requests (PRs),
rather than giving them admin rights to your repo.

If you want to prepare content ahead of time, I suggest using
"develop" and "master" branches. You would make all changes on the "develop"
branch, until ready to release to students. At that point, you
would merge "develop" into "master". You could also use feature branches
for groupings of material, so that youcould work on more than one section
at a time.

Something that I do is make all course content inactive at the beggining
of a session, and activate elements as the course unfolds. This lets students
see the planned material for the whole course, but they only have to worry about
the material that you have released at any point in time.

I also create a "release" from the "master" branch, at the end of a session.
I usually name it according to the session, eg. "2017winter". This provides
an archive or snapshot of the course material for each offering.
