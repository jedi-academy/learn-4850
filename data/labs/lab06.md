# Lab06 - XSL Presentation of Your Sushi Menu
ACIT4850 - BCIT - Winter 2019

## Lab Goals

This lab applies the techniques from seminar 10, to build an XSL transformation
for your sushi menu XML document.

## Lab Submission

This is an individual lab.

A marking rubric has been attached to the dropbox.

Submit a compressed copy of your project folder (zip or tarball) to the appropriate D2L dropbox.

Due: Sunday, Mar 24, 17:30 PST

## And ... Here We Go

Your XSL should produce a well-formed HTML page, along the lines of

- category
    - item
    - item
    - item
- category
    - item
    - ...

Further details:
- Use heading 2 elements for the categories.
- Show each menu item's description bolded
- Show each menu item's price right-aligned
- You may use a table or CSS for alignment
- The root template should contain the basic HTML document structure
and menu/restaurant name
- Use a category template to transform each category
- Use a for-each construct to iterate over the items in a category
- Use a template for a menu item
- Order your report by item name within category

I will not be running your node app - only looking at the XML & XSL.

If you want to keep multiple XML documents in your project, the only one I will
look at for grading is **sushimenu.xml**.
If you want to keep multiple XSL documents in your project, the only
one I will look at for grading is **sushimenu.xsl**.

**Ask for advice when (if?) you get stuck!**