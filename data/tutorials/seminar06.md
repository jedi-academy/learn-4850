# Seminar06 - Constraining XML with a Schema, Advanced
ACIT4850 - BCIT - Winter 2019

## Tutorial Goals

This tutorial continues the constraining ofan XML document with a schema.

Today's approach builds on last week's, adding more complex constraints
and restrictions.

### The End Result

I have made a [starter project](/download/seminar06starter.zip)
for you, namely the results of completing last week's seminar.
This was assigned as prep for today; this starter is for those who
didn't complete it.

## And here we go...

### Starting point

Download the starter project, if needed, or continue with
the one you used for seminar 5. 
When you load the page at `localhost:8080`, you should see a to-do list
in your browser:

    TO DO LIST
    Job 1: Comp1234 assignment
    Job 2: Mow the lawn
    Job 3: Wash the car
    Job 4: Paint the fence

The starter I provided for this seminar has a purposeful error
in it, so you can see what handling that looks like.

    { valid: false,
      result: 'WITH_ERRORS',
      messages:
       [ '[error] cvc-complex-type.2.4.a: Invalid content was found starting with element \'stzatus\'. One of \'{deadline, status}\' is expected. (22:18)' ] }

Fix that before continuing, or introduce your own error and fix it once
you see the error message results.

Once corrected, there should be no message in your command line output.

--> Your turn

### Quick review

We have an XML document, `tasks.xml`, bound to a schema.

We have an XML schema, `tasks.xsd`, used to validate the XML document above.

The schema contains both simple elements (is, job, ...) and
complex elements (task, tasks), some of them with "indicators"
(tasks, deadline).

Some of the elements are "typed" (id, job, task).


### Attributes

An element with attributes is, of necessity, a complex element.

Let's make the `id` element into an attribute instead, and a required
one at that.

    <xs:attribute name="id" type="xs:integer" use="required"/>

Note that "id" may not be the best choice of names, as there is an
"ID" type in schema.

We need to fix the XML to reflect the above.

It is not our job to ensure uniqueness of the "id" ... yet.

--> Your turn

### Making our own types

Any simple element can be made into a "type". Consider

    <xs:simpleType name="key">
        <xs:restriction base="xs:integer">
            <xs:maxInclusive value="100"/>
        </xs:restriction>
    </xs:simpleType>      
    
and then the attribute above could be expressed as

    <xs:attribute name="id" type="key" use="required"/>

Do this for things that you want to reuse, or things that
would overly complicate other definitions.

--> Your turn


### What else can we do with restrictions?

The `priority` element contains an integer value.

Assume that the value is meaningful ... 1=top, 2=medium,
3=low, 9=filler. We could use an enumeration restriction 
to constrain that.

    <xs:simpleType name="Tpriority">
        <xs:restriction base="xs:integer">
            <xs:enumeration value="1"/>
            <xs:enumeration value="2"/>
            <xs:enumeration value="3"/>
            <xs:enumeration value="4"/>
        </xs:restriction>
    </xs:simpleType>      
    
The `Tpriority` is just one possible naming convention
to distinguish element names from element types.

Don't forget:

    <xs:element name="priority" type="Tpriority"/>

You can have restrictions based on length, patterns, ranges,
white space, and more. 
The "XSD Data" section in the Wikipedia XML tutorial material
lists the restrictions available for each of the built-in
data types.

Remember: XML contains just string data. A schema lets you
impose restrictions on it.

--> Your turn

## Good & Bad Examples

### Nested vs separated definitions

This is bad:

    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <xs:element name="boroughs">
      <xs:complexType>
        <xs:sequence>
          <xs:element name="borough" maxOccurs="unbounded">
            <xs:complexType mixed="true">
              <xs:sequence>
                <xs:element name="sales" maxOccurs="unbounded">
                  <xs:complexType>
                    <xs:simpleContent>
                      <xs:extension base="xs:integer">
                        <xs:attribute name="class" type="xs:string"/>
                        <xs:attribute name="min" type="xs:integer"/>
                        <xs:attribute name="avg" type="xs:integer"/>
                        <xs:attribute name="max" type="xs:integer"/>  
                      </xs:extension>
                    </xs:simpleContent>
                  </xs:complexType>
                </xs:element>
              </xs:sequence>
              <xs:attribute name="number" type="xs:integer"/>
            </xs:complexType>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
    </xs:element>

    </xs:schema>

This is good:

    <?xml version="1.0"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <!-- Our root element, schedule, contains route & effective nested elements,
    the port and day definition tables, and then any number of sailing elements -->
        <xs:element name="schedule">
            <xs:complexType>
                <xs:sequence>
                    <xs:element name="route" type="xs:string"/>
                    <xs:element name="effective" type="xs:string"/>
                    <xs:element name="ports" type="portsType"/>
                    <xs:element name="days" type="daysType"/>
                    <xs:element name="sailing" type="sailingType" maxOccurs="unbounded"/>
                </xs:sequence>
            </xs:complexType>
        </xs:element>

        <!-- This is our ports table, a list of allowed port codes -->
        <xs:complexType name="portsType">
            <xs:sequence>
                <xs:element name="port" type="portType" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    ...

### DRY

Bad:

    ...
    <xs:element name="boss" type="bossType"/> 
    <xs:element name="supervisor" type="supervisorType"/> 

    <xs:simpleType name="bossType">
      <xs:restriction base="xs:string">
        <xs:maxLength value="8"/>
      </xs:restriction>
    </xs:simpleType>
    <xs:simpleType name="supervisorType">
      <xs:restriction base="xs:string">
        <xs:maxLength value="8"/>
      </xs:restriction>
    </xs:simpleType>

Good:

    ...
    <xs:element name="boss" type="employeeType"/> 
    <xs:element name="supervisor" type="employeeType"/> 

    <xs:simpleType name="employeeType">
      <xs:restriction base="xs:string">
        <xs:maxLength value="8"/>
      </xs:restriction>
    </xs:simpleType>

## Plan ahead

Good:

    <xs:element name="employee"> 
      <xs:complexType> 
        <xs:sequence>
          <xs:element name="firstname"
                     type="xs:string"/> 
          <xs:element name="lastname"
                     type="xs:string"/> 
        </xs:sequence> 
      </xs:complexType> 
    </xs:element> 

Better:

    <xs:complexType name="personinfo"> 
      <xs:sequence>
        <xs:element name="firstname"
                                   type="xs:string"/> 
        <xs:element name="lastname"
                                   type="xs:string"/> 
      </xs:sequence> 
    </xs:complexType> 

    <xs:element name="employee" type="personinfo"/>

### Use appropriate naming

Bad:

    <xs:element name="product">
      <xs:complexType>
       <xs:attribute name="target" type="xs:string"/>
      </xs:complexType>
    </xs:element>

    <xs:element name="target" type="xs:int"/>

Good:

    <xs:element name="product">
      <xs:complexType>
       <xs:attribute name="goal" type="xs:string"/>
      </xs:complexType>
    </xs:element>

    <xs:element name="target" type="xs:int"/>

## Onwards

### Best practices

- Create "types" for anything complex.
- Comment your schema! (see the examples)
- Work top-down or bottom-up, but consistently.
- Publish schemas online if you want others to conform.

### Other resources?

- You can see some more elaborate examples in the [Jedi Academy](https://github.com/jedi-academy/example-schema-winter2016)
- A [lecture](/download/ACIT4850-XML-Schemas-v2.4.ppt) from that same time period that you might find handy as a reference


### What have we learned?

- rich way to constrain the structure and content of an XML document,
- nodejs support for XML, DTDs & XSD hasn't improved since last week
