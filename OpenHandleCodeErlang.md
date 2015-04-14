[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Erlang Code Examples ##

This page shows an Erlang example. This makes use of the 'json.erl' module available here: [ejson-1.tgz](http://www.erlang-projects.org/Members/mremond/code/ejson/block_11381005072852/file).

Here's how Erlang (using the 'openhandle' module contained in file "[openhandle.erl](http://nurture.nature.com/tony/openhandle/code/erlang/openhandle.erl)") can grab a handle data record:

```
% erl openhandle.erl
```

opens and compiles the Erlang module, and one can then enter "`openhandle:get_handle("10100/nature")`" at the prompt as

```
1> openhandle:get_handle("10100/nature").
```

which gives us:

![http://nurture.nature.com/tony/imx/openhandle_erlang.jpg](http://nurture.nature.com/tony/imx/openhandle_erlang.jpg)

If the Erlang interpreter is opened without the module one will need to compile it first as "`c(openhandle).`"  That should compile and load both of the required modules (if not, you'll need to "`c(json).`" first).  Then one can call "`openhandle:get_handle("10100/nature").`" as before.


Here's the code for the OpenHandle module:
```
% cat openhandle.erl
%%% Example OpenHandle client
%%
%%  This program is in the Public Domain.

-module(openhandle).
-export([get_handle/1]).
-import(json, [obj_find/2, decode_string/1]).
-vsn("1").

get_handle(Handle) ->
    inets:start(),
    format_handle(get_handle_json(http:request(get_url(Handle)))).

get_url(Handle) ->
    string:concat(string:concat(string:concat(get_base_url(), "?id="), Handle), "&format=json").

get_base_url() ->
    "http://nascent.nature.com/openhandle/handle".

get_handle_json({ok, {_, _, Body}}) ->
    json:decode_string(Body).

format_handle({ok, Json}) ->
    {ok, Handle_name} = json:obj_find("handle", Json),
    {ok, Values} = json:obj_find("handleValues", Json),
    Value_count = tuple_size(Values),
    io:format("The handle <~s> has ~B values:~n", [Handle_name, Value_count]),
    tuple_foldl(fun(V, N) -> format_value(V, N) end, 1, Values),
    ok.

tuple_foldl(Fun, Acc0, Tuple) ->
    lists:foldl(Fun, Acc0, tuple_to_list(Tuple)).

format_value({json_object, _} = Obj, N) ->
    io:format("~nvalue #~B:~n", [N]),
    lists:foreach(fun(F) -> find_and_format(F, Obj) end,  ["index", "type", "data", "permission", "ttl", "timestamp", "reference"]),
    1 + N.

find_and_format(Field, {json_object, _} = Obj) ->
    format_prop(1, Field, json:obj_find(Field, Obj)).

format_prop(Level, Name, {ok, {}}) ->
    io:format("~s~s = []~n", [get_prefix_str(Level), Name]);

format_prop(Level, Name, {ok, {json_object, L}}) ->
    io:format("~s~s = {~n", [get_prefix_str(Level), Name]),
    lists:foreach(fun(V) -> format_item(Level + 1, V) end, L),
    io:format("~s}~n", [get_prefix_str(Level)]);

format_prop(Level, Name, {ok, Value}) ->
    io:format("~s~s = ~s~n", [get_prefix_str(Level), Name, Value]).

format_item(Level, {Name, Value}) ->
    io:format("~s~s = ~s~n", [get_prefix_str(Level), Name, Value]).

get_prefix_str(Level) ->
    string:chars(32, 2 * Level).
```