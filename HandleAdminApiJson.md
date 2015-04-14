## JSON API ##

A simple text-based API for Handle admin could be JSON, whereby a JSON admin object is sent to a server and a JSON response received. The inspiration for this work comes from [Freebase](http://freebase.com) and its associated [Metaweb Query Language](http://freebase.com/view/9202a8c04000641f800000000544e13e) which allows for a JSON query/result exchange.

Such a JSON request could look like the following skeleton (here using the JSON compact syntax) which would be used to create a Handle using secret key authentication:

```
{
    "admin" : [
        {
            "operation" : "AUTHENTICATE" ,
            "arguments" : {
                "password" : "secret" ,
                "handle" : "321/admin" ,
                "values" : [ { "index" : "300" } ]
            }
        } ,
        {
            "operation" : "CREATE" ,
            "arguments" : {
                "handle" : "1234/567" ,
                "handleValues" : [
                    {
                        "index" : "100" ,
                        "type" : "HS_ADMIN" ,
                        "data" : {
                             "adminPermission" : "11111111111" ,
                             "adminRef" : "hdl:0.NA/10101?index=200"
                         } ,
                        "ttl" : "+86400" ,
                        "timestamp" : "" ,
                        "permission" : "1110" ,
                        "reference" : []
                    } ,
                    {
                        "index" : "1" ,
                        "type" : "URL" ,
                        "data" : "http://apple.com/" ,
                        "ttl" : "+86400" ,
                        "timestamp" : "" ,
                        "permission" : "1110" ,
                        "reference" : []
                    }
                ] 
            }
        }
    ]
}
```
A similar format could be used for selective resolution:

```
{
    "resolve" : {
        "handle" : "hdl:10100/tony" ,
        "handleValues" : [
            {
                "index" : "100" ,
                "type" : "HS_ADMIN" ,
                "data" : {
                    "adminPermission" : "11111111111" ,
                    "adminRef" : "hdl:0.NA?10101#index=200"
                 } ,
                 "ttl" : "+86400" ,
                 "timestamp" : "" ,
                 "permission" : "1110" ,
                 "reference" : []
            } ,
            {
                "index" : "1" ,
                "type" : "URL" ,
                "data" : "http://apple.com/" ,
                "ttl" : "+86400" ,
                "timestamp" : "" ,
                "permission" : "1110" ,
                "reference" : []
            }
        ] 
    }
}
```

Some noodling code follows:

```
#!/usr/bin/env ruby

require 'rubygems'
require 'json'
require 'time'

$compact = true
$compact = false

OPS = [
    'CREATE', 'DELETE', 'HOME', 'UNHOME', 'ADD', 'REMOVE',
    'MODIFY', 'AUTHENTICATE', 'SESSIONSETUP',
]
AUTH_PARAMS = [
    'SECKEY', 'PUBKEY',
]
SETUP_PARAMS = [
    'SESSIONSETUP', 'USESESSION', 'PUBEXNGKEYFILE',
    'PUBEXNGKEYREF', 'PRIVEXNGKEYFILE', 'PASSPHRASE',
    'OPTIONS', 'TIMEOUT',
]

class Admin

  def initialize(operation, arguments)
    if OPS.include?(operation)
      @operation = operation
      @arguments = arguments
    else
      raise "! operation unrecognized"
    end
  end

end

class HS_ADMIN

  attr_reader :adminPermission, :adminRef
  attr_writer :adminPermission, :adminRef

  def initialize(adminPermission, adminRef)
    @adminPermission = adminPermission
    @adminRef = adminRef
  end

  def to_s
  end

  def from_s(s)
  end

end

class Permission

  attr_reader :adminRead, :adminWrite, :publicRead, :publicWrite
  attr_writer :adminRead, :adminWrite, :publicRead, :publicWrite

  def initialize
    @adminRead = true
    @adminWrite = true
    @publicRead = true
    @publicWrite = false
  end

  def to_s
    s = ""
    s += @adminRead ? "1" : "0"
    s += @adminWrite ? "1" : "0"
    s += @publicRead ? "1" : "0"
    s += @publicWrite ? "1" : "0"
  end

  def from_s(s)
    raise "! Invalid permission string" if s.length != 4
    s.split(//).each do |d|
      case d
        when "0", "1"
        else
          raise "! Invalid permission digit"
      end
    end
    @adminRead = (s[0] == "1"[0])
    @adminWrite = (s[1] == "1"[0])
    @publicRead = (s[2] == "1"[0])
    @publicWrite = (s[3] == "1"[0])
  end

  def to_json
    if $compact
      self.to_s
    else
      self
    end
  end

end

class AdminPermission

  attr_reader :addAdmin, :addHandle, :addNA, :addValue,
              :authorizedRead, :deleteHandle, :deleteNA,
              :deleteValue, :listHandle, :modifyAdmin,
              :modifyValue
  attr_writer :addAdmin, :addHandle, :addNA, :addValue,
              :authorizedRead, :deleteHandle, :deleteNA,
              :deleteValue, :listHandle, :modifyAdmin,
              :modifyValue

  def initialize
    @addAdmin = true
    @addHandle = true
    @addNA = true
    @addValue = true
    @authorizedRead = true
    @deleteHandle = true
    @deleteNA = true
    @deleteValue = true
    @listHandle = true
    @modifyAdmin = true
    @modifyValue = true
    @removeAdmin = true
  end

  def to_s
    s = ""
    s += @addAdmin? "1" : "0"
    s += @addHandle? "1" : "0"
    s += @addNA? "1" : "0"
    s += @addValue? "1" : "0"
    s += @authorizedRead? "1" : "0"
    s += @deleteHandle? "1" : "0"
    s += @deleteNA? "1" : "0"
    s += @deleteValue? "1" : "0"
    s += @listHandle? "1" : "0"
    s += @modifyAdmin? "1" : "0"
    s += @modifyValue? "1" : "0"
  end

  def from_s(s)
    raise "! Invalid permission string" if s.length != 11
    s.split(//).each do |d|
      case d
        when "0", "1"
        else
          raise "! Invalid permission digit"
      end
    end
    @addAdmin = (s[0] == "1"[0])
    @addHandle = (s[1] == "1"[0])
    @addNA = (s[2] == "1"[0])
    @addValue = (s[3] == "1"[0])
    @authorizedRead = (s[4] == "1"[0])
    @deleteHandle = (s[5] == "1"[0])
    @deleteNA = (s[6] == "1"[0])
    @deleteValue = (s[7] == "1"[0])
    @listHandle = (s[8] == "1"[0])
    @modifyAdmin = (s[9] == "1"[0])
    @modifyValue = (s[10] == "1"[0])
  end

end

class  HandleValue

  attr_reader :index, :type, :data, :ttl, :timestamp, :permission, :reference
  attr_writer :index, :type, :data, :ttl, :timestamp, :permission, :reference

  def initialize(index, type, data)
    @index = index
    @type = type
    @data = data
    @ttl = { :ttlType => 0, :ttlValue => 86400 }
    @timestamp = Time.new
    @permission = Permission.new
    @reference = {}
  end

  def to_json
    data = @data
    case @type
      when "HS_ADMIN" then data = {
        }
      when "URL" then data = @data
      else
    end
    if $compact
      permission = @permission.to_s
      timestamp = @timestamp.to_i
    else
      permission = {
        "adminRead" => @permission.adminRead,
        "adminWrite" => @permission.adminWrite,
        "publicRead" => @permission.publicRead,
        "publicWrite" => @permission.publicWrite,
      }
      # timestamp = @timestamp.iso8601
      timestamp = @timestamp.rfc2822
    end
    h = {
        "index" => @index,
        "type" => @type,
        "data" => data,
        "ttl" => @ttl,
        "timestamp" => timestamp,
        "permission" => permission,
        "reference" => @reference,
    }
    JSON.pretty_generate(h, {"object_nl"=> "\n"})
  end
# alias_method :to_json, :to_s

  def to_hcl
    s = @index.to_s
    s += " " + @type
    s += " " + @ttl[:ttlValue].to_s
    s += " " + @permission.to_s
    case @type
      when "HS_ADMIN" then s += " " + "**admin data**"
      when "URL" then s += " UTF8 " + @data
      else
    end
  end

end



f1 = HandleValue.new(12, "URL", "http://apple.com/")
f2 = HandleValue.new(11, "HS_ADMIN", "http://ibm.com/")

$compact = false
puts f1.to_json
puts f1.to_hcl

$compact = true
puts f1.to_json
puts f1.to_hcl

```