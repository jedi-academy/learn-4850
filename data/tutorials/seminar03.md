# Seminar02 - Adding to an XML document
ACIT4850 - BCIT - Winter 2019

## Tutorial Goals

This tutorial will walk you through building a Node app to work with an XML document.

I have provided some updated ["order" data](/download/seminar03data.zip) to work with.

There are several smaller steps to the tutorial. 
Nothing has to be handed in for this tutorial - that is coming up as lab3.

### Planning

This builds on your seminar from last week.
I trust that you have Node v10.15.0 installed, and an editor or IDE to work with.

I recommend opening the [XMLDOM API page](https://www.npmjs.com/package/xmldom) 
in a separate browser tab, as we will refer to it several times.

### The End Result

We will end up with a Node app which loads an order XML document into a DOM,
reads a CSV file and adds the data from it to the DOM, and saves
the result. Of course, we will present the order when done :)

I will share my final project on the course hub.

## And here we go...

### Project setup

Start with your project from last week.
You could rename last week's folder, or copy its
contents into a new project forlder for this week.

That will give you an app that reads & presents an XML order.

### Saving XML

Let's make sure that we can save stuff.

We can use an XMLSerializer to make a string from an XML document.

    // save our document
    serializer = new xmldom.XMLSerializer();
    tosave = serializer.serializeToString(xmldoc);
    console.log(tosave);    // what does this look like?

    // save it
    fs.writeFileSync('./data/neworder.xml',tosave);

What happens if we serialize the document root instead?

-> YOUR TURN

### How do work with this?

- "Document" API
- "Node" API
- "Element" API
- "XMLSerializer"?

### Adding elements

Node methods are used to add new elements.

We want to add a burger to the order.

    // add to the order
    addBurger();
    
    function addBurger() {
        newburger = xmldoc.createElement('burger');
        xmlroot.appendChild(newburger);
    }


But, we need the nested elements inside that too!

    cheeses = xmldoc.createElement('cheeses');
    cheeses.setAttribute('top','two blue moon');
    newburger.appendChild(cheeses);

Wow. This is clunky!

What about something with a value?

    name = xmldoc.createElement('name');
    name.textContent = 'In class special';
 

-> YOUR TURN

### Reading from CSV

Use the [Node CSV](https://www.npmjs.com/package/csv) package.



### What have we learned?

- clunky data creation :(
- easy to save :)
