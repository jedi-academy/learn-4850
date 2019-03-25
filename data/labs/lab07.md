# Lab07 - XPath Enhancement of Your Sushi Menu Presentation
ACIT4850 - BCIT - Winter 2019

## Lab Goals

This lab applies the techniques from seminar 11, to build a richer XSL transformation
for your sushi menu XML document.

## Lab Submission

This is an individual lab.

A marking rubric has been attached to the dropbox.

Submit a compressed copy of your project folder (zip or tarball) to the appropriate D2L dropbox.

Due: Sunday, Mar 31, 17:30 PST

## And ... Here We Go

Enhance last week's XSL, adding the following:

- on the category heading line, add the number of items and average price, inside parentheses (count, avg)
- on an item line, add the green vegetation leaf if appropriate (xsl:if)
- on the category heading line, add a small happy face icon to the right of the
  category name if that category includes any vegetarian items(xsl-if with fancy xpath)
- at the bottom of your HTML page, add a new section, like the following, with the hash
  signs replaced by your results (dateTime, variables)
    - Prices accurate as of ###
    - Typical cost for a typical family: ###

A typical family has two adults and two children. The first adult would buy 1 item from the 
first category, the second adult one from the second. Each child would
buy one item from the kids' menu. Use the average price of these categories.

In other words, the typical cost for a family will be 

    the average item price of the first category
    plus the average item price of the second categpry, 
    plus two times the average item price from the kids' category.

I will not be running your node app - only looking at the XML & XSL.

If you want to keep multiple XML documents in your project, the only one I will
look at for grading is **sushimenu.xml**.
If you want to keep multiple XSL documents in your project, the only
one I will look at for grading is **sushimenu.xsl**.

**Ask for advice when (if?) you get stuck!**