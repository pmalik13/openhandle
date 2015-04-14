## Handle Data Model ##

Essentially a handle record is a set of values. Each value is a tuple with 7 fields: index, type, data, ttl, permission, timestamp, reference. See [RFC 3651](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1) for details.

| **handle** | **[index](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** § | **[type](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** §1 | **[data](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** §2 | **[ttl](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** | **[permission](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** | **[timestamp](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** § | **[reference](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1)** |
|:-----------|:------------------------------------------------------------------|:------------------------------------------------------------------|:------------------------------------------------------------------|:-------------------------------------------------------------|:--------------------------------------------------------------------|:----------------------------------------------------------------------|:-------------------------------------------------------------------|
|  | 1 | URL | http://... | {...} | {...} | ? ms | {...} |
|  | 100 | ADMIN | {...} | {...} | {...} | ? ms | {...} |

§ Simple values.

§1 'Simple' value UTF8 string - length plus string.

§2 'Simple' value UTF8 string for **some** user types (e.g. 'URL') - length plus string.

Values marked '`{...}`' above are complex values.