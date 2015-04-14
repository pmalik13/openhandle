[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle JavaScript Code Examples ##

@@
This page badly needs updating. For now, though, for the latest JavaScript client library, see [openhandle-0.2.1.js](http://openhandle.googlecode.com/files/openhandle-0.2.1.js).
@@

Here's how a native JavaScript program (contained in file "[openhandle.js](http://nurture.nature.com/tony/openhandle/code/javascript/openhandle.js)") can grab a handle data record.

Note this example uses an embedded JSON object since there are cross-site security policy restrictions in place to open an JSON object. It also makes use of the [Rhino](http://www.mozilla.org/rhino/) JavaScript engine.

To run a similar script in the browser would require a [JSONP](http://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/) callback. A seperate template for OpenHandle will have to be set up.

```
% cat openhandle.js
var h = {
    "comment" : "OpenHandle (JSON) - see http://openhandle.googlecode.com/" ,
    "handle" : "hdl:10100/nature" ,
    "handleValues" : [
        {
            "index" : "100" ,
            "type" : "HS_ADMIN" ,
            "data" : {
                "adminRef" : "hdl:10100/nature?index=100" ,
                "adminPermission" : "111111111111"
            } ,
            "permission" : "1110" ,
            "ttl" : "+86400" ,
            "timestamp" : "Wed Feb 28 15:37:06 GMT 2007" ,
            "reference"  : {}
        } ,
        {
            "index" : "1" ,
            "type" : "URL" ,
            "data" : "http://www.nature.com/" ,
            "permission" : "1110" ,
            "ttl" : "+86400" ,
            "timestamp" : "Wed Feb 28 15:37:06 GMT 2007" ,
            "reference"  : {}
        }
    ]
}

var handle = h["handle"];
var values = h["handleValues"];
var value;
var i = 0;
var s = "";

s += "The handle <" + handle + "> has " + values.length + " values:\n";;
for (var i = 0; i < values.length; i++) {
    value = values[i];
    s += "\nvalue #" + (i + 1) + ":\n";
    s += "  index = " + value["index"] + "\n";
    s += "  type = " + value["type"] + "\n";
    s += "  data = " + value["data"] + "\n";
    s += "  permission = " + value["permission"] + "\n";
    s += "  ttl = " + value["ttl"] + "\n";
    s += "  timestamp = " + value["timestamp"] + "\n";
    s += "  reference = " + value["reference"] + "\n";
}
print(s);
```


```
% js openhandler.js
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = [object Object]
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [object Object]

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [object Object]

```

Embedding the same script within an HTML page between `<script>...</script>` tags and substituting `alert(s);` for `print(s);`, i.e.
```
<script type="application/javascript">
var h = {
...
}

var handle = h["handle"];
var values = h["handleValues"];
var value;
var i = 0;
var s = "";

s += "The handle <" + handle + "> has " + values.length + " values:\n";;
for (var i = 0; i < values.length; i++) {
    value = values[i];
    s += "\nvalue #" + (i + 1) + ":\n";
    s += "  index = " + value["index"] + "\n";
    s += "  type = " + value["type"] + "\n";
    s += "  data = " + value["data"] + "\n";
    s += "  permission = " + value["permission"] + "\n";
    s += "  ttl = " + value["ttl"] + "\n";
    s += "  timestamp = " + value["timestamp"] + "\n";
    s += "  reference = " + value["reference"] + "\n";
}
alert(s);
</script>
```
gives the following popup:

![http://nurture.nature.com/tony/imx/openhandle_js1.jpg](http://nurture.nature.com/tony/imx/openhandle_js1.jpg)

A freeform ['Hello World'](http://code.google.com/p/openhandle/wiki/OpenHandleHelloWorld) generated in the browser using [Processing.js](http://ejohn.org/blog/processingjs/) and [Firefox 3 RC1](http://www.mozilla.com/en-US/firefox/all-rc.html)

![http://nurture.nature.com/tony/imx/openhandle_p5.jpg](http://nurture.nature.com/tony/imx/openhandle_p5.jpg)