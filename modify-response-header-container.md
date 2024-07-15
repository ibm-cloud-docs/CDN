---

copyright:
  years: 2020, 2024
lastupdated: "2024-07-15"

keywords:

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Modify Response Header container
{: #modify-response-header-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader` collection contains attributes that are used by the `modify-response-header` APIs. Each of the `modify-response-header` APIs returns a collection of this type.
{: shortdesc}

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`

* `uniqueId` - The unique ID of the mapping to which `modify-response-header` belongs.
* `modResHeaderUniqueId` - The unique ID of the `modify-response-header`.
* `path` - The path, relative to the domain that should be accessed using `modify-response-header`.
* `type` - The type of `modify-response-header` (`append`, `modify`, or `delete`).
* `headers` - A collection of key-value pairs that specify the headers and associated values to be modified. The header name and header value must be separated by a colon (:). For example: ['header1:value1','header2:value2'].
* `delimiter` - Specifies the delimiter to be used when indicating multiple values for a header. Valid values are a space, comma (default), semicolon, comma followed by a space, or a semicolon followed by a space.
* `description` - Specifies the description of `modify-response-header`.
