[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Smalltalk Code Examples ##

The examples here are for [Squeak](http://www.squeak.org/) - the free, cross-platform Smalltalk-80 implementation. See the blog [Jazz Programming](http://blogs.corriga.net/giovanni/), and these posts on RESTful Web Services for Yahoo! News Search: [Example 2.15 (JSON)](http://blogs.corriga.net/giovanni/archives/2007/7/3/restful_web_services_example_215/) and the earlier post [Example 2.1 (XML)](http://blogs.corriga.net/giovanni/archives/2007/6/28/restful_web_services_example_21/) for useful examples.

This code uses the [json](http://map1.squeakfoundation.org/sm/package/d38bdc2d-e52a-4167-ae73-2cf438c65c2f) package, which is distributed in [Monticello](http://wiki.squeak.org/squeak/1287) format (basic install instructions [here](http://wiki.squeak.org/squeak/43)).

```
aHandle := FillInTheBlank request: 'Enter handle'.
openHandle := 'http://nascent.nature.com/openhandle/handle'.
term := aHandle encodeForHTTP.
json := (openHandle , '?format=json&id=' , term) asUrl retrieveContents contents.
h := Json readFrom: json readStream.
n := 0.
(h at: #handleValues) do:
    [ :value |
        n := n + 1.
        Transcript
            show: 'value #' , (n printString); cr;
            show: '  index = ' , ((value at: #index) printString); cr;
            show: '  type = ' , ((value at: #type) printString); cr;
            show: '  data = ' , ((value at: #data) printString); cr;
            show: '  permission = ' , ((value at: #permission) printString); cr;
            show: '  ttl = ' , ((value at: #ttl) printString); cr;
            show: '  timestamp = ' , ((value at: #timestamp) printString); cr;
            show: '  reference ' , ((value at: #reference) printString); cr;
            cr.
    ]
```

Running this code gives the following output on the Transcript:

![http://nurture.nature.com/tony/imx/openhandle_squeak.jpg](http://nurture.nature.com/tony/imx/openhandle_squeak.jpg)

In the above I could have used this:
```
json := Curl new getContentsUrl: openHandle , '?format=json&id=' , term.
```
But I don't have a working version of the [CurlPlugin](http://wiki.squeak.org/squeak/5865) for Mac OS X. So instead I'm having to use this:
```
json := (openHandle , '?format=json&id=' , term) asUrl retrieveContents contents.
```

And here's a very rough cut ([OpenHandle.st](http://nurture.nature.com/tony/openhandle/code/smalltalk/OpenHandle.st)) at adding a new class and some class methods to package the above code:

```
Object subclass: #Handle
	instanceVariableNames: ''
	classVariableNames: 'Online'
	poolDictionaries: ''
	category: 'OpenHandle'!
!Handle commentStamp: 'tonyh 4/14/2008 04:33' prior: 0!
Class for accessing handles using OpenHandle services!


!Handle methodsFor: 'actions' stamp: 'tonyh 4/14/2008 12:08'!
doHandle: aHandle 
	"Answers a text description of aHandle"
	| id json h nValues |
	Online
		ifTrue: [id := aHandle encodeForHTTP.
			json := (self openHandle , '?format=json&id=' , id) asUrl retrieveContents contents]
		ifFalse: [id := self testId.
			json := self testJson].
	h := Json readFrom: json readStream.
	nValues := (h at: #handleValues) size asString.
	Transcript show: 'The handle <hdl:' , id , '> has ' , nValues , ' values:';
		 cr;
		 cr.
	(h at: #handleValues)
		doWithIndex: [:value :n | Transcript show: 'value #' , n printString;
				 cr;
				 show: '  index = ' , (value at: #index) printString;
				 cr;
				 show: '  type = ' , (value at: #type) printString;
				 cr;
				 show: '  data = ' , (value at: #data) printString;
				 cr;
				 show: '  permission = ' , (value at: #permission) printString;
				 cr;
				 show: '  ttl = ' , (value at: #ttl) printString;
				 cr;
				 show: '  timestamp = ' , (value at: #timestamp) printString;
				 cr;
				 show: '  reference ' , (value at: #reference) printString;
				 cr;
				 cr]! !

!Handle methodsFor: 'actions' stamp: 'tonyh 4/11/2008 16:48'!
enterHandle
	"Prompt user to enter a handle"

	^ FillInTheBlank request: 'Enter handle'.! !

!Handle methodsFor: 'actions' stamp: 'tonyh 4/14/2008 11:47'!
setOnline: aBoolean
Online := aBoolean! !


!Handle methodsFor: 'private' stamp: 'tonyh 4/14/2008 03:47'!
openHandle
^ 'http://nascent.nature.com/openhandle/handle'! !

!Handle methodsFor: 'private' stamp: 'tonyh 4/14/2008 04:28'!
testId
^ '10100/nature'! !

!Handle methodsFor: 'private' stamp: 'tonyh 4/14/2008 08:28'!
testJson
	^ '{
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
}'! !


!Handle methodsFor: 'initialization' stamp: 'tonyh 4/14/2008 11:49'!
initialize
self setOnline: true
! !


Handle subclass: #HandleValue
	instanceVariableNames: 'index type data permission ttl timestamp reference'
	classVariableNames: ''
	poolDictionaries: ''
	category: 'OpenHandle'!
!HandleValue commentStamp: 'tonyh 4/13/2008 16:28' prior: 0!
A handle value!


!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:53'!
data
^ data! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:56'!
data: aString
data := aString! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/13/2008 16:30'!
index
^ index! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:58'!
index: aString
	index := aString! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:55'!
permission
^ permission! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:57'!
permission: aString
	permission := aString! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:55'!
reference
	^ reference! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:57'!
reference: aString
	reference := aString! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:54'!
timestamp
^ timestamp! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:58'!
timestamp: aString
	timestamp := aString! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:53'!
ttl
^ ttl! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/14/2008 03:58'!
ttl: aString
	ttl := aString! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/13/2008 16:36'!
type
^ type! !

!HandleValue methodsFor: 'accessing' stamp: 'tonyh 4/13/2008 16:37'!
type: aString
type := aString! !
```

And here's a screenshot:

![http://nurture.nature.com/tony/imx/openhandle_squeak1.jpg](http://nurture.nature.com/tony/imx/openhandle_squeak1.jpg)

And here's a little bit of Squeak screen real estate showing top-left a free button with a handler method at bottom right (just open for editing), which when pressed puts up a popup at top right and resolves handle entered and writes results out onto the
default output - the Transcript.

![http://nurture.nature.com/tony/imx/squeak_real_estate.jpg](http://nurture.nature.com/tony/imx/squeak_real_estate.jpg)