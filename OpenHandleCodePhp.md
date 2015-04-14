[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle PHP Code Examples ##

Here's how a native PHP program (contained in file "[handler.php](http://nurture.nature.com/tony/openhandle/code/php/handler.php.txt)") can grab a handle data record:

```
% cat handler.php 
<?php

define('OPENHANDLE', 'http://nascent.nature.com/openhandle/handle');

$params = array(
    'id' => $argv[1],
    'format' => 'json',
);

$h = json_decode(file_get_contents(OPENHANDLE . '?' . http_build_query($params)));

$values = $h->handleValues;
$s = "The handle <$h->handle>";
$s .= " has " . count($values) . " values:\n";

foreach ($values as $value) {
    $n++;
    $s .= "\nvalue #$n:\n";
    $s .= "  index = $value->index\n";
    $s .= "  type = $value->type\n";
    if (get_class($value->data)) {
        $s .= "  data = [" . get_class($value->data) . "]\n";
    } else {
        $s .= "  data = $value->data\n";
    }
    $s .= "  permission = $value->permission\n";
    $s .= "  ttl = " . $value->ttl / 3600 . " hours\n";
    $s .= "  timestamp = $value->timestamp\n";
    $s .= "  reference = [$value->reference]\n";
}
print $s;

?>
```

This can be run as:
```
% php handler.php 10100/nature
The handle <hdl:10100/nature> has 2 values:

value #1:
  index = 100
  type = HS_ADMIN
  data = [stdClass]
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


Here's the same thing (contained in file "[handler1.php](http://nurture.nature.com/tony/openhandle/code/php/handler1.php.txt)") which this time merely dumps the parsed JSON string.
```
% cat handler1.php
<?php

define('OPENHANDLE', 'http://nascent.nature.com/openhandle/handle');

$params = array(
    'id' => $argv[1],
    'format' => 'json',
);

$h = json_decode(file_get_contents(OPENHANDLE . '?' . http_build_query($params)));

print_r($h);

?>
```

And this gives us:
```
% php handler1.php 10100/nature
stdClass Object
(
    [comment] => OpenHandle (JSON) - see http://openhandle.googlecode.com/
    [handle] => hdl:10100/nature
    [handleValues] => Array
        (
            [0] => stdClass Object
                (
                    [index] => 100
                    [type] => HS_ADMIN
                    [data] => stdClass Object
                        (
                            [adminRef] => hdl:10100/nature?index=100
                            [adminPermission] => 111111111111
                        )

                    [permission] => 1110
                    [ttl] => +86400
                    [timestamp] => Wed Feb 28 15:37:06 GMT 2007
                    [reference] => Array
                        (
                        )

                )

            [1] => stdClass Object
                (
                    [index] => 1
                    [type] => URL
                    [data] => http://www.nature.com/
                    [permission] => 1110
                    [ttl] => +86400
                    [timestamp] => Wed Feb 28 15:37:06 GMT 2007
                    [reference] => Array
                        (
                        )

                )

        )

)
```