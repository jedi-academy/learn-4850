# Seminar03 - Adding to an XML document
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

## Reality Check

At this point, your seminar 3 app should look something like the following.
It isn't pretty, but will make sense if you have been following along
during the seminar.

    // ACIT4850 - seminar 3 outcome

    var http = require('http'), fs = require('fs'), xmldom = require('xmldom');
    http.createServer(function (req, res) {
        data = fs.readFileSync('./data/order4.xml')
        parser = new xmldom.DOMParser();
        xmldoc = parser.parseFromString(data.toString(), 'text/xml');
        xmlroot = xmldoc.documentElement;
        res.writeHead(200, {'Content-Type': 'text/html'});
        heading = '<h1>Greasy Spoon Order for ' + xmlroot.getElementsByTagName('customer')[0] + '</h1>';
        res.write(heading);

        // add to the order
        addBurger();

        // save our document
        serializer = new xmldom.XMLSerializer();
        tosave = serializer.serializeToString(xmldoc);
        fs.writeFileSync('./data/neworder.xml',tosave);

        result = '';
        x = xmlroot.childNodes;
        for (i = 0; i < x.length; i++) {
            if (x[i].nodeName == 'burger') {
                result += showBurger(x[i]);
            }
        }
        res.write(result);

        res.end();


    }).listen(8080);

    function showBurger(burger) {
        which = burger.getElementsByTagName('patty')[0].getAttribute('type');
        result = '<h2>' + which + " burger</h2>";
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

        return result;
    }

    function addBurger() {
        newburger = xmldoc.createElement('burger');

        patty = xmldoc.createElement('patty');
        patty.setAttribute('type','tauntaun');
        newburger.appendChild(patty);

        cheeses = xmldoc.createElement('cheeses');
        cheeses.setAttribute('top','two blue moon');
        newburger.appendChild(cheeses);

        name = xmldoc.createElement('name');
        name.textContent = 'In class special';

        newburger.appendChild(name);
        xmlroot.appendChild(newburger);
    }

### Make it look a bit better

The http.createServer() block is pretty busy, and it will
only get worse when we incorporate the CSV processing.

Let's extract the saving code into its own function...

    // save our document
    function saveData() {
        serializer = new xmldom.XMLSerializer();
        tosave = serializer.serializeToString(xmldoc);
        fs.writeFileSync('./data/neworder.xml', tosave);
    }

And also the order display...

    // display the XML document
    function displayOrder() {
        let result = '';
        let x = xmlroot.childNodes;
        for (i = 0; i < x.length; i++) {
            if (x[i].nodeName == 'burger') {
                result += showBurger(x[i]);
            }
        }
        return result;
    }

And the core of our createServer() looks a bit simpler:

        ...
        // add to the order
        addBurger();

        saveData(); // save our document
        res.write(displayOrder());
        res.end();
    }).listen(8080);


### Preparing for CSV

Use the [Node CSV](https://www.npmjs.com/package/csv) package.

    npm install csv

We will use the "csv-parse" package inside that, following
the first [API usage example](https://csv.js.org/parse/api/).

We will ignore the first line in the CSV file, as it provides column
headings and not real data.

    "burger","patty","cheeses","toppings","sauces"
    "Luke","tauntaun","blue","jalapeno peppers, banana peppers","chipotle mayo"

Let's define some constants for the CSV data column positions,
so that we have only one line to change if the data format
changes down the road.

    const BURGER=0, PATTY=1, CHEESES=2, TOPPINGS=3, SAUCES=4;

### CSV Processing Block

Finally, we are ready to apply the example from "csv-parse".
We need a block of code that handles each data row in the CSV file.

    var csv = require('csv-parse');
    csvdata = fs.readFileSync('./data/another burger.csv');
    csv(csvdata, {trim: true, skip_empty_lines: true, from_line: 2})
            .on('readable', function () {
                let record;
                while (record = this.read()) {
                    //addNewBurger(record);        
                }
            })
            .on('end', function () {
                saveData(); // save our document
                res.write(displayOrder());
                res.end();
            });

This is derived from the API example, and doesn't look toooo bad.

### Add a row of CSV data

We just need a different function to add a new burger with a row
of CSV data, instead of the hard-coded version we used earlier
this seminar.

    // add a new burger, with data coming from a row of CSV data
    function addNewBurger(record) {
        newburger = xmldoc.createElement('burger');

        patty = xmldoc.createElement('patty');
        patty.setAttribute('type', record[PATTY]);
        newburger.appendChild(patty);

        topping1 = xmldoc.createElement('toppping');
        topping1.setAttribute('type', record[TOPPINGS]);
        newburger.appendChild(topping1);

    // save the name of the burger as "instructions", since that is displayed already
        instructions = xmldoc.createElement('instructions');
        instructions.textContent = record[BURGER];
        newburger.appendChild(instructions);

        xmlroot.appendChild(newburger);
    }

This isn't perfect or complete, but you should get the idea :)

Looking at the HTML output in the browser, the burger name ("Luke") isn't
shown, but that is a simple matter of revising the showBurger() function,
which I am leaving as an exercise for the reader ;P

### What have we learned?

- clunky data creation :(
- easy to save :)
