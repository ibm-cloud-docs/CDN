---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-29"

keywords: class, collection, object, API

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Purge History container
{: #purge-history-container}

The `SoftLayer_Network_CdnMarketplace_Configuration_Cache_PurgeHistory` collection contains the attributes used by [Purge Group API](/docs/CDN?topic=CDN-cdn-api-reference#purge-by-group) and [Purge History API](/docs/CDN?topic=CDN-cdn-api-reference#api-for-purge-history).
{: shortdesc}

**class** `SoftLayer_Network_CdnMarketplace_Configuration_Cache_PurgeHistory`:

* `groupUniqueId`: The unique ID of the purge group.
* `uniqueId`: The unique ID of the domain mapping.
* `groupName`: Purge Group's name.
* `status`: The purge's status. Status can be `SUCCESS`, `FAILED`, or `IN_PROGRESS`.
