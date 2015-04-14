## OpenHandle Compact Forms ##

(This is a very rough draft.)

The Handle serializations can be verbose. It is especially noticeable that the admin fields of a handle value are structured elements and are suitable candidates for abbreviation.

A Handle record is composed of values each subdivided into fields - see HandleDataModel.

  * user/admin fields
  * simple/complex fields

Handle value fields

  * Permission
  * TTL
  * Timestamp
  * References - Handle Value Refs

Handle data predefined types

  * HS\_ADMIN
    * handle value ref
    * perms


### Permission ###

```
      "permission" : {
        "adminRead" : "true" ,
        "adminWrite" : "true" ,
        "publicRead" : "true" ,	
        "publicWrite" : "false"
      } ,
```

```
      "permission" : "1110" ,
```

### TTL ###

```
	"ttl" : {
		"ttlType" : "0" ,
		"ttlValue" : "86400" ,
	}
```

```
	"ttl" : "+86400" 
```

### Timestamp ###

```
      "timestamp" : "20080310T05:56:00.123Z" , 
```

```
      "timestamp" : "20080310T05:56:00.123Z" , 
```

### Reference ###

```
    "reference" : {
        "referenceCount" : "2" ,
        "referenceList" : [
            {
                "handle" : "1234/567" ,
                "handleValueIndex" : "200" ,
            } ,
            {
                "handle" : "1234/567" ,
                "handleValueIndex" : "201" ,
            }
        ]
    }
```

```
    "reference" : [
        "hdl:1234/567?index=200" ,
        "hdl:1234/567?index=201" 
    ]
```