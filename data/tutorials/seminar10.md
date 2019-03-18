# Seminar10 - XSL basics
ACIT4850 - BCIT - Winter 2019

## Resources

Download the XSL examples repo linked to in the organizer.
We wil be using the CD catalog as examples for this.

## Bind your XML to an XSL

    <?xml-stylesheet type='text/xsl' href='?.xsl'?>

Note: this would normally be done sdynamically, and server-side

## Start with an empty XSL

    <?xml version="1.0" encoding="ISO-8859-1"?>
    <xsl:stylesheet version="1.0"
        xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    </xsl:stylesheet>

## Specify the transformation target

    <xsl:output method='html' version='1.0' encoding='UTF-8' indent='yes'/>

## Add a root template

    <xsl:template match="/">
      <html>
      <body>
      </body>
      </html>
    </xsl:template>

## Apply templates

    <xsl:apply-templates/> 
 
## Include XML values

    <xsl:value-of select="?"/>

## Loop over node sets

     <xsl:for-each select="?">
        ...
     </xsl:for-each>

## Order templates

     <xsl:sort select="?"/> 

## Want to Find Out More?

Additional tutorial material available online:
- [w3Schools](https://www.w3schools.com/xml/xsl_intro.asp)
- [EduTech WIKI](http://edutechwiki.unige.ch/en/XSLT_Tutorial_-_Basics)

## What have we learned?

- XML can be transformed (extracted, mangled & reconstituted) into
various forms, like HTML or even different XML