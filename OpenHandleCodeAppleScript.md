[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle AppleScript Code Examples ##

Here's how a native AppleScript program (contained in file "`handler.scpt`") can grab a handle data record:

```
% cat handler.scpt
(*
    AppleScript to get OpenHandle Result
*)

property openhandle : "http://nascent.nature.com/openhandle/handle"
property my_handle : "10100/nature"

on run argv
    try
        -- run interactively with "Script Editor"
        set result to display dialog "Enter handle" default answer my_handle
        set handle to text returned of result
    on error
        -- run in batch mode with "osascript"
        try
            set handle to item 1 of argv
        on error
            set handle to my_handle
        end try
    end try
    getHandle(handle)
end run

on getHandle(handle)
	
    set the_url to openhandle & "?id=" & handle & "&format=json"
    set json to do shell script "curl " & quoted form of the_url
	
    tell application "TextEdit"
        activate
        make new document at the front
        set text of the front document to json
    end tell
	
end getHandle
```

And this can be run (with optional Handle argument) as
```
% osascript handler.scpt [handle]
```
which gives:

![http://nurture.nature.com/tony/imx/openhandle_textedit_scpt.jpg](http://nurture.nature.com/tony/imx/openhandle_textedit_scpt.jpg)

Alternatively it can be run interactively by opening from the Finder (or from within the Script Editor itself) or by launching from the command line as:
```
% open handler.scpt
```

![http://nurture.nature.com/tony/imx/openhandle_scpt.jpg](http://nurture.nature.com/tony/imx/openhandle_scpt.jpg)

The Script Editor will prompt for the Handle to lookup.

Note that as no JSON parsers seem to be available for AppleScript one can choose to filter the JSON through a script handler (e.g. Perl, Ruby, etc.) or to attempt simple text parsing from within AppleScript. (An alternate route would be to parse the XML using the [XML Tools](http://www.latenightsw.com/freeware/XMLTools2/index.html) scripting addition from [Late Night Software](http://www.latenightsw.com/).)

In fact, a simple Perl filter such as

```
property filter : " | perl -ne 'chomp; s/\"(\\w+)\"(\\s+:)/$1$2/; s/\\[/{/g; s/\\]/}/g;  print;'"
...
set json to do shell script "curl " & quoted form of the_url & filter
```

does the job sufficiently. JSON is more or less the same as AppleScript record format the main differences being:

  * Keys should be unquoted
  * Newlines are not tolerated (remove or replace by option return)
  * Lists are delimited by '{}' rather than '[.md](.md)'

So, the **string** returned from the above filter is in AppleScript **record** format, but ...

But there is a real problem in coercing an AppleScript string to record format. This seems to be due to some arcane management of text, strings - internationalized and styled. It is not clear if there is a simple means of achieving this.

One possible workaround could be to use the script to write out a new script and execute that. For example, if the `getHandle` handler is replaced by

```
on getHandle(handle)
	
    set the_url to openhandle & "?id=" & handle & "&format=json"
    set json to do shell script "curl " & quoted form of the_url & filter
	
    -- here we set the record explicitly
    set json to ¬
        {comment:"OpenHandle (JSON) - see http://openhandle.googlecode.com/", handle:"hdl:10100/nature", handleValues:{¬
        {index:"100", type:"HS_ADMIN", data:¬
            {adminRef:"hdl:10100/nature?index=100", adminPermission:¬
                "111111111111"} ¬
            , permission:"1110", ttl:"+86400", timestamp:"Wed Feb 28 15:37:06 GMT 2007", reference:{} ¬
        }, ¬
        {index:"1", type:"URL", data:"http://www.nature.com/", permission:"1110", ttl:"+86400", timestamp:"Wed Feb 28 15:37:06 GMT 2007", reference:{} ¬
            } ¬
        } ¬
    } ¬
			
	
    set results to "The handle <" & handle of json & "> "
    set values to handleValues of json
    set nvalues to length of handleValues of json
    set results to results & "has " & nvalues & " values:" & return & return

    -- set results to results & h as text
	
    repeat with n from 1 to nvalues
		
        set _item to item n of values
		
        set results to results & "value #" & n & ":" & return
		
        set results to results & "    index is " & index of _item & return
        set results to results & "    type is " & type of _item & return
        try
            set results to results & "    data is " & data of _item & return
        on error
            set results to results & "    data is a " & class of data of _item & return
        end try
        set results to results & "    permission is " & permission of _item & return
        set results to results & "    ttl is " & (ttl of _item) div 3600 & " hours " & return
        set results to results & "    timestamp is " & timestamp of _item & return
        if length of reference of _item is 0 then
           set results to results & "    reference is an empty " & class of reference of _item & return
        else
            set results to results & "    reference is a " & class of reference of _item & return
        end if
    end repeat

	tell application "TextEdit"
		activate
		make new document at the front
		set text of the front document to results
	end tell
	
end getHandle
```

This results in the following:

![http://nurture.nature.com/tony/imx/openhandle_textedit1_scpt.jpg](http://nurture.nature.com/tony/imx/openhandle_textedit1_scpt.jpg)