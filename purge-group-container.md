---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-29"

keywords: purge group, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Purge Group container
{: #purge-group-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup` collection contains the attributes utilized by our Purge Group APIs. Each of the Purge Group APIs returns a collection of this type.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`:

* `groupUniqueId`: A 10-digit, system-generated, identifier that is unique to purge group. Generated when a purge group is created.
* `uniqueId`: The unique ID of the mapping to which this purge group belongs.
* `name`: Purge Group name. The favorite group name must be unique.
* `saved`: Save the purge group as favorite or not. Possible values `SAVED` and `UNSAVED`.
* `paths`: A collection of file paths to purge.
* `pathCount`: Total number of purge paths.
* `option`: The following options are available to create a Purge Group:
  * `1`: Create the purge group as a favorite group, and do not execute a purge action.
  * `2`: Create the purge group as an unfavorite group, and execute a purge action.
  * `3`: Create the purge group as a favorite group, and execute a purge action.
* `lastPurgeDate`: A timestamp to mark last purge time.
* `purgeStatus`: The purge's status when the input option field is `1` or `3`. Status can be `SUCCESS`, `FAILED`, or `IN_PROGRESS`.

Of particular note is the `groupUniqueId`, which is generated when a Purge Group is created and is used as a parameter in subsequent Purge Group API calls.
