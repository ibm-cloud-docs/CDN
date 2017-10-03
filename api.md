---

copyright:
  years: 2017
lastupdated: "2017-10-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# API Reference

The {{site.data.keyword.BluSoftlayer_notm}} Application Programming Interface (commonly called the SLAPI), provided by IBM Cloud, is the development interface that gives developers and system administrators direct interaction with the {{site.data.keyword.BluSoftlayer_notm}} backend system.

The SLAPI implements many of the features in the Customer Portal: if an interaction is possible in the Customer Portal, it also can be accomplished in the SLAPI. Because you can interact with all portions of the {{site.data.keyword.BluSoftlayer_notm}} environment programmatically, within the SLAPI, you can use the API to automate tasks.

The  SLAPI is a Remote Procedure Call (RPC) system. Each call involves sending data toward an API endpoint and receiving structured data in return. The format used to send and receive data with the SLAPI depends on which implementation of the API you choose. The SLAPI currently uses SOAP, XML-RPC, or REST for data transmission.

For more information about SLAPI, or about the IBM CDN Service APIs, see the following resources in the IBM Cloud Development Network:

* [SLAPI Overview](https://sldn.softlayer.com/article/softlayer-api-overview )
* [Getting Started with SLAPI](http://sldn.softlayer.com/article/getting-started ) 
* [SoftLayer_Product_Package API](http://sldn.softlayer.com/reference/services/SoftLayer_Product_Package )
* [PHP Soap API Guide](https://sldn.softlayer.com/article/PHP )

----


To get started, here is a recommended API call sequence to follow:
* `listVendors` - Provides the list of supported vendors.  [Example code](./cdn-example-code.md#example-code-of-listing-vendors)
* `verifyOrder` - Verifies whether the order can be placed.  [Example code](./cdn-example-code.md#example-code-to-verify-order)
* `placeOrder`  - Creates the CDN account with a given vendor.  [Example code](./infrastructure/CDN/cdn-example-code.md#example-code-to-place-order)
* `createDomainMapping` - Creates the CDN Mapping.  [Example code](./cdn-example-code.md#example-code-to-create-cdn-or-create-domain-mapping)
* `verifyDomainMapping` - Changes CDN status to _RUNNING_  [Example code](./cdn-example-code.md#example-code-of-verify-domain-mapping)

You can use the other APIs after you've followed the previous sequence.

[Example Code is available for each step in this call sequence.](./cdn-example-code.md)


## API for Vendor
### listVendors
This API allows the user to list the supported CDN Vendors. The `vendorName` is needed to create a CDN account and get started with ordering the CDN

* **Parameters**: None
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Vendor`
 
___
## API for an Account
### verifyCdnAccountExists
Checks whether a CDN account exists for the user calling the API, for the given `vendorName`

 * **Parameters**: `vendorName`
 * **Return**: `true` if account exists, else `false`
___
## API for Domain Mapping
### createDomainMapping
Using the provided inputs, this function creates a domain mapping for the given vendor and associates it with the {{site.data.keyword.BluSoftlayer_notm}} Account ID of the user. The CDN account must be created using `createCustomerSubAccount` for this API to work. After successfully creating the CDN, a `defaultTTL` is created with a value of 3600 seconds.

 * **Parameters**:  a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. The collection should include the following items:  `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType`, `path`
 * **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`. The collection provides a `uniqueId` value, which needs to be sent as input for subsequent API calls related to mapping.
___ 
### deleteDomainMapping
Deletes the domain mapping based on the `uniqueId`. The domain mapping must be in one of the following states:  _RUNNING_, _STOPPED_, , _DELETED_, _ERROR_, _CNAME\_CONFIGURATION_, or _SSL\_CONFIGURATION_.

 * **Parameters**: `string` `uniqueId`
 * **Return**: an collection of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### verifyDomainMapping
Verifies the status of the CDN, and updates the `status` of the CDN Mapping. When a CDN Mapping is created initially, its status shows as _CNAME Configuration_. At this point, you must update the DNS record to point the CDN Mapping hostname to the CNAME. Check with your DNS provider if you have questions about how the update is done and how long it might take for the change to propagate on the internet. Typically, it should take 15 to 30 minutes. After that time, this `vendorDomainMapping` API must be called to verify whether the CNAME chain is complete. If the CNAME chain is complete, the CDN Mapping status changes to _RUNNING_ status. 

This API can be called at any time to get the latest CDN mapping status.
The domain mapping must be in one of the following states: _RUNNING_, _CNAME\_CONFIGURATION_.

 * **Parameters**: `string` `uniqueId`
 * **Return**: a collection of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### startDomainMapping
Starts a CDN domain mapping based on the `uniqueId`. To be started, the domain mapping must be in a _STOPPED_ state.

 * **Parameters**: `string` `uniqueId`
 * **Return**: a collection of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### stopDomainMapping
Stops a CDN domain mapping based on the `uniqueId`. To initiate the stop, the domain mapping must be in a _RUNNING_ state.

 * **Parameters**: `string` `uniqueId`
 * **Return**: a collection of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### updateDomainMapping
Enables the user to update properties of the mapping identified by the `uniqueId`. The following fields may be changed: `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path`, and if your path's origin type is Object Storage, the `bucketName` and `fileExtension` also may be changed. For an update to occur, the domain mapping must be in a _RUNNING_ state.

 * **Parameters**: `string` `uniqueId`
 * **Return**: a collection of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### listDomainMappings
Returns a collection of all domains for a particular customer.

 * **Parameters**: _none_ 
 * **Return**: a collection of objects of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### listDomainMappingByUniqueId
Returns a collection with a single domain object based on a CDN's `uniqueId`.

 * **Parameters**: `string` `uniqueId`
 * **Return**: a single-object collection of objects of type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___

## API for Purge
### createPurge
Creates a purge record and inserts it into the database.

 * **Parameters**: `string` `uniqueId`, `string` `path`
 * **Return**:  a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### getPurgeHistoryPerMapping
Returns the purge history for a CDN based on the `uniqueId` and `saved` status. (By default, the value of `saved` is set to _unsaved_.)

 * **Parameters**: `string` `uniqueId`, `int` `saved`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### saveOrUnsavePurgePath
Updates the status of the purge path entry to indicate whether that purge path should be saved. Creates a new `saved` purge if a purge path is saved. Deletes a saved purge record if the path is `unsaved`.

 * **Parameters**: `string` `uniqueId`, `string` `path`, `int` `saved`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
## API for Time to Live
### createTimeToLive
Creates a new `TimeToLive` object and inserts it into the database.

 * **Parameters**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Return**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Updates an existing `TimeToLive` object. If the _oldTtl_ and _newTtl_ inputs are equal, it exits early.

 * **Parameters**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Returs**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Deletes an existing `TimeToLive` object from the database.

 * **Parameters**: `string` `uniqueId`, `string` `pathName`
 * **Return**: a string with the status of the delete
___
### listTtl
Lists existing `TimeToLive` objects based on a CDN's `uniqueId`.

 * **Parameters**: `string` `uniqueId`
 * **Return**: an array of objects of type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
## API for Origin
### createOriginPath
Creates an Origin Path for an existing CDN and for a particular customer. The Origin Path can be based on a host server or Object Storage. To create the Origin Path, the domain mapping must be in either a _RUNNING_ or _CNAME\_CONFIGURATION_ state.  

 * **Parameters**: a `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` object that expects the following properties to be set: `domainName`, `vendorName`, `path`, `originType`, and `origin`. If the origin type is server, `httpPort` and/or `httpsPort` must also be set. If the origin type is Object Storage, the `bucketName` must also be provided, along with an optional `fileExtension`.  
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### updateOrigin
Updates an existing Origin Path for an existing mapping and for a particular customer. The `originType` cannot be changed. Only the following properties can be changed: `path`, `origin`, `httpPort`, and `httpsPort`. To be updated, the domain mapping must be in either a _RUNNING_ or _CNAME\_CONFIGURATION_ state.

 * **Parameters**: a `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` object that expects the following properties to be set: `domainName`, `vendorName`, `path`, `originType`, and `origin`. If the origin type is a server, `httpPort` and/or `httpsPort` must also be set. If the path's origin type is Object Storage, `bucketName` must be provided, and optionally `fileExtension` may be provided.  
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### deleteOriginPath
Deletes an existing Origin Path for an existing CDN, and for a particular customer. To be deleted, the domain mapping must be in either a _RUNNING_ or _CNAME\_CONFIGURATION_ state.  

 * **Parameters**: `string` `uniqueId`, `string` `path`
 * **Return**: a status message if delete was successful; otherwise, an execption
___
### listOriginPath
Lists the origin path for an existing mapping, and for a particular customer.

 * **Parameters**: `string` `uniqueId`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___

## API for Metrics
### getCustomerUsageMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a customer's account, over a given period of time.

 * **Parameters**: `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingUsageMetrics
Returns the total number of predetermined statistics for direct display for the given mapping. The value of `frequency` is 'aggregate' by default.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingHitsMetrics
Returns the total number of hits at a certain frequency over a given range of time, per domain mapping. Frequency can be 'day', 'week', and 'month', where each interval is one plot point for a graph. Return data is ordered based on `startDate`, `endDate`, and `frequency`. The value of `frequency` is 'aggregate' by default.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Returns the total number of hits at a certain frequency over a given range of time. Frequency can be 'day', 'week', and 'month', where each interval is one plot point for a graph. Return data must be ordered based on `startDate`, `endDate`, and `frequency`. The value of `frequency` is 'aggregate' by default.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Returns the number of edge hits for an individual CDN. Regions may differ for each vendor. Per Mapping.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a given mapping, over a given period of time. The value of `frequency` is 'aggregate' by default.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
