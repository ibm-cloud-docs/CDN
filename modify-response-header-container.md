---

copyright:
  years: 2020
lastupdated: "2020-12-29"

keywords:

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# modify-response-header container
{: #modify-response-header-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader` collection contains the attributes that are used by our `modify-response-header` APIs. Each of the `modify-response-header` APIs returns a collection of this type.
{:shortdesc}

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`  

* `uniqueId` - The unique ID of the mapping to which `modify-response-header` belongs.
* `modResHeaderUniqueId` - The unique ID of the `modify-response-header`.
* `path` - The path, relative to the domain that should be accessed using `modify-response-header`.
* `type` - The type of `modify-response-header` (`append`, `modify`, or `delete`).
* `headers` - A collection of key-value pairs that specify the headers and associated values to be modified. The header name and header value must be separated by a colon (:). For example: ['header1:value1','header2:Value2'].
* `delimiter` - Specifies the delimiter to be used when indicating multiple values for a header. Valid values are a space, comma (default), semicolon, comma followed by a space, or a semicolon followed by a space.
* `description` - Specifies the description of `modify-response-header`.
