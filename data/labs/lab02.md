# Lab02 - Display Your Sushi Menu
ACIT4850 - BCIT - Winter 2019

## Lab Goals

This lab applies the techniques from seminar 02, to build an HTML presentation
of your menu XML document.

Check with me before making any changes to your XML!

## Lab Submission

This is an individual lab.

A marking rubric is attached to the dropbox.

Submit a compressed copy of your project folder (zip or tarball) to the appropriate D2L dropbox.

Due: Sunday, Jan 20, 17:30 PST

## And ... Here We Go

Build a Node app to load and process your sushi menu XML document,
producing an HTML page with

- *h1* identifying the restaurant and the menu creator (eg Menu for Jim's Greasy Spoon, by Donald Duck)
- *h2* for each menu category
- *ul* to present the items in that category; each showing the menu item and price, bolded,
    with supporting details on the line below
- show "vegetarian" or "spicy" icons beside the menu item if appropriate, i.e. between the name and its price

Anticipated questions:

- Don't worry about styling - not our problem. No bonus marks for HTML page style
- You can incorporate icons for other things that are incorporated into your data (eg gluten-free),
    but don't have to.
- If a menu item has variations, you might choose a nested unordered list to handle those.
- If you wish to use Express, go for it
- I will test your app using the browser location "localhost:8080"
- Yes, JS code readability counts
- Keep your XML data in a "data" subfolder in your project
- You may modify your XML data from lab 1, but check with me first
- Good practice would be to include restaurant metadata as part of the XML,
    and your name (eg for headings) as a constant in your JS; i.e. avoid
    hard-coding stuff
- Comments improve readability, though well-structured and named code may not need them
- If your data does not include any vegetarian or spicy items, you can add some, or
    modify the data to introduce some, or not. If the submitted data does not have any,
    your code still needs to handle them, and you will need to make it clear how I
    can modify your data to trigger the icons in the output
