## Service ##

The project test service is sited at

  * [http://nascent.nature.com/openhandle/handle?](http://nascent.nature.com/openhandle/handle?)

## Parameters ##

Recognized parameters are:

  * `id` = handle
  * `index` = handle value index (e.g. `index=2`)
  * `type` = handle value type (e.g. `type=URL`)
  * `format` = `rdf` | `n3` | `json`
  * `mimetype` = mime type for overriding default mime type
  * `compact` = `true` | `false`
  * `callback` = JSONP callback function (e.g. `callback=openhandle`)
  * `include` = `datatypes` (**not** implemented yet)

## Examples ##

  * [?id=10.1000/1](http://nascent.nature.com/openhandle/handle?id=10.1000/1)

Returns default format (RDF/XML) for handle `10.1000/1` with mime type `application/rdf+xml`

  * [?id=10.1000/1&format=n3](http://nascent.nature.com/openhandle/handle?id=10.1000/1&format=n3)

Returns specified format (RDF/N3) for handle `10.1000/1` with mime type `text/rdf+n3`

  * [?id=10.1000/1&format=rdf&mimetype=application/xml](http://nascent.nature.com/openhandle/handle?id=10.1000/1&format=rdf&mimetype=application/xml)

Returns specified format (RDF/XML) for handle `10.1000/1` with mime type `application/xml`

  * [?id=10.1000/1&format=n3&mimetype=text/plain](http://nascent.nature.com/openhandle/handle?id=10.1000/1&format=n3&mimetype=text/plain)

Returns specified format (RDF/N3) for handle `10.1000/1` with mime type `text/plain`

  * [?id=10.1000/1&index=1](http://nascent.nature.com/openhandle/handle?id=10.1000/1&index=1)

Returns default format (RDF/XML) for handle `10.1000/1` and value for `index=1` with mime type `application/rdf+xml`

  * [?id=10.1000/1&type=URL](http://nascent.nature.com/openhandle/handle?id=10.1000/1&type=URL)

Returns default format (RDF/XML) for handle `10.1000/1` and value for `type=URL` with mime type `application/rdf+xml`