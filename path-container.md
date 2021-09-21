---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: collection, attributes, query, arguments, class, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Path (Origin) container
{: #path-origin-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` collection contains the attributes used by our (Origin) Path APIs. Each of the Path APIs returns a collection of this type.
{: shortdesc}

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

* `mappingUniqueId`: The unique ID of the mapping to which this origin path belongs.
* `path`:  Path relative to the domain that can be used to reach this origin.
* `originType`: Type of the origin host, currently `HOST\_SERVER` or `OBJECT\_STORAGE`.
* `httpPort`: Number of the port used for HTTP protocol.
* `httpsPort`: Number of the port used for HTTPS protocol.
* `status`: The status of the (Origin) Path. Valid statuses are: _CREATING_, _STARTING_, _RUNNING_, _UPDATING_, _STOPPING_, and _DELETING_.
* `fileExtension`: File extensions that are allowed to be cached.
* `header`: Specifies Host header information that is used by the origin server.
* `cacheKeyQueryRule`: The following options are available to configure Cache Key behavior:
    * `include-all`: Include all query arguments **Default**
    * `ignore-all`: Ignores all query arguments
    * `ignore: space separated query-args`: Ignores those specific query arguments. For example, "ignore: query1 query2"
    * `include: space separated query-args`: Includes those specific query arguments. For example, "include: query1 query2"
