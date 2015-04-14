[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Ruby Code Examples ##

It is expected that there could be an OpenHandle Ruby library but in the meantime OpenHandle can be accessed straightforwardly from native Ruby code.

Here's how a native Ruby program (contained in file "[handler.rb](http://nurture.nature.com/tony/openhandle/code/ruby/handler.rb)") can grab a handle data record:

```
% cat handler.rb
#!/usr/bin/env ruby

require 'rubygems'    # only if "json" lib installed as gem
require 'json'
require 'open-uri'

OPENHANDLE = "http://nascent.nature.com/openhandle/handle"

uri = URI.parse("#{OPENHANDLE}?id=#{ARGV[0]}&format=json")
json = uri.open.read

h = JSON.parse(json)

values = h['handleValues']
s = "The handle <#{h['handle']}>"
s += " has #{values.length} values:\n"

n_value = 0

values.each do |value|

  n_value += 1

  s += "\nvalue \##{n_value}:\n"
  s += "  index = #{value['index']}\n"
  s += "  type = #{value['type']}\n"
  if value['data'].class == String
    s += "  data = #{value['data']}\n"
  else
    s += "  data = [#{value['data'].class}]\n"
  end
  s += "  permission = #{value['permission']}\n"
  s += "  ttl = #{value['ttl'].to_i / 3600} hours\n"
  s += "  timestamp = #{value['timestamp']}\n"
  s += "  reference = [#{value['reference'].class}]\n"

end
puts s

__END__

```

This can be run as:
```
% ruby handler.rb 10100/nature
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = [Hash]
  permission = 1110
  ttl = 24 hours
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [Array]

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = 24 hours
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [Array]
```

Or, alternatively, selecting only certain fields:
```
% ruby handler.rb 10.2277/0521853575 | grep data 
  data = http://prp.contentdirections.com/mr/cupress.jsp/doi=10.2277/0521853575
  data = Odysseus Unbound
  data = Full details about this book=http://www.cambridge.org/catalogue/catalogue.asp?isbn=0521853575&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = View Samples from this book=#
  data = Table of contents=http://www.cambridge.org/catalogue/catalogue.asp?isbn=0521853575&ss=toc&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Excerpt=http://www.cambridge.org/catalogue/catalogue.asp?isbn=0521853575&ss=exc&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Index=http://www.cambridge.org/catalogue/catalogue.asp?isbn=0521853575&ss=ind&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Copyright=http://www.cambridge.org/catalogue/catalogue.asp?isbn=0521853575&ss=cop&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Frontmatter=http://www.cambridge.org/catalogue/catalogue.asp?isbn=0521853575&ss=fro&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Buy this book=#
  data = Customers in the UK, Europe, the Middle East and Africa!=!http://www.cambridge.org/uk/catalogue/AddToBasket.asp?qty=1&isbn=0521853575&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Customers in the Americas=http://www.cambridge.org/us/catalogue/AddToBasket.asp?qty=1&isbn=0521853575&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Customers in Australia and New Zealand!=!http://www.cambridge.org/aus/catalogue/AddToBasket.asp?qty=1&isbn=0521853575&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Related subjects=#
  data = Archaeology!=!http://www.cambridge.org/browse/browse_all.asp?subjectid=223&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Classical Archaeology!=!http://www.cambridge.org/browse/browse_all.asp?subjectid=228&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Classical Languages &amp; Literature!=!http://www.cambridge.org/browse/browse_all.asp?subjectid=1009104&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = European Archaeology!=!http://www.cambridge.org/browse/browse_all.asp?subjectid=1008828&utm_source=DOI&utm_medium=MultiLink&utm_content=0521853575&utm_campaign=CDI
  data = Related websites=#
  data = Feature site!=!http://www.cambridge.org/bittlestone/
  data = Cambridge University Press=http://www.cambridge.org/
  data = { 1001 1002 { 1003 1004 1005 1006 1007 } 1008 { 1009 1010 1011 } 1012 { 1013 1014 1015 1016 } 1017 { 1018 } 1019 }
  data = [Hash]
  data = [Hash]
  data = hdladmin@contentdirections.com
```

Here's the same thing (contained in file "[handler1.rb](http://nurture.nature.com/tony/openhandle/code/ruby/handler1.rb)") which this time merely dumps the parsed JSON string.
```
% cat handler1.rb
#!/usr/bin/env ruby

require 'rubygems'    # only if "json" lib installed as gem
require 'json'
require 'open-uri'

OPENHANDLE = "http://nascent.nature.com/openhandle/handle"

uri = URI.parse("#{OPENHANDLE}?id=#{ARGV[0]}&format=json")
json = uri.open.read

h = JSON.parse(json)

p h

__END__
```

And this gives:
```
% ruby handler1.rb 10100/nature
{"handle"=>"hdl:10100/nature", "comment"=>"OpenHandle (JSON) - see
http://openhandle.googlecode.com/", "handleValues"=>[{"reference"=>[], "timestamp"=>"Wed
Feb 28 15:37:06 GMT 2007", "ttl"=>"+86400", "permission"=>"1110", "type"=>"HS_ADMIN",
"index"=>"100", "data"=>{"adminPermission"=>"111111111111",
"adminRef"=>"hdl:10100/nature?index=100"}}, {"reference"=>[], "timestamp"=>"Wed Feb 28
15:37:06 GMT 2007", "ttl"=>"+86400", "permission"=>"1110", "type"=>"URL", "index"=>"1",
"data"=>"http://www.nature.com/"}]}
```

A fuller version of the handler (contained in file "[handler2.rb](http://nurture.nature.com/tony/openhandle/code/ruby/handler2.rb)") with support for querystrings is here:
```
% cat handler2.rb
#!/usr/bin/env ruby

require 'rubygems'    # only if "json" lib installed as gem
require 'json'
require 'open-uri'

OPENHANDLE = "http://nascent.nature.com/openhandle/handle"

handle, querystring = ARGV[0].split('?')
handle.sub!(/^hdl:/, "")
args = {}
if querystring
    querystring.split('&').each do |arg|
        key, val = arg.split('=')
        if args.has_key?(key)
            args[key] << val
        else
            args[key] = [ val ]
        end
    end
end

begin
    uri = URI.parse(OPENHANDLE + '?' + URI.encode("id=#{handle}&format=json"))
    json = uri.open.read
    
    h = JSON.parse(json)
    
    values = h['handleValues']
    n_value = 0
    s = ""
 
    values.each do |value|
    
        n_value += 1
    
        skip = false

        if args.length > 0
            skip = true
            args.keys.each do |key|
                if value.has_key?(key)
                    args[key].each do |val|
                        skip = false if value[key].to_s =~ /^#{val}$/i
                    end
                end
            end
        end

        next if skip

        s += "\nvalue \##{n_value}:\n"
        s += "  index = #{value['index']}\n"
        s += "  type = #{value['type']}\n"
        if value['data'].class == String
            s += "  data = #{value['data']}\n"
        else
            s += "  data = [#{value['data'].class}]\n"
        end
        s += "  permission = #{value['permission']}\n"
        s += "  ttl = #{value['ttl'].to_i / 3600} hours\n"
        s += "  timestamp = #{value['timestamp']}\n"
        s += "  reference = [#{value['reference'].class}]\n"
    
    end
    s1 = "The handle <hdl:#{handle}"
    s1 += "?#{querystring}" unless querystring.nil?
    s1 += "> has #{n_value} values (of #{values.length}):\n"
    puts s1, s

rescue => e

    puts "! Error reading handle \"#{handle}\""
    puts "#{$!.inspect}\n\n"
    puts "#{e.backtrace.join("\n")}\n\n"

end

__END__
{
    "comment" : "OpenHandle (JSON) - see http://openhandle.googlecode.com/" ,
    "handle" : "hdl:10100/nature" ,
    "handleValues" : [
        {
            "index" : "100" ,
            "type" : "HS_ADMIN" ,
            "data" : {
                "adminRef" : "hdl:10100/nature?index=100" ,
                "adminPermission" : "111111111111"
            } ,
            "permission" : "1110" ,
            "ttl" : "+86400" ,
            "timestamp" : "Wed Feb 28 15:37:06 GMT 2007" ,
            "reference"  : {}
        } ,
        {
            "index" : "1" ,
            "type" : "URL" ,
            "data" : "http://www.nature.com/" ,
            "permission" : "1110" ,
            "ttl" : "+86400" ,
            "timestamp" : "Wed Feb 28 15:37:06 GMT 2007" ,
            "reference"  : {}
        }
    ]
}
```

And this can be invoked as
```
% handler.rb '10100/10.1038/nature06264?type=xmp'
The handle <hdl:10100/10.1038/nature06264?type=xmp> has 1 values (of 3):

value #2:
  index = 0
  type = XMP
  data = http://nurture.nature.com/xmp/nature/n7169/xmp/nature06264.xmp
  permission = 1110
  ttl = 24 hours
  timestamp = Sun Mar 09 16:49:52 GMT 2008
  reference = [Array]
```