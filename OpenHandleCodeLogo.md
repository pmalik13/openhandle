[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Logo Code Examples ##

These examples use [ACSLogo](http://www.alancsmith.co.uk/logo/) for MacOS X. Logo does not have system calls so cannot fetch an OpenHandle decsription, but it can read from a disk file.

Here's how [ACSLogo](http://www.alancsmith.co.uk/logo/) can process a Logo program (contained in file
"[openhandle.acsl](http://nurture.nature.com/tony/openhandle/code/logo/openhandle.acsl)" with text export at "[openhandle.acsl.txt](http://nurture.nature.com/tony/openhandle/code/logo/openhandle.acsl.txt)")
and can grab a handle data record (from the disk file "[h.json](http://nurture.nature.com/tony/openhandle/code/logo/h.json)"):
```
% cat openhandle.acsl.txt

make "json [/Users/tony/Desktop/h.json]
readhandle


Define "NoQuotes[[string][butlast butfirst :string

// if member? char 34 :string
//  [ butlast butfirst :string ]
//  [ :string ]]]
Define "GetLines[[][if eof?
  []
  [
    make "line freadlist
    if (count :line) > 0
      [
        grtype :line back 15
        make "word first :line
        if member? "" :word
          [ make "key noquotes :word ]
          [ make "key :word ]
        if equal? :key "handle
          [ make "handle noquotes last butlast :line ]
          []
        if equal? :key "index
          [ make "values :values + 1 
            make "listing lastput "@ :listing
            make "valuesline (list "value ". (word "" :values "") ",)
            addline "value :valuesline
            addline :key :line ][]
        if equal? :key "type
          [ addline :key :line ][]
        if equal? :key "data
         [ addline :key :line ][]
        if equal? :key "permission
          [ addline :key :line ][]
        if equal? :key "ttl
          [ addline :key :line ][]
        if equal? :key "timestamp
          [ addline :key :line ][]
        if equal? :key "reference
          [ addline :key :line ][]
        getlines
      ]
    []
  ]]]
Define "AddLine[[key line][local "temp
local "value
if member? "@ :line
  [
    make "temp []
  ]
  [
    if member? "{ :line
      [
        make "temp (list char 32 char 32 :key char 61 "[]) 
      ]
      [
        if member? "{} :line
          [
            make "temp (list char 32 char 32 :key char 61 "[]) 
          ]
          [
            make "value butlast butfirst butfirst :line
            if equal? :key "value
              [
                make "temp (list :key (word "# noquotes first :value)) 
              ]
              [
                make "temp (list char 32 char 32 :key char 61 noquotes first :value) 
              ]
         ]
     ]
  ]
make "listing lastput :temp :listing]]
Define "PrintLines[[list][local "head
local "tail
if empty? :list
[]
[
  make "head first :list
  make "tail butfirst :list
  if member? "@ :head
  [ moveline 
  ]
  [ grtype :head
    moveline
  ]
  printlines :tail
]
]]
Define "MoveLine[[][if equal? :moveline_action "linear
  [ back 15 
  ]
  [ if equal? :moveline_action "radial
      [ right 15
      ]
      []
  ]]]
Define "ReadHandle[[][//
cs home setpc 1 pu
forward 400 left 90 forward 250 right 90
make "listing []
make "handle ""
make "values 0
// make "moveline_action "radial
make "moveline_action "linear
//
openread :json
getlines
closereadfile
//
if equal? :moveline_action "radial
  [ home
    back 50
    left 60
  ]
  []
setpc 8
moveline
make "msg (list "The "handle (word char 60 :handle char 62) "has :values (word "values char 58))
grtype :msg moveline
printlines :listing 
]]
```

So, executing the following lines in the work window (by pressing cmd-rtn)
```
make "json [/Users/tony/Desktop/h.json]
readhandle
```
one gets the following graphics:

a) Linear - with `make "moveline_action "linear`

![http://nurture.nature.com/tony/imx/openhandle_logo_linear.jpg](http://nurture.nature.com/tony/imx/openhandle_logo_linear.jpg)

b) Radial - with `make "moveline_action "radial`

![http://nurture.nature.com/tony/imx/openhandle_logo_radial.jpg](http://nurture.nature.com/tony/imx/openhandle_logo_radial.jpg)