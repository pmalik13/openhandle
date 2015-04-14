## Text API ##

Section 4 of the [Technical Manual](http://www.handle.net/tech_manual/Handle_Technical_Manual.pdf|Handle) describes a simple batch-oriented command-line interface to admin Handle. This covers all Handle Value fields, apart from "reference", i.e. it accounts for 6 of the 7 fields within a Handle Value. This interface is implemented in the distributed batch app `net.handle.apps.batch.GenericBatch`.

An example batch file (where the secret key is "`******`"):
```
AUTHENTICATE SECKEY:300:10100/admin
******

DELETE 10100/nature
CREATE 10100/nature
100 HS_ADMIN 86400 1110 ADMIN 200:111111111111:0.NA/10100
1 URL 86400 1110 UTF8 http://www.nature.com/

```

The API be summarized as below, where the recognized command-line keywords are followed by a usage template, with '`BLANK-LINE`' being a blank line, and the '`+`' character symbolizing that the argument can be repeated on its own new line):

```
=CREATE=
  CREATE handle
  value+
  BLANK-LINE

=DELETE=
  DELETE handle

=HOME=
  HOME address:port:protocol
  handle+
  BLANK-LINE

=UNHOME=
  UNHOME address:port:protocol
  handle+
  BLANK-LINE

=ADD=
  ADD handle
  value+
  BLANK-LINE

=REMOVE=
  REMOVE indexes:handle

=MODIFY=
  MODIFY handle
  value+
  BLANK-LINE

=AUTHENTICATE=
  AUTHENTICATE SECKEY:adm_index:adm_handle
  password
  LINE
  AUTHENTICATE PUBKEY:adm_index:adm_handle
  key_file_path|passphrase
  LINE
  AUTHENTICATE PUBKEY:adm_index:adm_handle
  key_file_path
  BLANK-LINE

=SESSIONSETUP=
  SESSIONSETUP
  USESESSION:session_on_or_off_flag
  PUBEXNGKEYFILE:public_exchange_key_file
  PUBEXNGKEYREF:pub_exchange_key_ref_index:pub_exchange_key_ref_handle
  PRIVEXNGKEYFILE:private_exchange_key_file
  PASSPHRASE:pass_phrase_for_private_exchange_key
  OPTIONS:<encrypt><authenticate><fallback on challenge response>
  TIMEOUT:time_out_in_hours
  BLANK-LINE
```