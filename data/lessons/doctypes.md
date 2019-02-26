# Week 8 - Notes on Handling DOCTYPES
ACIT4850 - BCIT - Winter 2019

## Summary

XMLDOM and/or XMLSerializer do not seem to handle DOCTYPES properly.

Another nail in their coffin, as it were.

### Observed Behaviour

If an XML document has a DOCTYPE directive, like mymenu.xml,
and you parse it into a document and then modify it,
when you save it using XMLSerializer, the new XML
document still has a DOCTYPE directive, but it says that
the name of the DTD is ">".

Ouch.

### DOM in the browser

The browser Javascript engines support `DOMImplementation.createDocumentType()`,
which returns a `DocumentType` object that can be passed to
`DOMImplementation.createDocument`.

As long as you create a new DOM this way, the DOCTYPE should be preserved.

Why isn't this part of the server-side JS DOM implementation?
Good question.

### Any other npm packages affected?

It appears so. I found a number of issues online that appear to be similar.

I did find a suggestion that `xml2js`, if invoked with the proper
undocumented incantation, can produce a doctype.
That doesn't help us.

I also found suggestions that the problem lies in `xmldom` and its parsing of
XML.

### The End Result

Not only can we not validate XML documents against a DTD in node, but
we do not appear to be able to preserve existing doctypes :(

I would not hold our breath waiting for this to be solved.

It is a good thing that our goal is schema-based validation!

I deducted marks for a few submissions that had ">" as the doctype
of a newmenu.xml (or equivalent) file. I will retract those.

### What have we learned?

- avoid reliance on DTDs in a nodejs app
