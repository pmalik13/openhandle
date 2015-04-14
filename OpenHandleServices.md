## OpenHandle Services ##

Registered OpenHandle services are managed with a handle (obviously):

  * [hdl:10100/openhandle](http://nascent.nature.com/openhandle/handle?id=10100/openhandle&format=json&&mimetype=text/plain)

The above handle is more a proof of concept. It is likely that a more permanent handle woudl be used going forward.

At the moment there is only one known OpenHandle service which is being run as a demo with no service commitment, i.e. no SLA:

  * http:/nascent.nature.com/openhandle/handle?

It would be useful to get at least one other service operational as a fallback.

Another consideration would be to emit an HTTP header from the OpenHandle service which would identify that service as an OpenHandle source. The header can be simply grabbed as:

```
% curl -I http://hdl.handle.net/10100/openhandle | grep ...
```