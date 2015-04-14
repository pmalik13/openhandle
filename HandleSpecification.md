## Handle Specification ##

There are two sources for the Handle specs:

  * [RFC 3651](http://hdl.handle.net/4263537/4068) (Nov. '03)
  * [HANDLE.NET 6.2](http://handle.net/download.html) (Sep. '07)

The RFC is authoritative but has slipped somewhat with respect to the HANDLE.NET 6.2 Java codebase. The table below attempts to capture some of the disparity between the RFC and the codebase.

| **RFC Section** | **Leave** | **Remove** | **Add** | **Reference** | **Comment** |
|:----------------|:----------|:-----------|:--------|:--------------|:------------|
| [Sect. 3.1](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1), p. 7 | `PUBLIC_EXECUTE` | - | - | [SS (260)](http://www.handle.net/mail-archive/handle-info/msg00237.html) | **Not** implemented - Feature placeholder for web services/grid type apps|
| [Sect. 3.1](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1), p. 7 | `ADMIN_EXECUTE` | - | - | [SS (260)](http://www.handle.net/mail-archive/handle-info/msg00237.html) | **Not** implemented - Feature placeholder for web services/grid type apps |
| [Sect. 3.2.1](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.1), p. 13 | `LIST_NA` | - | - | [SS (260)](http://www.handle.net/mail-archive/handle-info/msg00237.html) | **Not** implemented - Feature placeholder for namespace admin |
| [Sect. 3.2.1](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.1), p. 13 | `HS_ADMIN` | - | - | [TH (230)](http://www.handle.net/mail-archive/handle-info/msg00230.html) | - |
| [Sect. 3.2.2](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.2), p. 14 | `HS_SITE` | - | - | [TH (230)](http://www.handle.net/mail-archive/handle-info/msg00230.html) | - |
| [Sect. 3.2.3](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.3), p. 19 | `HS_NA_DELEGATE` | - | - | [SS (261)](http://www.handle.net/mail-archive/handle-info/msg00261.html) | **Not** implemented - Feature placeholder for handle prefex delegation; **Not** registered in type registry |
| [Sect. 3.2.4](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.4), p. 20 | `HS_SERV` | - | - | [TH (230)](http://www.handle.net/mail-archive/handle-info/msg00230.html) | - |
| [Sect. 3.2.5](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.5), p. 21 | `HS_ALIAS` | - | - | [TH (230)](http://www.handle.net/mail-archive/handle-info/msg00230.html) | - |
| [Sect. 3.2.6](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.6), p. 21 | `HS_PRIMARY` | - | - | [SS (261)](http://www.handle.net/mail-archive/handle-info/msg00261.html) | Unimplemented - Feature placeholder for multiple primary servers; **Not** registered in type registry |
| [Sect. 3.2.7](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.7), p. 22 | `HS_VLIST` | - | - | [TH (230)](http://www.handle.net/mail-archive/handle-info/msg00230.html) | - |
| [Sect. 3.2](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2) | - | - | `HS_SECKEY` | [SR (259)](http://www.handle.net/mail-archive/handle-info/msg00259.html) | Implemented as 'STD\_TYPE' in codebase; Registered in type registry |
| [Sect. 3.2](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2) | - | - | `HS_PUBKEY` | [SR (259)](http://www.handle.net/mail-archive/handle-info/msg00259.html) | Implemented as 'STD\_TYPE' in codebase; Registered in type registry |
| [Sect. 3.2](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2) | - | (`HS_DSAPUBKEY`) | -  | [SR (259)](http://www.handle.net/mail-archive/handle-info/msg00259.html) | Implemented as 'STD\_TYPE' in codebase - **to be removed**; **Not** registered in type registry |
| [Sect. 3.2](http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2) | - | - | `HS_NAMESPACE` | [SR (259)](http://www.handle.net/mail-archive/handle-info/msg00259.html) | **Not** implemented as 'STD\_TYPE' in codebase; Registered in type registry |
| [Sect. 2](http://www.apps.ietf.org/rfc/rfc3651.html#sec-2), p. 4 | - | - | URI Spec. | [TH (244)](http://www.handle.net/mail-archive/handle-info/msg00244.html) | Add subsection to Sect. 2 detailing URI scheme |