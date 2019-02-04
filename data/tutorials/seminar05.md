# Seminar05 - Constraining XML with a Schema
ACIT4850 - BCIT - Winter 2019

## Tutorial Goals

This tutorial will walk you through constraining an XML document with a schema,
also known as an "XSD" because of the filename extension used.

There are several smaller steps to the tutorial. 
Nothing has to be handed in for this.

Today's approach is a shallow one, touching on many but not all the aspects
of an XML schema. We will take a deeper dive into this next week.

### Want to Know More?

- [W3Schools](https://www.w3schools.com/xml/schema_intro.asp) has an easy to read
tutorial. The first section in it is what I asked you to read in preparation
for this week's class. The second and third sections (XSD Complex and XSD Data, respectively)
would be useful to help with next week's material.
- [EduTechWiki](http://edutechwiki.unige.ch/en/XML_Schema_tutorial_-_Basics#Introduction) 
has a comprehensive single-page XML schema tutorial.
It isn't as easy to read as the W3Schools material, but it is complete,
and all on one page.

### The End Result

We will use a different document this week, to suit 
our shallow exploration of XML schema.
I have made a [starter project](/download/seminar05starter.zip)
for you.

Our Node app will provide a summary list of todo tasks, but its main purpose
is to validate, server-side, the tasks data constrained by a schema.
We will come back to our ordering data next week :)


## And here we go...

### Starting point

Download the starter project, and get it running on your system.
When you load the page at `localhost:8080`, you should see a to-do list
in your browser:

    TO DO LIST
    Job 1: Comp1234 assignment
    Job 2: Mow the lawn
    Job 3: Wash the car
    Job 4: Paint the fence

and you should see `Done.` in your console output.

--> Your turn

### Start our schema

Let's name our schema with the same root filename as the data,
so `data/tasks.xsd`. Create this, with an `<xs:schema>` element
"bound" to the `xs` namespace.

    <?xml version="1.0"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    </xs:schema>

Validate this inside your IDE.

--> Your turn

### Document root

`tasks` is the root element of our document.

    <xs:element name="tasks">        
    </xs:element>

It is considered "complex", because it has nested elements.

    <xs:element name="tasks">        
        <xs:complexType></xs:complexType>      
    </xs:element>

specifically `task` elements.

    <xs:element name="tasks">  
        <xs:complexType>
            <xs:sequence>
                <xs:element name="task"/>
            </xs:sequence>
        </xs:complexType>      
    </xs:element>

And there can be any number of them.

    <xs:element name="tasks">  
        <xs:complexType>
            <xs:sequence>
                <xs:element name="task" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>      
    </xs:element>

Your schema should validate at each stage along the way.

--> Your turn


### What is a Task?

A task, so far, is vague. In reality, it is also
a complex element, containing a number of subelements.

We can define it separately

    <xs:element name="task">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="id"/>
                <xs:element name="job"/>
                <xs:element name="priority"/>
            </xs:sequence>
        </xs:complexType>      
    </xs:element>

and then refer to it.

    <xs:element name="tasks">  
        <xs:complexType>
            <xs:sequence>
                <xs:element ref="task" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>      
    </xs:element>

--> Your turn

### Binding our XML to our Schema

Validating the (incomplete) schema is useful,
but we also want to make sure that our XML document conforms to it.

Bind your XML document to a schema by specifying appropriate
attributes of your XML document's root element.

    <tasks
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="tasks.xsd"
    >

Note the "noNamespaceSchemaLocation", which is simpler than
what W3Schools suggests.

Once you have done this, you can try to validate your XML
document against the schema, inside your IDE.

It won't validate, because we have not specified all the nested elements
for a `task`.

    XML validation started.
    Checking file:/home/bcit/winter2019/acit4850/seminar05/data/tasks.xml...
    Referenced entity at "file:/home/bcit/winter2019/acit4850/seminar05/data/tasks.xsd".
    cvc-complex-type.2.4.d: Invalid content was found starting with element 'size'. No child element is expected at this point. [10] 
    cvc-complex-type.2.4.d: Invalid content was found starting with element 'size'. No child element is expected at this point. [20] 
    cvc-complex-type.2.4.d: Invalid content was found starting with element 'size'. No child element is expected at this point. [30] 
    cvc-complex-type.2.4.d: Invalid content was found starting with element 'size'. No child element is expected at this point. [40] 
    XML validation finished.

I bet you have an idea how to fix that, eh?

--> Your turn

### Add some basic typing

The schema we have come up is legal, but not so useful.

Add type definitions to the elements in a task...

    <xs:element name="id" type="xs:integer"/>
    <xs:element name="job" type="xs:string"/>


--> Your turn

### Attributes

What happens when we specify that the `deadline` element should
be a date?

We can fix the one date format...

        <deadline>2017-02-19</deadline>

What about the other empty `deadline` elements?

Remove them in the XML, and specify that the element is optional in the XSD.

        <xs:element name="deadline" type="xs:date" minOccurs="0"/>

Without further information about the `status` element, we should not try to type it
after all.

--> your turn

### Validation in your code


That's far enough for the data and schema. We will go into more details
next week.

We do, however, want to do the validation in code.

I found an npm module, [xsd-schema-validator](https://www.npmjs.com/package/@passify/xsd-schema-validator), 
that does the trick, albeit clunkily.

Install it & then add a validating block of code to your app, for
instance before attempting to build a DOM for your document.

    var validator = require('xsd-schema-validator');
    validator.validateXML(data, 'data/tasks.xsd', function(err, result) {
      if (err) {
        console.log(result);
      }
      result.valid; // true
    });    

 --> your turn

### Best practices

- Create "types" for anything complex.
- Comment your schema! (see the examples)
- Work top-down or bottom-up, but consistently.
- Publish schemas online if you want others to conform.

### What have we learned?

- somewhat more complex way to constrain the structure 
and content of an XML document,
- easy to validate? :)
- getting better, with data typing, but still room for improvement :-/
- nodejs support for XML, DTDs & XSD is ... underwhelming
