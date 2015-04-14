[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Haskell Code Examples ##

This page shows a Haskell example. It makes use of these two libraries:

  * [HTTP](http://www.haskell.org/http/)

  * [JSON](http://hackage.haskell.org/cgi-bin/hackage-scripts/package/json-0.3.3)

Here's the code for the Haskell handle client (see file "[OpenHandle.hs](http://nurture.nature.com/tony/openhandle/code/haskell/OpenHandle.hs)"):
```
% cat OpenHandle.hs
module Openhandle where

import Data.Char (intToDigit)
import Network.URI
import Network.HTTP
import Text.JSON

main :: IO()
main = do
  putStr "Enter Handle: "
  handle <- getLine
  raw <-get (getUri handle)
  putStr (formatJson (decodeStrict raw))

formatJson :: Text.JSON.Result (JSObject JSValue) -> String
formatJson (Ok j) = let values = (getValue j "handleValues") in 
                      "\nThe handle <" ++ (formatHandle (getValue j "handle")) ++ "> has " ++ (show (countValues values)) ++ " values:\n"
                      ++ (formatHandleValues values)
formatJson (Error e) = e

formatHandleValues :: Maybe JSValue -> String
formatHandleValues (Just (JSArray values)) = formatHandleValuesN 1 values

formatHandleValuesN :: Int -> [JSValue] -> [Char]
formatHandleValuesN _ [] = ""
formatHandleValuesN n (x:xs) = "\nValue " ++ (show n) ++ ": " ++ (formatJSValue 0 x) ++ (formatHandleValuesN (n + 1) xs)

formatHandle :: Maybe JSValue -> String
formatHandle (Just (JSString str)) = fromJSString str

countValues :: Maybe JSValue -> Int
countValues (Just (JSArray arr)) = length arr

getValue :: JSObject o -> String -> Maybe o
getValue obj name = lookup name (fromJSObject obj)

formatJSValue :: Int -> JSValue -> String
formatJSValue indent (JSArray arr)  = (makeIndent indent) ++ (foldl (++) "" (map (formatJSValue indent) arr)) ++ "\n"
formatJSValue indent (JSString str) = (fromJSString str) ++ "\n"
formatJSValue indent (JSObject obj) = "{\n" ++ (foldl (formatEntry (indent + 1)) "" (fromJSObject obj)) ++ (makeIndent indent) ++ "}\n"

formatEntry :: Int -> [Char] -> (String, JSValue) -> [Char]
formatEntry indent str (name, val) = str ++ (makeIndent indent) ++ name ++ ": " ++ (formatJSValue indent val)

makeIndent :: Int -> String
makeIndent 0 = ""
makeIndent n = "  " ++ (makeIndent (n - 1))

get :: URI -> IO String
get uri =
    do
      eresp <- simpleHTTP (request uri)
      resp <- handleE (error . show) eresp
      case rspCode resp of
                      (2,0,0) -> return (rspBody resp)
                      _ -> error (httpError resp)
      where
        showRspCode (a,b,c) = map intToDigit [a,b,c]
        httpError resp = showRspCode (rspCode resp) ++ " " ++ rspReason resp

handleE :: Monad m => (ConnError -> m a) -> Either ConnError a -> m a
handleE h (Left e) = h e
handleE _ (Right v) = return v

request :: URI -> Request
request uri = Request{ rqURI = uri,
                       rqMethod = GET,
                       rqHeaders = [],
                       rqBody = "" }

getUri :: String -> URI
getUri handle =
    let uriStr = (getUriStr handle)
    in case parseURI uriStr of
         Nothing -> error ("Couldn't Parse ur: " ++ uriStr)
         Just uri -> uri

getUriStr :: String -> String
getUriStr handle = "http://nascent.nature.com/openhandle/handle?id=" ++ handle ++ "&format=json"
```

Running this from the [GHC](http://www.haskell.org/ghc/) interactive shell ("[GHCi](http://www.haskell.org/ghc/docs/latest/html/users_guide/ghci.html)") gives this:
```
% ghci
WARNING: GHCi invoked via 'ghci.exe' in *nix-like shells (cygwin-bash, in partic
ular)
         doesn't handle Ctrl-C well; use the 'ghcii.sh' shell wrapper instead
GHCi, version 6.8.2: http://www.haskell.org/ghc/  :? for help
Loading package base ... linking ... done.
Prelude> :load OpenHandle
Ok, modules loaded: Openhandle.
Prelude Openhandle> main
Loading package array-0.1.0.0 ... linking ... done.
Loading package containers-0.1.0.1 ... linking ... done.
Loading package bytestring-0.9.0.1 ... linking ... done.
Loading package parsec-2.1.0.0 ... linking ... done.
Loading package network-2.1.0.0 ... linking ... done.
Loading package HTTP-3001.0.4 ... linking ... done.
Loading package pretty-1.0.0.0 ... linking ... done.
Loading package json-0.3.3 ... linking ... done.
Enter Handle: 10100/nature

The handle <hdl:10100/nature> has 2 values:

Value 1: {
  index: 100
  type: HS_ADMIN
  data: {
    adminRef: hdl:10100/nature?index=100
    adminPermission: 111111111111
  }
  permission: 1110
  ttl: +86400
  timestamp: Wed Feb 28 15:37:06 GMT 2007
  reference:
}

Value 2: {
  index: 1
  type: URL
  data: http://www.nature.com/
  permission: 1110
  ttl: +86400
  timestamp: Wed Feb 28 15:37:06 GMT 2007
  reference:
}
Prelude Openhandle>
```