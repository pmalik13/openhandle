[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle XSLT Code Examples ##

Here's a simple XSLT stylesheet (contained in file "[openhandle.xsl](http://nurture.nature.com/tony/openhandle/code/xslt/openhandle.xsl)") which can be used to grab a handle data record:

```
% cat openhandle.xsl
<?xml version="1.0"?>
<!DOCTYPE stylesheet [
    <!ENTITY nl "<xsl:text>&#10;</xsl:text>">
]>
<xsl:transform version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:r="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns:h="http://hdl.handle.net/10100/handle.rdfs#"
    exclude-result-prefixes="r h">

<xsl:output method="text"/>
<xsl:strip-space elements="r:RDF"/>

<xsl:template match="/r:RDF/h:Handle">
    <xsl:text>The handle &lt;</xsl:text>
    <xsl:value-of select="./@r:about"/>
    <xsl:text>&gt; has </xsl:text>
    <xsl:value-of select="count(*/h:HandleValue)"/>
    <xsl:text> values:</xsl:text>&nl;
    <xsl:for-each select="*/h:HandleValue">
        &nl;<xsl:text>value #</xsl:text>
        <xsl:value-of select="position()"/>
        <xsl:text>:</xsl:text>&nl;
        <xsl:call-template name="handle-value"/>
    </xsl:for-each>
</xsl:template>

<xsl:template name="handle-value">

    <!-- Set field variables -->
    <xsl:variable name="index">
        <xsl:value-of select="normalize-space(h:index)"/>
    </xsl:variable>
    <xsl:variable name="type">
        <xsl:value-of select="normalize-space(h:type)"/>
    </xsl:variable>
    <xsl:variable name="data">
        <xsl:value-of select="normalize-space(h:data/@r:resource)"/>
    </xsl:variable>
    <xsl:variable name="permission">
        <xsl:value-of select="normalize-space(h:permission)"/>
    </xsl:variable>
    <xsl:variable name="ttl">
        <xsl:value-of select="normalize-space(h:ttl)"/>
    </xsl:variable>
    <xsl:variable name="timestamp">
        <xsl:value-of select="normalize-space(h:timestamp)"/>
    </xsl:variable>
    <xsl:variable name="reference">
        <xsl:value-of select="normalize-space(h:reference)"/>
    </xsl:variable>

    <!-- Write handle value -->
    <xsl:if test="$index!=''">
        <xsl:text>  index = </xsl:text>
        <xsl:value-of select="$index"/>&nl;
    </xsl:if>
    <xsl:if test="$type!=''">
        <xsl:text>  type = </xsl:text>
        <xsl:value-of select="$type"/>&nl;
    </xsl:if>
    <xsl:choose>
        <xsl:when test="$data!=''">
            <xsl:text>  data = </xsl:text>
            <xsl:value-of select="$data"/>&nl;
        </xsl:when>
        <xsl:otherwise>
            <xsl:text>  data = [object]</xsl:text>&nl;
        </xsl:otherwise>
    </xsl:choose>
    <xsl:if test="$permission!=''">
        <xsl:text>  permission = </xsl:text>
        <xsl:value-of select="$permission"/>&nl;
    </xsl:if>
    <xsl:if test="$ttl!=''">
        <xsl:text>  ttl = </xsl:text>
        <xsl:value-of select="$ttl"/>&nl;
    </xsl:if>
    <xsl:if test="$timestamp!=''">
        <xsl:text>  timestamp = </xsl:text>
        <xsl:value-of select="$timestamp"/>&nl;
    </xsl:if>
    <xsl:if test="$reference=''">
        <xsl:text>  reference = []</xsl:text>
        <xsl:value-of select="string($reference)"/>&nl;
    </xsl:if>

</xsl:template>

</xsl:transform>
```

This can be applied simply (here using [Xalan](http://xalan.apache.org/index.html)) as:
```
% xalan -xsl openhandle.xsl -in 'http://nascent.nature.com/openhandle/handle?format=rdf&id=10100/nature'
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = [object]
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = []

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = []
```

A variant stylesheet (contained in file "[openhandle\_dot.xsl](http://nurture.nature.com/tony/openhandle/code/xslt/openhandle_dot.xsl)")  can be used to generate a "`.dot`" file for a [GraphViz](http://www.graphviz.org/) presentation:

```
% cat openhandle_dot.xsl
<?xml version="1.0"?>
<!DOCTYPE stylesheet [
    <!ENTITY nl "<xsl:text>&#10;</xsl:text>">
]>
<xsl:transform version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:r="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns:h="http://hdl.handle.net/10100/handle.rdfs#"
    exclude-result-prefixes="r h">

<xsl:output method="text"/>
<xsl:strip-space elements="r:RDF"/>

<xsl:template match="/r:RDF/h:Handle">
    <xsl:text>digraph h {</xsl:text>&nl;
    <xsl:text>graph [</xsl:text>&nl;
    <xsl:text>  rankdir = "LR"</xsl:text>&nl;
    <xsl:text>];</xsl:text>&nl;
    &nl;
    <xsl:variable name="handle">
        <xsl:value-of select="./@r:about"/>
    </xsl:variable>
    <xsl:for-each select="*/h:HandleValue">
        <xsl:text>"&lt;</xsl:text>
        <xsl:value-of select="$handle"/>
        <xsl:text>&gt;" -> </xsl:text>
        <xsl:text>value_</xsl:text>
        <xsl:value-of select="position()"/>
        <xsl:text>:index;</xsl:text>&nl;
        <xsl:text>value_</xsl:text>
        <xsl:value-of select="position()"/>
        <xsl:text> [</xsl:text>&nl;
        <xsl:text>  shape = "record"</xsl:text>&nl;
        <xsl:text>  </xsl:text>
        <xsl:call-template name="handle-value"/>&nl;
        <xsl:text>];</xsl:text>&nl;&nl;
    </xsl:for-each>
    <xsl:text>}</xsl:text>&nl;

</xsl:template>

<xsl:template name="handle-value">

    <!-- Set field variables -->
    <xsl:variable name="index">
        <xsl:value-of select="normalize-space(h:index)"/>
    </xsl:variable>
    <xsl:variable name="type">
        <xsl:value-of select="normalize-space(h:type)"/>
    </xsl:variable>
    <xsl:variable name="data">
        <xsl:value-of select="normalize-space(h:data/@r:resource)"/>
    </xsl:variable>
    <xsl:variable name="permission">
        <xsl:value-of select="normalize-space(h:permission)"/>
    </xsl:variable>
    <xsl:variable name="ttl">
        <xsl:value-of select="normalize-space(h:ttl)"/>
    </xsl:variable>
    <xsl:variable name="timestamp">
        <xsl:value-of select="normalize-space(h:timestamp)"/>
    </xsl:variable>
    <xsl:variable name="reference">
        <xsl:value-of select="normalize-space(h:reference)"/>
    </xsl:variable>

    <!-- Write handle value -->
    <xsl:text>label = "</xsl:text>
    <xsl:if test="$index!=''">
        <xsl:text>&lt;index&gt; </xsl:text>
        <xsl:value-of select="$index"/>
        <xsl:text> | </xsl:text>
    </xsl:if>
    <xsl:if test="$type!=''">
        <xsl:text>&lt;type&gt; </xsl:text>
        <xsl:value-of select="$type"/>
        <xsl:text> | </xsl:text>
    </xsl:if>
    <xsl:choose>
        <xsl:when test="$data!=''">
            <xsl:text>&lt;data&gt; </xsl:text>
            <xsl:value-of select="$data"/>
        <xsl:text> | </xsl:text>
        </xsl:when>
        <xsl:otherwise>
            <xsl:text>&lt;data&gt; [...]</xsl:text>
        <xsl:text> | </xsl:text>
        </xsl:otherwise>
    </xsl:choose>
    <xsl:if test="$permission!=''">
        <xsl:text>&lt;permission&gt; </xsl:text>
        <xsl:value-of select="$permission"/>
        <xsl:text> | </xsl:text>
    </xsl:if>
    <xsl:if test="$ttl!=''">
        <xsl:text>&lt;ttl&gt; </xsl:text>
        <xsl:value-of select="$ttl"/>
        <xsl:text> | </xsl:text>
    </xsl:if>
    <xsl:if test="$timestamp!=''">
        <xsl:text>&lt;timestamp&gt; </xsl:text>
        <xsl:value-of select="$timestamp"/>
        <xsl:text> | </xsl:text>
    </xsl:if>
    <xsl:if test="$reference=''">
        <xsl:text>&lt;reference&gt; []</xsl:text>
        <xsl:value-of select="string($reference)"/>
    </xsl:if>
    <xsl:text>"</xsl:text>

</xsl:template>

</xsl:transform>
```

This can be applied simply (here using [Xalan](http://xalan.apache.org/index.html)) as:
```
% xalan -xsl openhandle_dot.xsl -in 'http://nascent.nature.com/openhandle/handle?format=rdf&id=10100/nature' > test.dot
```

This can either be be displayed directly using a [GraphViz](http://www.graphviz.org/) viewer application, e.g.
```
% dotty test.dot
```
or can processed by "`dot`" and saved in a preferred graphics format, e.g.
```
% dot test.dot -T jpg > test.jpg
```

An example of such a graphics file is this:

![http://nurture.nature.com/tony/imx/openhandle_dot.jpg](http://nurture.nature.com/tony/imx/openhandle_dot.jpg)

A variant stylesheet (contained in file "[openhandle\_dot.xsl](http://nurture.nature.com/tony/openhandle/code/xslt/openhandle_lol.xsl)")  can be used to generate LOLCODE:

```
% cat openhandle_lol.xsl
<?xml version="1.0"?>
<!DOCTYPE stylesheet [
    <!ENTITY nl "<xsl:text>&#10;</xsl:text>">
    <!ENTITY hdl "<xsl:text>hdl:</xsl:text>">
    <!ENTITY nil "<xsl:text></xsl:text>">
]>
<xsl:transform version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:r="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns:h="http://hdl.handle.net/10100/handle.rdfs#"
    exclude-result-prefixes="r h">

<xsl:output method="text"/>
<xsl:strip-space elements="r:RDF"/>

<xsl:template match="/r:RDF/h:Handle">
    <xsl:text>HAI</xsl:text>&nl;
    <xsl:text>BTW, UR HDL "</xsl:text>

    <xsl:value-of select="substring(./@r:about, 5)"/>
    <xsl:text>" HAS </xsl:text>
    <xsl:value-of select="count(*/h:HandleValue)"/>
    <xsl:text> VALS</xsl:text>&nl;
    <xsl:text>IM IN UR BUCKETS MAKING UP VALS</xsl:text>&nl;

    <xsl:for-each select="*/h:HandleValue">

        &nl;<xsl:text>  GIMME VAL NUM</xsl:text>
        <xsl:value-of select="position()"/>&nl;
        <xsl:call-template name="handle-value"/>
        <xsl:text>  KTHX.</xsl:text>&nl;
    </xsl:for-each>

    &nl;<xsl:text>KTHXBYE.</xsl:text>&nl;

</xsl:template>

<xsl:template name="handle-value">

    <!-- Set field variables -->
    <xsl:variable name="index">
        <xsl:value-of select="normalize-space(h:index)"/>
    </xsl:variable>
    <xsl:variable name="type">
        <xsl:value-of select="normalize-space(h:type)"/>
    </xsl:variable>

    <xsl:variable name="data">
        <xsl:value-of select="normalize-space(h:data/@r:resource)"/>
    </xsl:variable>
    <xsl:variable name="permission">
        <xsl:value-of select="normalize-space(h:permission)"/>
    </xsl:variable>
    <xsl:variable name="ttl">
        <xsl:value-of select="normalize-space(h:ttl)"/>
    </xsl:variable>

    <xsl:variable name="timestamp">
        <xsl:value-of select="normalize-space(h:timestamp)"/>
    </xsl:variable>
    <xsl:variable name="reference">
        <xsl:value-of select="normalize-space(h:reference)"/>
    </xsl:variable>

    <!-- Write handle value -->
    <xsl:if test="$index!=''">

        <xsl:text>    I CAN HAS INDEX</xsl:text>&nl;
        <xsl:text>      ITZ AT </xsl:text>
        <xsl:value-of select="$index"/>&nl;
        <xsl:text>    KTHX.</xsl:text>&nl;
    </xsl:if>
    <xsl:if test="$type!=''">

        <xsl:text>    I CAN HAS TYPE</xsl:text>&nl;
        <xsl:text>      ITZ AT </xsl:text>
        <xsl:value-of select="$type"/>&nl;
        <xsl:text>    KTHX.</xsl:text>&nl;
    </xsl:if>
    <xsl:choose>

        <xsl:when test="$data!=''">
            <xsl:text>    I CAN HAS DATA</xsl:text>&nl;
            <xsl:text>      ITZ AT </xsl:text>
            <xsl:value-of select="$data"/>&nl;
            <xsl:text>    KTHX.</xsl:text>&nl;
        </xsl:when>

        <xsl:otherwise>
            <xsl:text>    I CAN HAS DATA</xsl:text>&nl;
            <xsl:text>      ITZ AT BIG FLUFFY BALL</xsl:text>&nl;
            <xsl:text>      LOL ITZ SOOO WOOLLY</xsl:text>&nl;
            <xsl:text>    KTHX.</xsl:text>&nl;

        </xsl:otherwise>
    </xsl:choose>
    <xsl:if test="$permission!=''">
        <xsl:text>    I CAN HAS PERMISSION</xsl:text>&nl;
        <xsl:text>      ITZ AT </xsl:text>
        <xsl:value-of select="$permission"/>&nl;
        <xsl:text>    KTHX.</xsl:text>&nl;

    </xsl:if>
    <xsl:if test="$ttl!=''">
        <xsl:text>    I CAN HAS TTL</xsl:text>&nl;
        <xsl:text>      ITZ AT </xsl:text>
        <xsl:value-of select="$ttl"/>&nl;
        <xsl:text>    KTHX.</xsl:text>&nl;

    </xsl:if>
    <xsl:if test="$timestamp!=''">
        <xsl:text>    I CAN HAS TIMESTAMP</xsl:text>&nl;
        <xsl:text>      ITZ AT </xsl:text>
        <xsl:value-of select="$timestamp"/>&nl;
        <xsl:text>    KTHX.</xsl:text>&nl;

    </xsl:if>
    <xsl:if test="$reference=''">
        <xsl:text>    I CAN HAS REFERENCE</xsl:text>&nl;
        <xsl:text>      ITZ AT </xsl:text>
<!--
        <xsl:value-of select="$reference"/>&nl;
-->
        <xsl:text>EMPTY</xsl:text>&nl;
        <xsl:text>    KTHX.</xsl:text>&nl;

    </xsl:if>

</xsl:template>

</xsl:transform>
```

This can be applied simply (here using [Xalan](http://xalan.apache.org/index.html)) as:
```
% xalan -xsl openhandle_lol.xsl -in 'http://nascent.nature.com/openhandle/handle?format=rdf&id=10.1000/1'
```
which gives the following:
```
HAI
BTW, UR HDL "10.1000/1" HAS 2 VALS
IM IN UR BUCKETS MAKING UP VALS

  GIMME VAL NUM1
    I CAN HAS INDEX
      ITZ AT 100
    KTHX.
    I CAN HAS TYPE
      ITZ AT HS_ADMIN
    KTHX.
    I CAN HAS DATA
      ITZ AT BIG FLUFFY BALL
      LOL ITZ SOOO WOOLLY
    KTHX.
    I CAN HAS PERMISSION
      ITZ AT 1110
    KTHX.
    I CAN HAS TTL
      ITZ AT +86400
    KTHX.
    I CAN HAS TIMESTAMP
      ITZ AT Thu Apr 13 16:08:57 BST 2000
    KTHX.
    I CAN HAS REFERENCE
      ITZ AT EMPTY
    KTHX.
  KTHX.

  GIMME VAL NUM2
    I CAN HAS INDEX
      ITZ AT 1
    KTHX.
    I CAN HAS TYPE
      ITZ AT URL
    KTHX.
    I CAN HAS DATA
      ITZ AT http://www.doi.org/index.html
    KTHX.
    I CAN HAS PERMISSION
      ITZ AT 1110
    KTHX.
    I CAN HAS TTL
      ITZ AT +86400
    KTHX.
    I CAN HAS TIMESTAMP
      ITZ AT Fri Sep 10 20:49:59 BST 2004
    KTHX.
    I CAN HAS REFERENCE
      ITZ AT EMPTY
    KTHX.
  KTHX.

KTHXBYE.
```