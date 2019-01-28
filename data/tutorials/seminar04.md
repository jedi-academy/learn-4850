# Seminar04 - Constraining XML with a DTD
ACIT4850 - BCIT - Winter 2019

## Tutorial Goals

This tutorial will walk you through constraining an XML document with a DTD.

There are several smaller steps to the tutorial. 
Nothing has to be handed in for this tutorial - that is coming up as lab3.

### The End Result

Our Node app from last week will expect an XML document bound to a DTD,
and will validate it in addition to anything else it does, with the
validation results shown on the console.

## And here we go...

### DTD goals

- valid XML (not just well-formed)
- external (shared) DTD
- DOCTYPE directive inside each XML

--> Your turn

### Elements

- ELEMENT directive
- <!ELEMENT name (components)> **or**
- <!ELEMENT name (#PCDATA)>

### Element order

- specific order ... separated by commas
- any order ... separated by vertical bars
- use parentheses for grouping

--> Your turn

### Element multiplicity

- optional (?)
- zero or more (*)
- one or more (+)

--> Your turn

### Element content

- none (EMPTY)
- some (#PCDATA)
- "any" (ANY)
- "mixed" (#PCDATA|...)

--> Your turn

### Attributes

- ATTLIST directive
- <!ATTLIST element name1 type1 mod1 name2 type2 mod2 ...>

- types: CDATA or enumeration (no spaces)
- special types (NMTOKEN, ID, IDREF)
- modifiers: #IM{IED, #REQUIRED, default


--> your turn

### Validation

- in your IDE
- https://www.npmjs.com/package/node-libxml
- https://validator.w3.org/

 --> your turn

### Best practices

- Create your DTD, for what **you** want your data to look like.
- Comment your DTD!
- Build the XML to conform to it.
- Publish DTDs online if you want others to conform.

### What have we learned?

- simple way to constraint the structure of an XML document,
and *some* of the data content (attributes)
- easy to validate :)
- not rich enough for most purposes :(
