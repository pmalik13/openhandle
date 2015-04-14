## OpenHandle Serializations ##

Initial serializations include the following:

  * [RDF/XML](http://www.w3.org/TR/rdf-syntax-grammar/)
    * Standard XML-based serialization for RDF ([Page](http://code.google.com/p/openhandle/wiki/OpenHandleSerializationRdf), [Example](http://nascent.nature.com/openhandle/handle?id=4263537/4069&format=rdf&mimetype=application/xml))
  * [RDF/N3](http://www.w3.org/TeamSubmission/n3/)
    * Notation3, a text-based serialization of RDF ([Page](http://code.google.com/p/openhandle/wiki/OpenHandleSerializationN3), [Example](http://nascent.nature.com/openhandle/handle?id=4263537/4069&format=n3&mimetype=text/plain))
  * [JSON](http://www.json.org/)
    * JavaScript Object Notation ([Page](http://code.google.com/p/openhandle/wiki/OpenHandleSerializationJson), [Example](http://nascent.nature.com/openhandle/handle?id=4263537/4069&format=json&mimetype=text/plain))

A candidate serialization format would be [SPARQL](http://www.w3.org/TR/rdf-sparql-query/) Query Results: [XML Format](http://www.w3.org/TR/rdf-sparql-XMLres/) and/or [JSON](http://www.w3.org/TR/rdf-sparql-json-res/).

Each serialization can also occur in two forms - see OpenHandleCompactForms:

  * Full - each element of a compund structre is expressed
  * Compact - complex structures are represented as strings (or arrays of strings)