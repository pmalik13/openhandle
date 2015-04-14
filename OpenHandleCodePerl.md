[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Perl Code Examples ##

Here's how a native Perl program (contained in file "[handler.pl](http://nurture.nature.com/tony/openhandle/code/perl/handler.pl)") can grab a handle data record:

```
% cat handler.pl
#!/usr/bin/env perl

use LWP::Simple;
use JSON;

$OPENHANDLE = "http://nascent.nature.com/openhandle/handle";

$json = get($OPENHANDLE . "?id=$ARGV[0]&format=json");
$h = from_json($json);

@values = @{$h->{'handleValues'}};
$s = "The handle <$h->{'handle'}>";
$s .= " has " . eval($#values + 1) . " values:\n";

for $value (@values) {

    $s .= "\nvalue \#" . ++$n . ":\n";
    $s .= "  index = $value->{'index'}\n";
    $s .= "  type = $value->{'type'}\n";
    if (ref($value->{'data'})) {
        $s .= "  data = [" . ref($value->{'data'}) . "]\n";
    } else {
        $s .= "  data = $value->{'data'}\n";
    }
    $s .= "  permission = $value->{'permission'}\n";
    $s .= "  ttl = " . eval($value->{'ttl'} / 3600) . " hours\n";
    $s .= "  timestamp = $value->{'timestamp'}\n";
    $s .= "  reference = [" . ref($value->{'reference'}) . "]\n";

}
print $s;

__END__
```

This can be run as:
```
% perl handler.pl 10100/nature
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = [HASH]
  permission = 1110
  ttl = 24 hours
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [ARRAY]

value #2:
  index = 1
  type = URL
  data = http://www.nature.com/
  permission = 1110
  ttl = 24 hours
  timestamp = Wed Feb 28 15:37:06 GMT 2007
  reference = [ARRAY]
```

Here's the same thing (contained in file "[handler1.pl](http://nurture.nature.com/tony/openhandle/code/perl/handler1.pl)") which this time merely dumps the parsed JSON string.
```
% cat handler1.pl
#!/usr/bin/env perl

use Data::Dumper;
use LWP::Simple;
use JSON;

$OPENHANDLE = "http://nascent.nature.com/openhandle/handle";

$json = get($OPENHANDLE . "?id=$ARGV[0]&format=json");
$h = from_json($json);

print Dumper($h);

__END__
```


And here's the dump:
```
% perl handler1.pl 10.1038/nature05932
$VAR1 = {
          'handle' => 'hdl:10100/nature',
          'comment' => 'OpenHandle (JSON) - see http://openhandle.googlecode.com/',
          'handleValues' => [
                              {
                                'index' => '100',
                                'reference' => [],
                                'timestamp' => 'Wed Feb 28 15:37:06 GMT 2007',
                                'ttl' => '+86400',
                                'permission' => '1110',
                                'data' => {
                                            'adminRef' => 'hdl:10100/nature?index=100',
                                            'adminPermission' => '111111111111'
                                          },
                                'type' => 'HS_ADMIN'
                              },
                              {
                                'index' => '1',
                                'reference' => [],
                                'timestamp' => 'Wed Feb 28 15:37:06 GMT 2007',
                                'ttl' => '+86400',
                                'permission' => '1110',
                                'data' => 'http://www.nature.com/',
                                'type' => 'URL'
                              }
                            ]
        };
```