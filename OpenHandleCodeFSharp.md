[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle F# Code Examples ##

This page shows an F# example.

Here's the code for the F# handle client (see file "[HandleClient.fs](http://nurture.nature.com/tony/openhandle/code/fsharp/HandleClient.fs)"):
```
% cat HandleClient.fs
#light
#r "Newtonsoft.Json.dll"

// F# code example for OpenHandle. 
// See http://code.google.com/p/openhandle/wiki/OpenHandleCodeExamples
// By Inigo Surguy, 67 Bricks Ltd, April 2008.
// Requires the Json.Net libraries (2.0b3+) by James Newton-King, 
// available from http://www.codeplex.com/Json

open System.Net
open Newtonsoft.Json.Linq

let OPEN_HANDLE = "http://nascent.nature.com/openhandle/handle?format=json&id="

let downloadHandleValues id = 
    let client = new WebClient()
    let handlesText = client.DownloadString(OPEN_HANDLE+id)
    let handles = JObject.Parse(handlesText)
    (handles.Value<JArray>("handleValues")).Children<JToken>()
    
let displayItem (c:JToken) = 
    printf "value\n  index = %d\n  type = %s\n  data = %A\n  permission = %s\n  ttl = %d\n  timestamp = %s\n  reference = %A\n"
            (c.Value<int>("index"))
            (c.Value<string>("type")) 
            (c.Value<JToken>("data")) 
            (c.Value<string>("permission"))
            (c.Value<int>("ttl"))
            (c.Value<string>("timestamp"))
            (c.Value<JArray>("reference"))
        
let id = if (Sys.argv.Length < 2) then "10100/nature" else Sys.argv.[1]
id |> downloadHandleValues |> Seq.iter displayItem
```