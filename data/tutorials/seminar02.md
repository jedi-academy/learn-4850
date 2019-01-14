# Seminar02 - Working With XML
ACIT4850 - BCIT - Winter 2019

## Tutorial Goals

This tutorial will walk you through building a Node app to work with an XML document.

I have provided some ["order" data](/download/seminar02data.zip) to work with.

There are several smaller steps to the tutorial. 
Nothing has to be handed in for this - that is coming up as lab2.

### Planning

Make sure you have Node installed, and an editor or IDE to work with your
project. I have assumed Node.js v10.15.0 (LTS).

### The End Result

We will end up with a Node app which reads and processes an order XML document,
producing an HTML page to display it.

I will share my final project on the course hub.

## And here we go...

### Create a new Node project

I suggest a meaningful name, like "seminar02".

### Make a simple server

We can use the [HTTP](https://nodejs.org/dist/latest-v10.x/docs/api/http.html) module built into Node.

    var http = require('http');
    http.createServer(function (req, res) {
        res.write('Hello world');
        res.end(); //end the response
    }).listen(8080); //the server object listens on port 8080 

-> YOUR TURN

### Retrieve our XML document

    var fs = require('fs');
    ...
    data = fs.readFileSync('./data/order1.xml')

What does it look like?

-> YOUR TURN

### Choose a DOM module

What do we use?? [npm](https://www.npmjs.com/search?q=xml) has almost 3000 "xml" packages.

Trying to search by popularity doesn't help muc, though "xml2js" looks promising.

It is problematic, however popular.

Restricting the search to "xmldom", there are over 40 packages.

I investigated, and chose [xmldom](https://www.npmjs.com/package/xmldom) as the most useful.

Install it, and add it to your project.

    var xmldom = require('xmldom');

### Build our DOM

Using the module page as a guide, but getting the data from our document...

    parser = new xmldom.DOMParser();
    xmldoc = parser.parseFromString(data.toString(), 'text/xml');

What does that look like?

    console.log(xmldoc);

-> YOUR TURN

### Let's use the document root instead

    root = xmldoc.documentElement;
    console.log(root);

Hmmm - this really needs to be interpreted!

-> YOUR TURN

### Switch to HTML output

    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write('<p>Hi there</p>');
    res.end();

-> YOUR TURN

### How about a heading?

    res.write('<h1>Greasy Spoon</h1>');''

### Tailor the heading from our data

We want the value of the "customer" element in our data.

    heading = '<h1>Greasy Spoon Order for ' + root.getElementsByTagName('customer')[0] + '</h1>';
    res.write(heading);

-> YOUR TURN

### What exactly is in our root element?

We can use the "childNodes" property...

    result = '';
    x = root.childNodes;
    for (i = 0; i < x.length; i++) {
        result += x[i].nodeName + "<br>";
    }
    res.write(result);

-> YOUR TURN

### We want all the burgers

We could do that in a loop...

    for (i = 0; i < x.length; i++) {
        if (x[i].nodeName == 'burger') {
            result += x[i].nodeName + "<br>";
        }
    }

-> YOUR TURN

### Show basic burger info

Let's extract some burger basics

    which = x[i].getElementsByTagName('patty')[0].getAttribute('type');           
    result += '<h2>' + which + " burger<br>";

Let's break down that mouthful!

-> YOUR TURN

### Move things to a function for better readability

    result += showBurger(x[i]);
    ...
    function showBurger(burger) {
        which = burger.getElementsByTagName('patty')[0].getAttribute('type');
        result = '<h2>' + which + " burger</h2>";
        return result;
    }

### Add the supplementary info

Use a switch construct to handle whitespace more easily

    cheese = '';
    toppings = '';
    sauces = '';
    choices = burger.childNodes;
    for (cc = 0; cc < choices.length; cc++) {
        switch (choices[cc].nodeName) {
            case('cheeses'):
                cheese += choices[cc].getAttribute('top') + ' ' + choices[cc].getAttribute('bottom');
                continue;
            case('topping'):
                toppings += choices[cc].getAttribute('type') + ', ';
                continue;
            case('sauce'):
                sauces += choices[cc].getAttribute('type') + ', ';
                continue;
        }
    }
    result += '<p>' +
            'Cheeses: ' + cheese + '<br>' +
            'Toppings: ' + toppings + '<br>' +
            'Sauces: ' + sauces +
            '</p>';


-> YOUR TURN

### Will this work for our other order?

    ...
    data = fs.readFileSync('./data/order4.xml')
    ...

Missing the delivery instructions, but we know how to handle that.

-> YOUR TURN

### What have we learned?

- wild west
- clunky traversal
- clunky data extraction
- transferable skills, though
