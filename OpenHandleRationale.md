## OpenHandle Rationale ##

Running a handle client natively usually means writing Java code which communicates with a handle server over port 2641 inbound/outbound traffic which inevitably leads to firewall issues within many organizations. (There is also a C library - but the Java code is the reference implementation.)

Alternately a handle server can be reached through a web service over an HTTP port:

  * Resolver
    * lookup: http://hdl.handle.net/
    * service: e.g. http://dx.doi.org/
  * Admin
    * web forms:

To open up handle means the ability to interact with a handle server using standard web services:

  * Standard port
    * port 80 rather than 2641
  * Standard object
    * common text-based markup as return object rather than native binary object
  * Structured format - differently from existing lookup web services returns
    * "structured" format
    * "complete" record

## Standard Port ##

As HTTP-based network endpoint returning markup body simple to request and consume from any language (e.g. Perl, Python, Ruby, Java, C?C++, .NET, etc.).


## Standard Object ##

  * As RDF/XML or RDF/N3 can be consumed as an RDF datasource (mozilla) or merged with other RDF datasources

  * As JSON very amenable to Web 2.0 type apps

## Structured Format ##

Having a structured format means more control facilitating new services.

complete record

> handle record is set of handle values containg mix of data and admin fields. not always clear where the bundaries are, i.e. some admin-type fields can be used as data

> Filters to select handle values