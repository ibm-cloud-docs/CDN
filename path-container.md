---

copyright:
  years: 2017
lastupdated: "2017-11-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Path (Origin) Container
The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` collection contains the attributes utilized by our (Origin) Path APIs. Each of the Path APIs returns a collection of this type.

class `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: The unique id of the mapping to which this Origin Path belongs.  
* `path`:  Path relative to the domain that can be used to reach this Origin.  
* `originType`: Type of the Origin host, currently ‘HOST\_SERVER’ or ‘OBJECT\_STORAGE’.  
* `httpPort`: Number of the port used for HTTP protocol.  
* `httpsPort`: Number of the port used for HTTPS protocol.  
* `status`: The status of the (Origin) Path. Valid statuses are: _CREATING_, _STARTING_, _RUNNING_, _UPDATING_, _STOPPING_, and _DELETING_.
* `fileExtension`: File extensions that are allowed to be cached.  
* `header`: Specifies Host header information used by the Origin server.
