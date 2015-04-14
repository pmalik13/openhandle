## Handle RDF Schema ##

(This page needs elaboration. The RDFS is included here for convenience. The current schema can be downloaded from [here](http://openhandle.googlecode.com/files/handle.rdfs).)

```
<?xml version="1.0" encoding="UTF-8"?>

<!--
########################################################################
##  Handle System - RDF Schema for Handle Data Model
##
##  This document presents an RDF Schema for the Handle System - see
##  RFC 3651 [1]. This is the authoritative reference for the Handle
##  Data Model. For ease of linking, however, this schema refers to
##  the HTML version of the RFC [2]. For further information on the
##  Handle System, see [3].
##
##  Author: Tony Hammond <t.hammond@nature.com>
##  Date:   2008-02-08T16:40:00Z
##
##  ==
##  [1] http://www.ietf.org/rfc/rfc3651.txt
##  [2] http://www.apps.ietf.org/rfc/rfc3651.html
##  [3] http://wwww.handle.net/
## 
########################################################################
##
##  The HS data model is defined in Sect. 3 of RFC 3651 [1]. This schema
##  follows the order of presentation in the RFC by section. Classes are
##  given first followed by properties, ordered by object properties
##  followed by datatype properties.
##
##  3. Handle System Data Model [2a]
##  3.1. Handle Value Set [2b]
##  3.2. Pre-defined Handle Data Types [2c]
##  3.2.1. Handle Administrator: HS_ADMIN [2d]
##  3.2.2. Service Site Information: HS_SITE [2e]
##  3.2.3. Naming Authority Delegation Service: HS_NA_DELEGATE [2f]
##  3.2.4. Service Handle: HS_SERV [2g]
##  3.2.5. Alias Handle: HS_ALIAS [2h]
##  3.2.6. Primary Site: HS_PRIMARY [2i]
##  3.2.7. Handle Value List: HS_VLIST [2j]
##
##  [2a] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3
##  [2b] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1
##  [2c] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2
##  [2d] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.1
##  [2e] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.2
##  [2f] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.3
##  [2g] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.4
##  [2h] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.5
##  [2i] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.6
##  [2j] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.7
##
########################################################################
-->

<!DOCTYPE schema [
  <!ENTITY rdf "http://www.w3.org/1999/02/22-rdf-syntax-ns#">
  <!ENTITY rdfs "http://www.w3.org/2000/01/rdf-schema#">
  <!ENTITY owl "http://www.w3.org/2002/07/owl#">
  <!ENTITY xsd "http://www.w3.org/2000/10/XMLSchema#">
]>

<r:RDF
  xmlns:r="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
  xmlns:s="http://www.w3.org/2000/01/rdf-schema#"
  xmlns:o="http://www.w3.org/2002/07/owl#"
  xml:base="http://nascent.nature.com/schemas/handle.rdfs"
>

<!--
########################################################################
##  3.1. Handle Value Set (p. 4) [2b]
##
##  [2b] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.1
##
##
##  Figure 3.1: Handle "10.1045/may99-payette" and its set of values
##
##                  Handle "10.1045/may99-payette"
##
##                              |
##                              |
##                              V
##
##      _____________________________________________________________
##     |        <index>:            3                                |
##    _____________________________________________________________  |
##   |        <index>:            2                                | |
##  _____________________________________________________________  | |
##  |                                                             | | |
##  |  <index>:           1                                       | | |
##  |  <type>:            URL                                     | | |
##  |  <data>:            http://www.dlib.org/dlib...             | | |
##  |  <TTL>:             {Relative: 24 hours}                    | | |
##  |  <permission>:      PUBLIC_READ, ADMIN_WRITE                | | |
##  |  <timestamp>:       927314334000                            | | |
##  |  <reference>:       {empty}                                 | |-
##  |                                                             |-
##   _____________________________________________________________
##
########################################################################
-->

<!--
  Classes
-->

<!-- "Handle" class -->
<s:Class r:ID="Handle">
  <s:label>Handle</s:label>
  <s:comment>A handle is a globally unique name for an Internet
	resource. Handles are character strings that consist of two parts:
	a naming authority, followed by a unique local name under the naming
	authority.</s:comment>
</s:Class>

<!-- "HandleValues" class -->
<s:Class r:ID="HandleValues">
  <s:label>HandleValues</s:label>
  <s:comment>A set of values assigned to a handle.</s:comment>
  <s:subClassOf r:resource="&rdf;Collection"/>
</s:Class>

<!-- "HandleValue" class -->
<s:Class r:ID="HandleValue">
  <s:label>HandleValue</s:label>
  <s:comment>A handle value is a record that consists of a set of data
	fields: "Index", "Type", "Data", "TTL", "Permission", "Timestamp",
	"Reference".</s:comment>
</s:Class>

<!-- "HandleReference" class -->
<s:Class r:ID="HandleReference">
  <s:label>HandleReference</s:label>
  <s:comment>A handle reference refers to another handle value in terms
	 of a UTF8-string and a 4-byte integer (where the UTF8-string is the
	 handle name and the integer is the value index). For convenience a
	 handle reference is implemented as a URI-ref using the "info:" URI
	 scheme, where the handle value index is referenced in the fragment
	 identifier. For example, a handle reference to the handle
	 "10.1045/may99-payette" with index value "100" will be implemented
	 as the URI "info:hdl/10.1045/may99-payette#index=100".</s:comment>
  <s:subClassOf r:resource="&xsd;anyURI"/>
</s:Class>

<!-- "Reference" class -->
<s:Class r:ID="Reference">
  <s:label>Reference</s:label>
  <s:comment></s:comment>
</s:Class>

<!-- "ReferenceList" class -->
<s:Class r:ID="ReferenceList">
  <s:label>ReferenceList</s:label>
  <s:comment></s:comment>
  <s:subClassOf r:resource="&rdf;Collection"/>
</s:Class>

<!--
  Object Properties
-->

<!-- "handleValues" property -->
<r:Property r:ID="handleValues">
  <s:label>handleValues</s:label>
  <s:comment>Property attributing a "HandleValues" object.</s:comment>
  <s:domain r:resource="#Handle"/>
  <s:range r:resource="#HandleValues"/>
</r:Property>

<!-- "handleValue" property -->
<r:Property r:ID="handleValue">
  <s:label>handleValue</s:label>
  <s:comment>Property attributing a "HandleValue" object.</s:comment>
  <s:domain r:resource="#HandleValues"/>
  <s:range r:resource="#HandleValue"/>
</r:Property>

<!-- "handleReference" property -->
<r:Property r:ID="handleReference">
  <s:label>handleReference</s:label>
  <s:comment>Property attributing a "HandleReference"
	object.</s:comment>
  <s:domain r:resource="#ReferenceList"/>
  <s:range r:resource="&xsd;anyURI"/>
</r:Property>

<!--
  Datatype Properties
-->

<!-- "index" property -->
<r:Property r:ID="index">
  <s:label>index</s:label>
  <s:comment>Property attributing an "Index" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "type" property -->
<r:Property r:ID="type">
  <s:label>type</s:label>
  <s:comment>Property attributing a "Type" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <s:range r:resource="&xsd;string"/>
</r:Property>

<!-- "data" property -->
<r:Property r:ID="data">
  <s:label>data</s:label>
  <s:comment>Property attributing a "Data" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <!-- Range for generic data types -->
  <s:range r:resource="&xsd;string"/>
  <!-- Ranges for pre-defined data types -->
  <s:range r:resource="#HS_ADMIN"/>
  <s:range r:resource="#HS_SITE"/>
  <s:range r:resource="#HS_NA_DELEGATE"/>
  <s:range r:resource="#HS_SERV"/>
  <s:range r:resource="#HS_ALIAS"/>
  <s:range r:resource="#HS_PRIMARY"/>
  <s:range r:resource="#HS_VLIST"/>
</r:Property>

<!-- "ttl" property -->
<r:Property r:ID="ttl">
  <s:label>ttl</s:label>
  <s:comment>Property attributing a "TTL" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <s:range r:resource="#TTL"/>
</r:Property>

<!-- "permission" property -->
<r:Property r:ID="permission">
  <s:label>permission</s:label>
  <s:comment>Property attributing a "Permission" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <s:range r:resource="#Permission"/>
</r:Property>

<!-- "timestamp" property -->
<r:Property r:ID="timestamp">
  <s:label>timestamp</s:label>
  <s:comment>Property attributing a "Timestamp" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <s:range r:resource="&xsd;dateTime"/>
</r:Property>

<!-- "reference" property -->
<r:Property r:ID="reference">
  <s:label>reference</s:label>
  <s:comment>Property attributing a "Reference" field.</s:comment>
  <s:domain r:resource="#HandleValue"/>
  <s:range r:resource="#Reference"/>
</r:Property>

<!--
  TTL properties
-->

<!-- "ttlType" property -->
<r:Property r:ID="ttlType">
  <s:label>ttlType</s:label>
  <s:comment>Property attributing a "TTL" type.</s:comment>
  <s:domain r:resource="#TTL"/>
  <s:range r:resource="&xsd;token"/>
</r:Property>

<!-- "ttlValue" property -->
<r:Property r:ID="ttlValue">
  <s:label>ttlValue</s:label>
  <s:comment>Property attributing a "TTL" value.</s:comment>
  <s:domain r:resource="#TTL"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!--
  Permission properties
-->

<!-- "publicWrite" property -->
<r:Property r:ID="publicWrite">
  <s:label>publicWrite</s:label>
  <s:comment>Property attributing a "PUBLIC_WRITE" permission. This
	 permission permission allows anyone to modify or delete the handle
	 value.</s:comment>
  <s:domain r:resource="#Permission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "publicRead" property -->
<r:Property r:ID="publicRead">
  <s:label>publicRead</s:label>
  <s:comment>Property attributing a "PUBLIC_READ" permission. This
	 permission allows anyone to read the handle value.</s:comment>
  <s:domain r:resource="#Permission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "adminWrite" property -->
<r:Property r:ID="adminWrite">
  <s:label>adminWrite</s:label>
  <s:comment>Property attributing a "ADMIN_WRITE" permission. This
	 permission allows any handle administrator to update or delete
	 the handle value.</s:comment>
  <s:domain r:resource="#Permission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "adminRead" property -->
<r:Property r:ID="adminRead">
  <s:label>adminRead</s:label>
  <s:comment>Property attributing a "ADMIN_READ" permission. This
	 permission allows the handle value to be read by any handle
	 administrator with AUTHORITIVE_READ privilege.</s:comment>
  <s:domain r:resource="#Permission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- ** Deprecated Property: Included in RFC 3651 but never
        implemented. To be written out of RFC revision.  ** -->
<!-- "publicExecute" property -->
<r:Property r:ID="publicExecute">
  <s:label>publicExecute</s:label>
  <s:comment>** Deprecated Property: Included in RFC 3651 but never
	 implemented. To be written out of RFC revision. **</s:comment>
  <s:comment>Property attributing a "PUBLIC_EXECUTE" permission. This
	 ermission allows anyone to execute the program identified by the
	 handle value on the handle host as anonymous user.</s:comment>
  <s:domain r:resource="#Permission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- ** Deprecated Property: Included in RFC 3651 but never
        implemented. To be written out of RFC revision.  ** -->
<!-- "adminExecute" property -->
<r:Property r:ID="adminExecute">
  <s:label>adminExecute</s:label>
  <s:comment>** Deprecated Property: Included in RFC 3651 but never
	 implemented. To be written out of RFC revision. **</s:comment>
  <s:comment>Property attributing a "ADMIN_EXECUTE" permission. This
	 permission allows handle administrator(s) to run the program
	 identified by the handle value on the handle server.</s:comment>
  <s:domain r:resource="#Permission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!--
  Reference properties
-->

<!-- "referenceCount" property -->
<r:Property r:ID="referenceCount">
  <s:label>referenceCount</s:label>
  <s:comment>Property attributing a "ReferenceCount" value.</s:comment>
  <s:domain r:resource="#Reference"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "referenceList" property -->
<r:Property r:ID="referenceList">
  <s:label>referenceList</s:label>
  <s:comment>Property attributing a "ReferenceList" object.</s:comment>
  <s:domain r:resource="#Reference"/>
  <s:range r:resource="#ReferenceList"/>
</r:Property>

<!--
########################################################################
##  3.2. Pre-defined Handle Data Types (p. 9) [2c]
##
##  [2c] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2
########################################################################
-->

<!--
########################################################################
##  3.2.1. Handle Administrator: HS_ADMIN (p. 10) [2d] 
##
##  [2d] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.1
##
##  Figure 3.2.1: Administrator for the naming authority
##                handle "0.NA/10"
##
##       _____________________________________________________________
##     _____________________________________________________________  |
##    _____________________________________________________________ | |
##  |                                                             | | |
##  |  <index>:       2                                           | | |
##  |  <type>:        HS_ADMIN                                    | | |
##  |  <data>:                                                    | | |
##  |    <AdminRef>:    "0.NA/10": 3                              | | |
##  |    <AdminPerm>:   Add_NA,     Delete_NA,                    | | |
##  |                   Add Handle, Delete_Handle,                | | |
##  |                   Add_Value,  Delete_Value,  Modify_Value,  | | |
##  |                   Authorized_Read, List_Handle, List_NA     | | |
##  |                                                             | | |
##  |  <TTL>:         24 hours                                    | | |
##  |  <permission>:  PUBLIC_READ, ADMIN_WRITE                    | | |
##  |  <reference>:   {empty}                                     | |-
##  |                                                             |-
##   _____________________________________________________________
##
########################################################################
-->

<!--
  Classes - HS_ADMIN
-->

<!-- "HS_ADMIN" class  (pre-defined HS type) -->
<s:Class r:ID="HS_ADMIN">
  <s:label>HS_ADMIN</s:label>
  <s:comment>HS_ADMIN is a pre-defined Handle System data type which is
	 used to assign handle administrators to handles. An HS_ADMIN value
	 is a handle value whose "type" field is HS_ADMIN and whose "data"
	 field consists of the following entries: "AdminRef" - a reference
	 to a handle value, and "AdminPermission" - a 16-bit bit-mask that
	 defines the administration privilege of the set of handle
	 administrators identified by the HS_ADMIN value.</s:comment>
</s:Class>

<!-- "AdminRef" class -->
<s:Class r:ID="AdminRef">
  <s:label>AdminRef</s:label>
  <s:comment></s:comment>
</s:Class>

<!-- "AdminPermission" class -->
<s:Class r:ID="AdminPermission">
  <s:label>AdminPermission</s:label>
  <s:comment></s:comment>
</s:Class>

<!--
  Object Properties - HS_ADMIN
-->

<!-- "adminRef" property -->
<r:Property r:ID="adminRef">
  <s:label>adminRef</s:label>
  <s:comment>Property attributing an "AdminRef" object.</s:comment>
  <s:domain r:resource="#HS_ADMIN"/>
  <s:range r:resource="#AdminRef"/>
</r:Property>

<!-- "adminPermission" property -->
<r:Property r:ID="adminPermission">
  <s:label>adminPermission</s:label>
  <s:comment>Property attributing an "AdminPermission"
	 object.</s:comment>
  <s:domain r:resource="#HS_ADMIN"/>
  <s:range r:resource="#AdminPermission"/>
</r:Property>

<!--
  Datatype Properties - HS_ADMIN
-->

<!-- "addHandle" property -->
<r:Property r:ID="addHandle">
  <s:label>addHandle</s:label>
  <s:comment>Property attributing an "Add_Handle" permission. This
	 permission allows naming authority administrator to create new
	 handles under a given naming authority.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "deleteHandle" property -->
<r:Property r:ID="deleteHandle">
  <s:label>deleteHandle</s:label>
  <s:comment>Property attributing a "Delete_Handle" permission. This
	 permission allows naming authority administrator to delete handles
	 under a given naming authority.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "addNA" property -->
<r:Property r:ID="addNA">
  <s:label>addNA</s:label>
  <s:comment>Property attributing an "Add_NA" permissio. This
	 permission allows the naming authority administrator to create
	 new sub-naming authorities.
</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "deleteNA" property -->
<r:Property r:ID="deleteNA">
  <s:label>deleteNA</s:label>
  <s:comment>Property attributing a "Delete_NA" permission. This
	 permission allows naming authority administrator to delete an
	 existing sub-naming authority.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "modifyValue" property -->
<r:Property r:ID="modifyValue">
  <s:label>modifyValue</s:label>
  <s:comment>Property attributing a "Modify_Value" permission. This
	 permission allows handle administrator to modify any handle values
	 other than HS_ADMIN values. HS_ADMIN values are used to define
	 handle administrators and are managed by a different set of permissions</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "deleteValue" property -->
<r:Property r:ID="deleteValue">
  <s:label>deleteValue</s:label>
  <s:comment>Property attributing a "Delete_Value" permission. This
	 permission allows handle administrator to delete any handle values
	 other than the HS_ADMIN values.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "addValue" property -->
<r:Property r:ID="addValue">
  <s:label>addValue</s:label>
  <s:comment>Property attributing an "Add_Value" permission. This
	 permission allows handle administrator to add handle values other
	 than the HS_ADMIN values.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "modifyAdmin" property -->
<r:Property r:ID="modifyAdmin">
  <s:label>modifyAdmin</s:label>
  <s:comment>Property attributing a "Modify_Admin" permission. This
	 permission allows handle administrator to modify HS_ADMIN
	 values.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "removeAdmin" property -->
<r:Property r:ID="removeAdmin">
  <s:label>removeAdmin</s:label>
  <s:comment>Property attributing a "Remove_Admin" permission. This
	 permission allows handle administrator to remove HS_ADMIN
	 values.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "addAdmin" property -->
<r:Property r:ID="addAdmin">
  <s:label>addAdmin</s:label>
  <s:comment>Property attributing an "Add_Admin" permission. This
	 permission allows handle administrator to add new HS_ADMIN
	 values.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "authorizedRead" property -->
<r:Property r:ID="authorizedRead">
  <s:label>authorizedRead</s:label>
  <s:comment>Property attributing a "Authorized_Read" permission. This
	 permission grants handle administrator read-access to handle
	 values with the ADMIN_READ permission. Administrators without this
	 permission will not have access to handle values that require
	 authentication for read access.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "listHandle" property -->
<r:Property r:ID="listHandle">
  <s:label>listHandle</s:label>
  <s:comment>Property attributing a "LIST_Handle" permission. This
	 permission allows naming authority administrator to list handles
	 under a given naming authority.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- ** Deprecated Property: Included in RFC 3651 but never
        implemented. To be written out of RFC revision.  ** -->
<!-- "listNA" property -->
<r:Property r:ID="listNA">
  <s:label>listNA</s:label>
  <s:comment>** Deprecated Property: Included in RFC 3651 but never
	 implemented. To be written out of RFC revision. **</s:comment>
  <s:comment>Property attributing a "LIST_NA" permission. This
	 permission allows naming authority administrator to list
	 immediate sub-naming authorities under a given naming authority.</s:comment>
  <s:domain r:resource="#AdminPermission"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>


<!--
########################################################################
##  3.2.2. Service Site Information: HS_SITE (p. 14) [2e]
##
##  [2e] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.2
##
##  Fig. 3.2.2: The primary service site for the naming authority "10"
##
##       ___________________________________________________________
##     ___________________________________________________________  |
##   ___________________________________________________________  | |
##  |                                                           | | |
##  | <index>:       2                                          | | |
##  | <type>:        HS_SITE                                    | | |
##  | <data>:                                                   | | |
##  |    Version:           0                                   | | |
##  |    ProtocolVersion:   2.1                                 | | |
##  |    SerialNumber:      1                                   | | |
##  |    PrimaryMask:                                           | | |
##  |        MultiPrimary:    FALSE                             | | |
##  |        PrimarySite:     TRUE                              | | |
##  |    HashOption:        HASH_BY_HANDLE                      | | |
##  |    HashFilter:        {empty UTF8-String}                 | | |
##  |    AttributeList:     0    {followed by no attributes}    | | |
##  |    NumOfServer:       3                                   | | |
##  |         {followed by a list of <ServerRecord>}            | | |
##  |                                                           | | |
##  |         _________________________________________         | | |
##  |       __________________________________________ |        | | |
##  |      __________________________________________ ||        | | |
##  |     | ServerID:        1                       |||        | | |
##  |     | Address:         :FFFF:132.151.1.155     |||        | | |
##  |     | PublicKeyRecord: HS_DSAKEY, iQCuR2R...   |||        | | |
##  |     | ServiceInterface                         |||        | | |
##  |     |    ServiceType:          Resolution_Only |||        | | |
##  |     |    TransmissionProtocol: TCP & UDP       |||        | | |
##  |     |    PortNumber:           2641            |||        | | |
##  |     |                                          |||        | | |
##  |     |    ServiceType:          Admin only      |||        | | |
##  |     |    TransmissionProtocol: TCP             ||         | | |
##  |     |    PortNumber:           2642            |          | | |
##  |      __________________________________________           | | |
##  |                                                           | | |
##  |  <TTL>:        24 hours                                   | | |
##  |  <permission>: PUBLIC_READ, ADMIN_WRITE                   | | |
##  |  <reference>:  {empty}                                    | |-
##  |                                                           |-
##   ___________________________________________________________
##
########################################################################
-->

<!--
  Classes - HS_SITE
-->

<!-- "HS_SITE" class  (pre-defined HS type) -->
<s:Class r:ID="HS_SITE">
  <s:label>HS_SITE</s:label>
  <s:comment>HS_SITE is a pre-defined Handle System data type which is
	 used to define a service site. An HS_SITE value defines a service
	 site by identifying the server computers (e.g., IP addresses) that
	 comprise the site along with their service configurations (e.g.,
	 port numbers).</s:comment>
</s:Class>

<!-- "PrimaryMask" class -->
<s:Class r:ID="PrimaryMask">
  <s:label>PrimaryMask</s:label>
  <s:comment></s:comment>
</s:Class>

<!-- "AttributeList" class -->
<s:Class r:ID="AttributeList">
  <s:label>AttributeList</s:label>
  <s:comment>A 4-byte integer followed by a list of UTF8-string pairs.</s:comment>
</s:Class>

<!-- "AttributePair" class -->
<s:Class r:ID="AttributePair">
  <s:label>AttributePair</s:label>
  <s:comment>A UTF8-string pair is an "Attribute":"Value"
	 pair.</s:comment>
</s:Class>

<!-- "ServerRecords" class -->
<s:Class r:ID="ServerRecords">
  <s:label>ServerRecords</s:label>
  <s:comment></s:comment>
  <s:subClassOf r:resource="&rdf;Collection"/>
</s:Class>

<!-- "ServerRecord" class -->
<s:Class r:ID="ServerRecord">
  <s:label>ServerRecord</s:label>
  <s:comment></s:comment>
</s:Class>

<!-- "ServiceInterface" class -->
<s:Class r:ID="ServiceInterface">
  <s:label>ServiceInterface</s:label>
  <s:comment></s:comment>
</s:Class>

<!--
  Object Properties - HS_SITE
-->

<!-- "primaryMask" property -->
<r:Property r:ID="primaryMask">
  <s:label>primaryMask</s:label>
  <s:comment>Property attributing a "PrimaryMask" object.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="#PrimaryMask"/>
</r:Property>

<!-- "serverRecords" property -->
<r:Property r:ID="serverRecords">
  <s:label>serverRecords</s:label>
  <s:comment>Property attributing a "ServerRecords" object.</s:comment>
  <s:domain r:resource="#HS_ADMIN"/>
  <s:range r:resource="#ServerRecords"/>
</r:Property>

<!-- "serverRecord" property -->
<r:Property r:ID="serverRecord">
  <s:label>serverRecord</s:label>
  <s:comment>Property attributing a "ServerRecord" object.</s:comment>
  <s:domain r:resource="#ServerRecords"/>
  <s:range r:resource="#ServerRecord"/>
</r:Property>

<!-- "serviceInterface" property -->
<r:Property r:ID="serviceInterface">
  <s:label>serviceInterface</s:label>
  <s:comment>Property attributing a "ServiceInterface"
	 object.</s:comment>
  <s:domain r:resource="#ServerRecord"/>
  <s:range r:resource="#ServiceInterface"/>
</r:Property>

<!--
  Datatype Properties - HS_SITE
-->

<!-- "version" property -->
<r:Property r:ID="version">
  <s:label>version</s:label>
  <s:comment>Property attributing a "Version" field. A 2-byte value
	 that identifies the version number of the HS_SITE.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "protocolVersion" property -->
<r:Property r:ID="protocolVersion">
  <s:label>protocolVersion</s:label>
  <s:comment>Property attributing a "ProtocolVersion" field. A 2-byte
	 integer value that identifies the handle protocol version.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="&xsd;unsignedByte"/>
</r:Property>

<!-- "serialNumber" property -->
<r:Property r:ID="serialNumber">
  <s:label>serialNumber</s:label>
  <s:comment>Property attributing a "SerialNumber" field. A 2-byte
	 integer value that increases by 1 (and may wrap around through 0)
	 each time the HS_SITE value gets changed.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "multiPrimary" property -->
<r:Property r:ID="multiPrimary">
  <s:label>multiPrimary</s:label>
  <s:comment>Property attributing a "MultiPrimary" object.</s:comment>
  <s:domain r:resource="#PrimaryMask"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "primarySite" property -->
<r:Property r:ID="primarySite">
  <s:label>primarySite</s:label>
  <s:comment>Property attributing a "PrimarySite" object.</s:comment>
  <s:domain r:resource="#PrimaryMask"/>
  <s:range r:resource="&xsd;boolean"/>
</r:Property>

<!-- "hashOption" property -->
<r:Property r:ID="hashOption">
  <s:label>hashOption</s:label>
  <s:comment>Property attributing a "HashOption" field. An 8-bit octet
	 that identifies the hash option used by the service site to
	 distribute handles among its servers.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="&xsd;token"/>
</r:Property>

<!-- "hashFilter" property -->
<r:Property r:ID="hashFilter">
  <s:label>hashFilter</s:label>
  <s:comment>Property attributing a "HashFilter" object. An UTF8-string
	 entry reserved for future use.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="&xsd;unsignedByte"/>
</r:Property>

<!-- "attributeList" property -->
<r:Property r:ID="attributeList">
  <s:label>attributeList</s:label>
  <s:comment>Property attributing an "attributeList" object. A 4-byte
	 integer followed by a list of UTF8-string pairs.</s:comment>
  <s:domain r:resource="#HS_SITE"/>
  <s:range r:resource="#AttributeList"/>
</r:Property>

<!-- "attributeCount" property -->
<r:Property r:ID="attributeCount">
  <s:label>attributeCount</s:label>
  <s:comment>Property attributing an "AttributeCount" value. The
	 integer indicates the number of UTF8-string pairs that
	 follow.</s:comment>
  <s:domain r:resource="#AttributeList"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "attributePair" property -->
<r:Property r:ID="attributePair">
  <s:label>attributePair</s:label>
  <s:comment>Property attributing an "AttributePair" value. Each
	 UTF8-string pair is an "attribute":"value" pair.</s:comment>
  <s:domain r:resource="#AttributeList"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "attribute" property -->
<r:Property r:ID="attribute">
  <s:label>attribute</s:label>
  <s:comment>Property attributing an "Attribute" object.</s:comment>
  <s:domain r:resource="#AttributePair"/>
  <s:range r:resource="&xsd;string"/>
</r:Property>

<!-- "value" property -->
<r:Property r:ID="value">
  <s:label>value</s:label>
  <s:comment>Property attributing a "Value" object.</s:comment>
  <s:domain r:resource="#AttributePair"/>
  <s:range r:resource="&xsd;string"/>
</r:Property>

<!-- "numOfServer" property -->
<r:Property r:ID="numOfServer">
  <s:label>numOfServer</s:label>
  <s:comment>Property attributing an "NumOfServer" object.</s:comment>
  <s:domain r:resource="#ServerRecords"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "serverID" property -->
<r:Property r:ID="serverID">
  <s:label>serverID</s:label>
  <s:comment>Property attributing a "ServerID" object.</s:comment>
  <s:domain r:resource="#ServerRecord"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "address" property -->
<r:Property r:ID="address">
  <s:label>address</s:label>
  <s:comment>Property attributing an "Address" object.</s:comment>
  <s:domain r:resource="#ServerRecord"/>
  <s:range r:resource="&xsd;string"/>
</r:Property>

<!-- "publicKeyRecord" property -->
<r:Property r:ID="publicKeyRecord">
  <s:label>publicKeyRecord</s:label>
  <s:comment>Property attributing a "PublicKeyRecord"
	 object.</s:comment>
  <s:domain r:resource="#ServerRecord"/>
  <s:range r:resource="&xsd;string"/>
</r:Property>

<!-- "interfaceCounter" property -->
<r:Property r:ID="interfaceCounter">
  <s:label>serviceType</s:label>
  <s:comment>Property attributing an "InterfaceCounter"
	 object.</s:comment>
  <s:domain r:resource="#ServiceInterface"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!-- "serviceType" property -->
<r:Property r:ID="serviceType">
  <s:label>serviceType</s:label>
  <s:comment>Property attributing a "ServiceType" object.</s:comment>
  <s:domain r:resource="#ServiceInterface"/>
  <s:range r:resource="&xsd;token"/>
</r:Property>

<!-- "transmissionProtocol" property -->
<r:Property r:ID="transmissionProtocol">
  <s:label>transmissionProtocol</s:label>
  <s:comment>Property attributing a "TransmissionProtocol"
	 object.</s:comment>
  <s:domain r:resource="#ServiceInterface"/>
  <s:range r:resource="&xsd;token"/>
</r:Property>

<!-- "portNumber" property -->
<r:Property r:ID="portNumber">
  <s:label>portNumber</s:label>
  <s:comment>Property attributing a "PortNumber" object.</s:comment>
  <s:domain r:resource="#ServiceInterface"/>
  <s:range r:resource="&xsd;nonNegativeInteger"/>
</r:Property>

<!--
########################################################################
##  3.2.3. Naming Authority Delegation: HS_NA_DELEGATE (p. 19) [2f]
##
##  [2f] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.3
########################################################################
-->

<!-- "HS_NA_DELEGATE" class  (pre-defined HS type) -->
<s:Class r:ID="HS_NA_DELEGATE">
  <s:label>HS_NA_DELEGATE</s:label>
  <s:comment>HS_NA_DELEGATE is a pre-defined Handle System data type
	 which may be assigned to naming authority handles to designate
	 naming authority administration to a LHS. It has the exact same
	 format as the HS_SITE value.</s:comment>
</s:Class>

<!--
########################################################################
##  3.2.4. Service Handle: HS_SERV (p. 20) [2g]
##
##  [2g] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.4
########################################################################
-->

<!--
  Classes - HS_SERV
-->

<!-- "HS_SERV" class  (pre-defined HS type) -->
<s:Class r:ID="HS_SERV">
  <s:label>HS_SERV</s:label>
  <s:comment>HS_SERV is a pre-defined Handle System data type which
	 may be used to maintain the HS_SITE values for a handle service
	 and is referenced from a naming authority handle via a HS_SERV
	 value. A HS_SERV value is a handle value whose "type" field is
	 HS_SERV and whose "data" field contains the reference to the
	 service handle.</s:comment>
</s:Class>

<!--
  Datatype Properties - HS_SERV
-->

<!-- "service" property -->
<r:Property r:ID="service">
  <s:label>service</s:label>
  <s:comment>Property attributing a "Service" object.</s:comment>
  <s:domain r:resource="#HS_SERV"/>
  <s:range r:resource="#Handle"/>
</r:Property>

<!--
########################################################################
##  3.2.5. Alias Handle: HS_ALIAS (p. 21) [2h]
##
##  [2h] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.5
########################################################################
-->

<!--
  Classes - HS_ALIAS
-->

<!-- "HS_ALIAS" class  (pre-defined HS type) -->
<s:Class r:ID="HS_ALIAS">
  <s:label>HS_ALIAS</s:label>
  <s:comment>HS_ALIAS is a pre-defined Handle System data type which
	 allows a handle to provide an alternate name for a digital object
	 already referenced by a handle. An HS_ALIAS value is a handle
	 value whose "type" field is HS_ALIAS and whose "data" field
	 contains a reference to another handle.</s:comment>
</s:Class>

<!--
  Datatype Properties - HS_ALIAS
-->

<!-- "alias" property -->
<r:Property r:ID="alias">
  <s:label>alias</s:label>
  <s:comment>Property attributing a "Alias" object.</s:comment>
  <s:domain r:resource="#HS_ALIAS"/>
  <s:range r:resource="#HandleReference"/>
</r:Property>

<!--
########################################################################
##  3.2.6. Primary Site: HS_PRIMARY (p. 21) [2i]
##
##  [2i] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.6
########################################################################
-->

<!--
  Classes - HS_PRIMARY
-->

<!-- "HS_PRIMARY" class  (pre-defined HS type) -->
<s:Class r:ID="HS_PRIMARY">
  <s:label>HS_PRIMARY</s:label>
  <s:comment>HS_PRIMARY is a pre-defined Handle System data type used
	 to designate the primary service sites for any given handle. A
	 HS_PRIMARY value is a handle value whose "type" field is HS_PRIMARY
	 and whose "data" field contains a list of references to HS_SITE
	 values.</s:comment>
</s:Class>

<!--
  Datatype Properties - HS_PRIMARY
  ~
-->

<!-- "primary" property -->
<r:Property r:ID="primary">
  <s:label>primary</s:label>
  <s:comment>Property attributing a "Primary" object.</s:comment>
  <s:domain r:resource="#HS_PRIMARY"/>
  <s:range r:resource="#Handle"/>
</r:Property>

<!--
########################################################################
##  3.2.7. Handle Value List: HS_VLIST (p. 22) [2j]
##
##  [2j] http://www.apps.ietf.org/rfc/rfc3651.html#sec-3.2.7
########################################################################
-->

<!--
  Classes - HS_VLIST
-->

<!-- "HS_VLIST" class  (pre-defined HS type) -->
<s:Class r:ID="HS_VLIST">
  <s:label>HS_VLIST</s:label>
  <s:subClassOf>&rdf;Collection</s:subClassOf>
  <s:comment>HS_VLIST is a pre-defined Handle System data type that
	 allows a handle value to be used as a reference to a list of other
	 handle values.  An HS_VLIST value is a handle value whose "type" is
	 HS_VLIST and whose "data" consists of a 4-byte unsigned integer
	 followed by a list of references to other handle
	 values.</s:comment>
</s:Class>

<!-- "ValueReference" class -->
<s:Class r:ID="ValueReference">
  <s:label>ValueReference</s:label>
  <s:comment></s:comment>
</s:Class>

<!--
  Datatype Properties - HS_VLIST
-->

<!-- "valueReference" property -->
<r:Property r:ID="valueReference">
  <s:label>valueReference</s:label>
  <s:comment>Property attributing a "ValueReference" object.</s:comment>
  <s:domain r:resource="#HS_VLIST"/>
  <s:range r:resource="#HandleReference"/>
</r:Property>

</r:RDF>
<!--
########################################################################
##  EOF
########################################################################
-->
```