[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle C# Code Examples ##

This page shows a C# example.

Here's the code for the C# handle client (see file "[Program.cs](http://nurture.nature.com/tony/openhandle/code/csharp/Program.cs)"):
```
% cat Program.cs
using System;
using System.Linq;
using System.Net;
using Newtonsoft.Json.Linq;

namespace HandleClient
{
    /// <summary>
    /// C# code example for OpenHandle. See
    /// http://code.google.com/p/openhandle/wiki/OpenHandleCodeExamples
    /// </summary>
    /// <remarks>
    /// By Inigo Surguy, 67 Bricks Ltd, April 2008.
    /// Requires the Json.Net libraries (2.0b3+) by James Newton-King, available from http://www.codeplex.com/Json
    /// </remarks>
    class Program
    {
        private const string OPEN_HANDLE = "http://nascent.nature.com/openhandle/handle?format=json&id=";
        static void Main(string[] args)
        {
            string id = args.Length > 1 ? args[1] : "10100/nature";
            WebClient Client = new WebClient();
            string handlesText = Client.DownloadString(OPEN_HANDLE+id);
            var handles = JObject.Parse(handlesText);
            int count = 0;

            var strings =
                    (from c in handles["handleValues"].Children()
                          select string.Format(
                            "value #{0}\n"+
                            "  index = {1}\n"+
                            "  type = {2}\n"+
                            "  data = {3}\n"+
                            "  permission = {4}\n"+
                            "  ttl = {5}\n"+
                            "  timestamp = {6}\n"+
                            "  reference = {7}\n",
                              ++count, c.Value<int>("index"), c.Value<string>("type"), 
                              c["data"] is JObject ? "[object]" : c.Value<string>("data"), 
                              c.Value<string>("permission"), c.Value<int>("ttl"), c.Value<string>("timestamp"),
                              c["reference"]
                             )).ToList();

            Console.WriteLine("The handle "+handles["handle"]+" has "+strings.Count()+" values");
            foreach (var s in strings) Console.WriteLine(s);
            Console.ReadKey();
        }
    }
}
```