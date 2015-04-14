[![](http://www.handle.net/images2/hs_logo_bundle_sm1.gif)](http://www.handle.net/) See  project pages:  [Home Page](http://code.google.com/p/openhandle/wiki/OpenHandle) ([All Pages](http://code.google.com/p/openhandle/wiki)) | ['Hello World'](http://code.google.com/p/openhandle/wiki/OpenHandleHelloWorld) | [Code Examples](http://code.google.com/p/openhandle/wiki/OpenHandleCodeExamples) | [Handle Gallery](http://code.google.com/p/openhandle/wiki/HandleGalleryPage) | [To-Do List](http://code.google.com/p/openhandle/wiki/OpenHandleToDoList)

| ![http://www.crossref.org/CrossTech/images/last-mile.png](http://www.crossref.org/CrossTech/images/last-mile.png) |
|:------------------------------------------------------------------------------------------------------------------|
| _[The Last Mile](http://www.crossref.org/CrossTech/2008/10/the_last_mile.html) - CrossTech, Oct 1, '08_ |

![http://images.del.icio.us/static/img/delicious.small.gif](http://images.del.icio.us/static/img/delicious.small.gif) [del.icio.us](http://del.icio.us/tag/openhandle)

**[
[AppleScript](OpenHandleCodeAppleScript.md) |
[C](OpenHandleCodeC.md) |
[C#](OpenHandleCodeCSharp.md) |
[Erlang](OpenHandleCodeErlang.md) |
[F#](OpenHandleCodeFSharp.md) |
[Haskell](OpenHandleCodeHaskell.md) |
[Java](OpenHandleCodeJava.md) |
[JavaScript](OpenHandleCodeJavaScript.md) |
[Lisp](OpenHandleCodeLisp.md) |
[Logo](OpenHandleCodeLogo.md) |
[Perl](OpenHandleCodePerl.md) |
[PHP](OpenHandleCodePhp.md) |
[Prolog](OpenHandleCodeProlog.md) |
[Python](OpenHandleCodePython.md) |
[Ruby](OpenHandleCodeRuby.md) |
[Smalltalk](OpenHandleCodeSmalltalk.md) |
[XSLT](OpenHandleCodeXslt.md)
]**

_**Oct 4, '08** ... added JavaScript modules [openhandle-0.2.1.js](http://openhandle.googlecode.com/files/openhandle-0.2.1.js) and [openhandle-utils-0.2.1.js](http://openhandle.googlecode.com/files/openhandle-utils-0.2.1.js) (together with minified versions)_ ...

OpenHandle is an open-source community project to expose [Handle Values](http://www.handle.net/) in common text-based serializations to make the data stored within the records more accessible to Web applications. OpenHandle is not a replacement for the [Handle System](http://www.handle.net/), but rather an alternate means to access Handle. See a simple [test client](http://nurture.nature.com/openhandle/) (sources:  [html](http://nurture.nature.com/openhandle/handle.html.txt),
[js](http://nurture.nature.com/openhandle/scripts/handle.js)).

Specifically, OpenHandle is implemented as a Java servlet front-end to the Handle System which exposes records in structured form (see [XML](http://nascent.nature.com/openhandle/handle?&mimetype=text/plain&format=rdf&id=10100/10.1038/nri1842) and [JSON](http://nascent.nature.com/openhandle/handle?&mimetype=text/plain&format=json&id=10100/10.1038/nri1842) examples) and as such makes Handle records readable via a simple web service. See also:

  * [Home Page](http://code.google.com/p/openhandle/wiki/OpenHandle) ([All Pages](http://code.google.com/p/openhandle/wiki))
  * ['Hello World'](http://code.google.com/p/openhandle/wiki/OpenHandleHelloWorld) document
  * [Code Examples](http://code.google.com/p/openhandle/wiki/OpenHandleCodeExamples) for  accessing OpenHandle from various programming languages
  * [Handle Gallery](http://code.google.com/p/openhandle/wiki/HandleGalleryPage) of Handles 'in the wild' as serialized by OpenHandle
  * [To-Do List](http://code.google.com/p/openhandle/wiki/OpenHandleToDoList)

| ![http://nurture.nature.com/tony/imx/handle_xml_small_c.png](http://nurture.nature.com/tony/imx/handle_xml_small_c.png) | ![http://nurture.nature.com/tony/imx/handle_json_small_c.png](http://nurture.nature.com/tony/imx/handle_json_small_c.png) |
|:------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------|
| **XML: 'Compact' form** ([Link](http://nascent.nature.com/openhandle/handle?&mimetype=text/plain&format=rdf&compact=true&id=10100/nature)) | **JSON: 'Compact' form** ([Link](http://nascent.nature.com/openhandle/handle?&mimetype=text/plain&format=json&compact=true&id=10100/nature)) |

| ![http://nurture.nature.com/tony/imx/handle_xml_small.png](http://nurture.nature.com/tony/imx/handle_xml_small.png) | ![http://nurture.nature.com/tony/imx/handle_json_small.png](http://nurture.nature.com/tony/imx/handle_json_small.png) |
|:--------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------|
| **XML: 'Full' form** ([Link](http://nascent.nature.com/openhandle/handle?&mimetype=text/plain&format=rdf&compact=false&id=10100/nature)) | **JSON: 'Full' form** ([Link](http://nascent.nature.com/openhandle/handle?&mimetype=text/plain&format=json&compact=false&id=10100/nature)) |
