[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Prolog Code Examples ##

Here's how [SWI-Prolog](http://www.swi-prolog.org/) can process a Prolog program (contained in file "[openhandle.pl](http://nurture.nature.com/tony/openhandle/code/prolog/openhandle.pl)" - for working offline a test handle data record is read from the file "[h.json](http://nurture.nature.com/tony/openhandle/code/prolog/h.json)")
can grab a handle data record:
```
% cat openhandle.pl
#!/usr/bin/env swipl -q -t main -f

/*
    Prolog code example for OpenHandle. 
    See http://code.google.com/p/openhandle/wiki/OpenHandleCodeExamples
*/

:- use_module(library('http/http_open')).
:- use_module(library('http/json')).

%%   env(?Key, ?Value)
%
%    Environment variable table.

env(baseurl, 'http://nascent.nature.com/openhandle/handle?format=json&id=').
env(json, 'h.json').

%%   json_list(+JsonList, -List)
%
%    Unbundles JSON list object into list.

json_list(json(List), List).

%%   json_handle(+JsonList, -Comment, -Handle, -HandleValues)
%
%    Unbundles handle JSON list object.

json_handle([comment=Comment, handle=Handle, handleValues=HandleValues],
            Comment, Handle, HandleValues).

%%   json_handle_value(+JsonList, -Index, -Type, -Data, -Permission, -Ttl, -Timestamp, -Reference)
%
%    Unbundles handle value JSON list object into separate fields.

json_handle_value([index=Index, type=Type, data=Data, permission=Permission, ttl=Ttl, timestamp=Timestamp, reference=Reference],
                  Index, Type, Data, Permission, Ttl, Timestamp, Reference).

%%   handle_values(+JsonList, +Count)
%
%    Recurses over list of handle values as JSON list object and prints values.

handle_values([], 0).
handle_values([Head|Tail], Count) :- 
    Count_ is Count + 1,
    json_list(Head, List),
    json_handle_value(List, Index, Type, Data, Permission, Ttl, Timestamp, Reference),
    format('value #~w:~n', [Count_]),
    format('  index = ~w~n', [Index]),
    format('  type = ~w~n', [Type]),
    ( atom(Data)
      -> format('  data = ~w~n', [Data])
      ;  json_list(Data, Data_), format('  data = ~w~n', [Data_])
    ),
    format('  permission = ~w~n', [Permission]),
    format('  ttl = ~w~n', [Ttl]),
    format('  timestamp = ~w~n', [Timestamp]),
    format('  reference = ~w~n~n', [Reference]),
    % json_list(Reference, Reference_), format('  reference = ~w~n~n', [Reference_]),
    handle_values(Tail, Count_).

%%   get_handle
%
%    Get JSON from OpenHandle service and parse.

get_handle :-
    env(baseurl, BaseUrl),
    current_prolog_flag(argv, Argv),
    append(_, [--|Args], Argv),
    concat_atom(Args, ' ', Handle),
    concat_atom([BaseUrl, Handle], OpenHandle),
    http_open(OpenHandle, In, []),
    json_read(In, Json),
    close(In),
    parse_json(Json).

%%   get_test_handle
%
%    Get JSON from disk file and parse.

get_test_handle :-
    env(json, JsonFile),
    open(JsonFile, read, File, [alias(data)]),
    json_read(data, Json),
    close(File),
    parse_json(Json).

%%   parse_json(+Json)
%
%    Parse the JSON object.

parse_json(Json) :-
    json_list(Json, List),
    json_handle(List, _, Handle, HandleValues),
    length(HandleValues, Values),
    format('The handle <~w> has ~w values:~n~n', [Handle, Values]),
    handle_values(HandleValues, 0).

%%   main
%
%    Entry point for program.

main :-
    catch(get_handle, E, (print_message(error, E), get_test_handle)),
    halt.

main :-
    halt(1).

%__END__
```

Running this gives the following:
```
% swipl -q -t go -f openhandle.pl 10100/nature
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = json([adminRef=hdl:10100/nature?index=100, adminPermission=111111111111])
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = []

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = +86400
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = []

```

To just get the raw Prolog record for a handle this simple program (contained in file "[openhandle1.pl](http://nurture.nature.com/tony/openhandle/code/prolog/openhandle1.pl)" will do:
```
% cat openhandle1.pl
#!/usr/bin/env swipl -q -t main -f

/*
  Prolog program to grab handle values.
*/

:- use_module(library('http/http_open')).
:- use_module(library('http/json')).

eval :-
    BaseUrl = 'http://nascent.nature.com/openhandle/handle?format=json&id=',
    current_prolog_flag(argv, Argv),
    append(_, [--|Args], Argv),
    concat_atom(Args, ' ', Handle),
    concat_atom([BaseUrl, Handle], OpenHandle),
    http_open(OpenHandle, In, []),
    json_read(In, Json),
    close(In),
    write(Json),
    nl.

main :-
    catch(eval, E, (print_message(error, E), fail)),
    halt.

main :-
    halt(1).
```

Running this gives the following:
```
% swipl -q -t go -f openhandle1.pl 10100/nature
json([comment=OpenHandle (JSON) - see http://openhandle.googlecode.com/, handle=hdl:10100/nature, handleValues=[json([index=100, type=HS_ADMIN,
data=json([adminRef=hdl:10100/nature?index=100, adminPermission=111111111111]), permission=1110, ttl= +86400, timestamp=Wed Feb 28 15:37:06 GMT 2007, reference=[]]),
json([index=1, type=URL, data=http://www.nature.com/, permission=1110, ttl= +86400, timestamp=Wed Feb 28 15:37:06 GMT 2007, reference=[]])]])
```

But we can also use [SWI-Prolog](http://www.swi-prolog.org/)'s semantic web libraries to read the RDF/XML. Here's a simple program which prints out all the triples for a handle.
```
% cat my_semantic_handle.pl
:- use_module(library('semweb/rdf_http_plugin')).
:- use_module(library('semweb/rdf_db')).

list_triples :-
  rdf(S,P,O),
  print_triple(S,P,O),
  fail
  ; true.

print_triple(S,P,O) :-
  format('S = ~w~n', S),
  format('P = ~w~n', P),
  format('O = ~w~n', O).

go :-
  rdf_load('http://nascent.nature.com/openhandle/handle?&format=rdf&id=10100/nature'),  
  list_triples.
```

Running this gives the following listing:
```
% swipl -q -t go -f my_semantic_handle.pl
S = hdl:10100/nature
P = http://www.w3.org/1999/02/22-rdf-syntax-ns#type
O = http://hdl.handle.net/10100/handle.rdfs#Handle
S = hdl:10100/nature
P = http://hdl.handle.net/10100/handle.rdfs#handleValues
O = __http://nascent.nature.com/openhandle/handle?format=rdf&id=10100/nature#__List1
S = __http://nascent.nature.com/openhandle/handle?format=rdf&id=10100/nature#__Description1
P = http://www.w3.org/1999/02/22-rdf-syntax-ns#type
O = http://hdl.handle.net/10100/handle.rdfs#HandleValue
S = __http://nascent.nature.com/openhandle/handle?format=rdf&id=10100/nature#__Description1
P = http://hdl.handle.net/10100/handle.rdfs#index
O = literal(100)
...
```