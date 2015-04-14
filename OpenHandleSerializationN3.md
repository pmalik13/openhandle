## RDF/N3 ##

The [RDF/N3](http://www.w3.org/TeamSubmission/n3/) serialization essentially looks like the following:

```
<info:hdl/10.1000/1> a :Handle ;
    :handleValues  (
        [
            h:index "1" ;
            h:type "URL" ;
            h:data <http://www.nature.com/> ;
            h:permission [
                a h:Permission;
                ...
            ] ;
            h:ttl [
                a h:TTL;
                ...
            ] ;
            h:timestamp "" ;
            h:reference [
               a h:Reference ;
               ...
            ]
        ]
    ) .
```