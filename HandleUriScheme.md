## URI Scheme ##

Currently handle is registered as an [info](http://info-uri.info/) URI namespace (see the [handle namespace](http://info-uri.info/registry/OAIHandler?verb=GetRecord&metadataPrefix=reg&identifier=info:hdl/) registration) which means a handle can be cited in URI form as e.g.

> info:hdl/1234/567

This is a temporary measure and uses the [info](http://info-uri.info/) URI scheme as a bootstrapping mechanism.

A native URI scheme for handle should be registered.

Four recent (Feb. '08) posts to the handle-info mailing list are germane:

  * [Feb 12](http://www.handle.net/mail-archive/handle-info/msg00243.html)
    * discusses rationale for embedding URI scheme details into revised RFC 3651, rather than  making a separate RFC
  * [Feb 13](http://www.handle.net/mail-archive/handle-info/msg00244.html)
    * discusses how Handle should mirror HTTP in defining a generic syntax, devoid of application syntax for querystring and/or fragment
  * [Feb 14](http://www.handle.net/mail-archive/handle-info/msg00245.html)
    * provides a first cut at an ABNF for a generic Handle URI scheme
  * [Feb 25](http://www.handle.net/mail-archive/handle-info/msg00252.html)
    * points to the DNS URI scheme as a possible model for a Handle URI scheme

The first cut ABNF from message #3 is reproduced below. In light of message #4, this might be modified to allow for differentiation between naming and resolution in dealing with naming authority. That is, one might see something more like:

```
  hdl:1234/567

  hdl://190.12.34.56/1234/567
```

the first form being the handle as name, and the second the handle presented to a specific handle server for resolution.

```
       ;; ABNF for a Handle string

       handle             = naming-authority "/" local-name

       naming-authority   = *(naming-authority  ".") na-segment

       na-segment         = 1*(%x00-2D / %x30-3F / %x41-FF )
                            ; any octets that map to UTF-8 encoded
                            ; Unicode 2.0 characters except
                            ; octets "%x2E" and "%x2F" (which
                            ; correspond to the ASCII characters "."
                            ; and "/")

       local-name         = *(%x00-FF)
                            ; any octets that map to UTF-8 encoded
                            ; Unicode 2.0 characters



       ;; ABNF for a Handle URI

       hdl-uri            = "hdl" ":" "//" u-handle [ "?" query ]

                                [ "#" fragment ]

       ; All productions not starting "u-" are from RFC 3986

       u-handle           = u-naming-authority "/" u-local-name

       u-naming-authority = *(u-naming-authority  ".") u-na-segment

       u-na-segment       = *( u-unreserved / pct-encoded / sub-delims
                             ; native handle chars for <na-segment> other
                             ; than characters in the range
                             ; (%x21 / %x24 / %x26-2D / %x30-39 / %x3B
                             ;  / %x3D / %x41-5A / %x5F /%x61-7A / %x7E)
                             ; which must be %-encoded

       u-local-name       = *( pchar / "/" )
                             ; native handle chars for <local-name> other
                             ; than characters in the range
                             ; (%x21 / %x24 / %x26-2F / %x30-39 / %x3B
                             ;  / %x3D / %x41-5A / %x5F /%x61-7A / %x7E)
                             ; which must be %-encoded

       u-unreserved       = ALPHA / DIGIT / "-" / "_" / "~"
```

Crib sheet:
```

     00 nul   01 soh   02 stx   03 etx   04 eot   05 enq   06 ack   07 bel
     08 bs    09 ht    0a nl    0b vt    0c np    0d cr    0e so    0f si
     10 dle   11 dc1   12 dc2   13 dc3   14 dc4   15 nak   16 syn   17 etb
     18 can   19 em    1a sub   1b esc   1c fs    1d gs    1e rs    1f us
     20 sp    21  !    22  "    23  #    24  $    25  %    26  &    27  '
     28  (    29  )    2a  *    2b  +    2c  ,    2d  -    2e  .    2f  /
     30  0    31  1    32  2    33  3    34  4    35  5    36  6    37  7
     38  8    39  9    3a  :    3b  ;    3c  <    3d  =    3e  >    3f  ?
     40  @    41  A    42  B    43  C    44  D    45  E    46  F    47  G
     48  H    49  I    4a  J    4b  K    4c  L    4d  M    4e  N    4f  O
     50  P    51  Q    52  R    53  S    54  T    55  U    56  V    57  W
     58  X    59  Y    5a  Z    5b  [    5c  \    5d  ]    5e  ^    5f  _
     60  `    61  a    62  b    63  c    64  d    65  e    66  f    67  g
     68  h    69  i    6a  j    6b  k    6c  l    6d  m    6e  n    6f  o
     70  p    71  q    72  r    73  s    74  t    75  u    76  v    77  w
     78  x    79  y    7a  z    7b  {    7c  |    7d  }    7e  ~    7f del


   pchar         = *( unreserved / pct-encoded / sub-delims / ":" / "@")

   query         = *( pchar / "/" / "?" )

   fragment      = *( pchar / "/" / "?" )

   pct-encoded   = "%" HEXDIG HEXDIG

   unreserved    = ALPHA / DIGIT / "-" / "." / "_" / "~"

   reserved      = gen-delims / sub-delims

   gen-delims    = ":" / "/" / "?" / "#" / "[" / "]" / "@"

   sub-delims    = "!" / "$" / "&" / "'" / "(" / ")"
                 / "*" / "+" / "," / ";" / "="
```