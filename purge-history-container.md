---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-29"

keywords: purge history, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Purge History container
{: #purge-history-container}

The `SoftLayer_Network_CdnMarketplace_Configuration_Cache_PurgeHistory` collection contains the attributes utilized by [Purge Group API](/docs/CDN?topic=CDN-cdn-api-reference#purge-by-group) and [Purge History API](/docs/CDN?topic=CDN-cdn-api-reference#api-for-purge-history).

**class** `SoftLayer_Network_CdnMarketplace_Configuration_Cache_PurgeHistory`:

* `groupUniqueId`: The unique ID of the purge group.
* `uniqueId`: The unique ID of the domain mapping.
* `groupName`: Purge Group's name.
* `status`: The purge's status. Status can be `SUCCESS`, `FAILED`, or `IN_PROGRESS`.
