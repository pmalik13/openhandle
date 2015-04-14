[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Python Code Examples ##

Here's how a native Python program (contained in file "[handler.py](http://nurture.nature.com/tony/openhandle/code/python/handler.py)") can grab a handle data record:

```
% cat handler.py
#!/usr/bin/env python

import sys, simplejson, urllib

OPENHANDLE = "http://nascent.nature.com/openhandle/handle"

if len(sys.argv) > 1:
    hdl = sys.argv[1]
else:
    hdl = "10100/nature"

args = {}
args.update({
    'id': hdl,
    'format': 'json'
})
url = OPENHANDLE + '?' + urllib.urlencode(args)
h = simplejson.load(urllib.urlopen(url))

handle = h['handle']
values = h['handleValues']

s = ""
n_value = 0

for value in values:
    n_value += 1
    s += "\nvalue #" + str(n_value) + ":\n"
    s += "  index = " + value['index'] + "\n"
    s += "  type = " + value['type'] + "\n"
    if type(value['data']) is dict:
        s += "  data = " + str(type(value['data'])) + "\n"
    else:
        s += "  data = " + value['data'] + "\n"
    s += "  permission = " + value['permission'] + "\n"
    s += "  ttl = " + value['ttl'] + "\n"
    s += "  timestamp = " + value['timestamp'] + "\n"
    s += "  reference = " + str(type(value['reference'])) + "\n"

s1 = "The handle <" + handle + ">"
s1 += " has " + str(n_value) + " values (of " + str(len(values)) + "):\n"
print s1, s

#__END__
```


This can be run as:
```
% python handler.py 10100/nature
The handle <hdl:10100/nature> has 2 values (of 2):

value #1:
  index = 100
  type = HS_ADMIN
  data = <type 'dict'>
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = <type 'list'>

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = <type 'list'>

```

Here's the same thing (contained in file "[handler1.py](http://nurture.nature.com/tony/openhandle/code/python/handler1.py)") which this time merely dumps the parsed JSON string:
```
% cat handler1.py
#!/usr/bin/env python

import sys, simplejson, urllib

OPENHANDLE = "http://nascent.nature.com/openhandle/handle"

if len(sys.argv) > 1:
    hdl = sys.argv[1]
else:
    hdl = "10100/nature"

args = {}
args.update({
    'id': hdl,
    'format': 'json'
})
url = OPENHANDLE + '?' + urllib.urlencode(args)
h = simplejson.load(urllib.urlopen(url))

print h

#__END__
```

And this gives:
```
% python handler1.py 10100/nature
{u'comment': u'OpenHandle (JSON) - see http://openhandle.googlecode.com/',
u'handleValues': [{u'index': u'100', u'reference': [], u'permission': u'1110',
u'timestamp': u'Wed Feb 28 15:37:06 GMT 2007', u'data': {u'adminRef':
u'hdl:10100/nature?index=100', u'adminPermission': u'111111111111'}, u'ttl': u'+86400',
u'type': u'HS_ADMIN'}, {u'index': u'1', u'reference': [], u'permission': u'1110',
u'timestamp': u'Wed Feb 28 15:37:06 GMT 2007', u'data': u'http://www.nature.com/', u'ttl':
u'+86400', u'type': u'URL'}], u'handle': u'hdl:10100/nature'}
```