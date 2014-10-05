---
layout: post
title:  "XSLT Tutorial"
date:   2012-07-06 00:00:00
categories: Web
---
With XSLT (Extensible Stylesheet Language Transformations) you can transform a xml file into any desired output. You can generate html, text or even sourcecode.

I wrote a small tutorial to show you how it works.

The following diagramm shows you how the XSLT-process works:

![image](http://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/XSLT_en.svg/220px-XSLT_en.svg.png)

Let’s start with a simple example. We’ll try to generate some html code to make the xml-data more readable. I run [Saxon 9 HE](http://saxon.sourceforge.net/#F9.4HE) as processor.

The XML input:

	<?xml version="1.0" ?>
	<users>
	    <user>
	        <name>Patrick</name>
	        <image>patrick.png</image>
	    </user>
	    <user>
	        <name>Paul</name>
	        <image>paul.png</image>
	    </user>
	</users>
	
The XSLT stylesheet:

	<?xml version="1.0" encoding="UTF-8"?>
	<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
	    <xsl:output method="xml" indent="yes" encoding="UTF-8"/>
	    <xsl:template match="/users">
	        <html>
	            <head>
	                <title>XSLT Tutorial</title>
	            </head>
	            <body>
	                <xsl:apply-templates select="user" />
	            </body>
	        </html>
	    </xsl:template>
	      
	    <xsl:template match="user">
	        <img>
	            <xsl:attribute name="src">
	                <xsl:value-of select="image" />
	            </xsl:attribute>
	        </img>
	        <xsl:value-of select="name" />
	        <br />
	    </xsl:template>
	</xsl:stylesheet>
	
The output:

![image]({{ site.baseurl }}/uploads/screenshot.png)

	<?xml version="1.0" encoding="UTF-8"?>
	<html>
	   <head>
	      <title>XSLT Tutorial</title>
	   </head>
	   <body>
	      <img src="patrick.png"/>Patrick<br/>
	      <img src="paul.png"/>Paul<br/>
	   </body>
	</html>
	
XSLT is very powerfull, check out all the tags at [w3schools](http://www.w3schools.com/xsl/default.asp).