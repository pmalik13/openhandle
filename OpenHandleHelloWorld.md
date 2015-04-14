[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle 'Hello World' ##

In order to provide a simple test for languages that support the OpenHandle interface, the following 'Hello World' type document is proposed:

```
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = [Hash]
  permission = 1110
  ttl = 24 hours
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [Array]

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = 24 hours
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [Array]
```

Note that there may be small differences in rendering the 'data' and 'references' fields which may be structured.

See the [Code Examples](http://code.google.com/p/openhandle/wiki/OpenHandleCodeExamples) page for language implementations.