# Seminar09 - Identity constraints and annotations
ACIT4850 - BCIT - Winter 2019

## Identity Constraints

### xs:ID for Unique Identifiers in a Document

`xs:ID` lets you specify a field which is required, and which
implies a uniqueness validation constraint within a namespace,
i.e. a document.

A "field" can be either an element or an attribute.
You can maintain compatibility with the ID attribute type in a DTD
by using `xs:ID` for attributes only.

`xs:IDREF` is a schema element defining a field in the target
document which is required, and which must hold one of the
`ID` values implied by the document.

You may have more than one `ID` field in a document, but values
need to be unique across all of them.

A sample `book` element from a `library` document:

    <book identifier="isbn-0836217462">
      <isbn>
        0836217462
      </isbn>
      <title>
        Being a Dog Is a Full-Time Job
      </title>
      <author-ref ref="au-Charles_M._Schulz"/>
      <character-refs>
        ch-Peppermint_Patty ch-Snoopy ch-Schroeder ch-Lucy
      </character-refs>
    </book>

A schema excerpt showing ID and IDREF definitions for the above.

    <xs:element name="book">
      <xs:complexType>
        <xs:sequence>
          <xs:element name="isbn" type="xs:NMTOKEN"/>
          <xs:element name="title" type="xs:string"/>
          <xs:element name="author-ref">
            <xs:complexType>
              <xs:attribute name="ref" type="xs:IDREF" use="required"/>
            </xs:complexType>
          </xs:element>
          <xs:element name="character-refs" type="xs:IDREFS"/>
        </xs:sequence>
        <xs:attribute name="identifier" type="xs:ID" use="required"/>
      </xs:complexType>
    </xs:element>

Note the naming convention (isbn-..., au-..., ch-...) used to
avoid identifier value collisions. The example is taken from
_Definitive XML Schema_.

### xs:unique for Unique Values in an Element

`xs:unique` lets you specify an optional field which, if present,
implies a uniqueness validation constraint within a document element.

A "field" can be either an element or an attribute.
This is not the same kind of thing as the ID attribute type in a DTD,

`xs:keyref` is a schema element defining a field in the target
document which is optional, but which can refer to a "unique" element
or attribute elsewhere in the document.

You may have more than one `unique` field in a document, and their values
only need to be unique within their containing element.

A sample `book` element from a `library` document:

    <book identifier="isbn-0836217462">
      <isbn>
        0836217462
      </isbn>
      <title>
        Being a Dog Is a Full-Time Job
      </title>
      <authors>
         <author code="CMS">Charles_M._Schulz</author>
         <author>Snoopy</author>
      </authors>
      <character-refs>
        ch-Peppermint_Patty ch-Snoopy ch-Schroeder ch-Lucy
      </character-refs>
    </book>

A schema excerpt requiring unique author `code` attributes id they are used.

    <xs:element name="authors">
      <xs:complexType>
        <xs:sequence>
          <xs:element name="author" maxOccurs="unbounded">
            <xs:complexType>
              <xs:attribute name="code" type="xs:keyref" use="optional"/>
            </xs:complexType>
          </xs:element>
         </xs:sequence>
         <xs:unique name="code" type="string"/>
      </xs:complexType>  
    </xs:element>


### xs:key for Unique Identifiers in an Element

`xs:key` lets you specify a required non-null field which
implies a uniqueness validation constraint within a document element.

A "field" can be either an element or an attribute.
This is similar to the ID attribute type in a DTD,

`xs:keyref` is a schema element defining a required field in the target
document which refers to a "key" element
or attribute elsewhere in the document.

You may have more than one `key` field in a document, and their values
only need to be unique within their containing element.

A sample `book` element from a `library` document:

    <book identifier="isbn-0836217462">
      <isbn>
        0836217462
      </isbn>
      <title>
        Being a Dog Is a Full-Time Job
      </title>
      <authors>
         <author code="CMS">Charles_M._Schulz</author>
         <author code="S">Snoopy</author>
      </authors>
      <character-refs>
        ch-Peppermint_Patty ch-Snoopy ch-Schroeder ch-Lucy
      </character-refs>
    </book>

A schema excerpt requiring unique author `code` attributes id they are used.

    <xs:element name="authors">
      <xs:complexType>
        <xs:sequence>
          <xs:element name="author" maxOccurs="unbounded">
            <xs:complexType>
              <xs:attribute name="code" type="xs:keyref" use="optional"/>
            </xs:complexType>
          </xs:element>
         </xs:sequence>
         <xs:key name="code">
           <xs:selector xpath=//book//author"/>
           <xs:field xpath="@code"/>
         </xs:key>
      </xs:complexType>  
    </xs:element>

### Pulling these together...

If you had a document like

    <orders>
      <order id="123245">
        <customer>blah, blah, blah</customer>
        <item no="1">
          <product sku="abc-333"><qty>2</qty><price>1.23</price></product>
        </item>
        <item no="2">
          <labor><desc>Polish it</desc><qty>1.0</qty><rate>40.00</rate></labor>
        </item>
      </order>
      <order id="123246">
        <customer>blah, blah, blah</customer>
        <item no="1">
          <product sku="mac-big"><qty>1</qty><price>4.99</price></product>
        </item>
        <item no="2">
          <labor><desc>Wrap it</desc><qty>1.0</qty><rate>0.00</rate></labor>
        </item>
      </order>
    </orders>

Then you might use:

- an `xs:ID` constraint for the `id` attribute of an `order`
- an `xs:keyref` constraint for the `no` attribute of `item` elements in an `order`
- an `xs:keyref` constraint for the `sku` attribute of `product` elements inside an `item`

- the `id` namespace would probably be defined inside the `order` schema definition.
- the `xs:unique` for `sku` would probably be defined in a separate schema/document section for procude codes
- the `xs:key` got `item` would probably be defined inside `order`

## Shall we try it?

Let's pick a sushi menu XML & XSD (which could be your own), and spend a few minutes applying these.

--> Your turn. Apply this to a field or two in the sushi menu schema.

## Annotations


An `xs:annotation` element in a schema provides "documentation" for the element it is
contained inside.

It can hold an optional `xs:appinfo` element, referring to an authoritative resource, with
an optional `source` attribute.

It can hold an optional `xs:documentation` element, describing the element in question,
also with an optional `source` attribute.

Use this instead of an XML comment, `<!-- ... -->`, to document your schema.

Bind your schema to anappropriate stylesheet, and the schema documentation
will be rendered by opening the schema inside your browser.

    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="Annotation.xsl"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    ...

--> Your turn. Apply this to a field or two in the sushi menu schema.


## Want to Find Out More?

Prentice-Hall has a sample chapter from the _Definitive XML Schema_ book
online: [Chapter 17: Identity constraints](http://www.datypic.com/books/defxmlschema/chapter17.html).
All of the [examples](http://www.datypic.com/books/defxmlschema/dxsexamples.zip) 
from the book can be downloaded as well.

O'Reilly's _XML Schema_  is online too, with [Chapter 9: Defining Uniqueness...]
(https://docstore.mik.ua/orelly/xml/schema/ch09_01.htm) relevant to today's seminar.

There are some explanations/examples of annotations online from 
[Oxygen](https://www.oxygenxml.com/doc/versions/21.0/ug-editor/topics/content-completion-schema-annotations.html) and
[IBM](https://www.ibm.com/support/knowledgecenter/en/SSQP76_8.9.0/com.ibm.odm.itoa.develop/modeling_topics/ref_example_xsd.html).


The `annotation` & `key` schema elements are mentioned in the 
[W3Schools schema reference](https://www.w3schools.com/xml/schema_elements_ref.asp),
but `ID` isn't.

## What have we learned?

- we can constrain uniqueness in an XML document :)
- there is a convention for documenting schemas :)
