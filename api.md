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



# CDN API Reference

The {{site.data.keyword.BluSoftlayer_notm}} Application Programming Interface (commonly called the SLAPI), provided by IBM Cloud, is the development interface that gives developers and system administrators direct interaction with the {{site.data.keyword.BluSoftlayer_notm}} backend system.

The SLAPI implements many of the features in the Customer Portal: if an interaction is possible in the Customer Portal, it also can be accomplished in the SLAPI. Because you can interact with all portions of the {{site.data.keyword.BluSoftlayer_notm}} environment programmatically, within the SLAPI, you can use the API to automate tasks.

The SLAPI is a Remote Procedure Call (RPC) system. Each call involves sending data toward an API endpoint and receiving structured data in return. The format used to send and receive data with the SLAPI depends on which implementation of the API you choose. The SLAPI currently uses SOAP, XML-RPC, or REST for data transmission.

For more information about SLAPI, or about the IBM Cloud Content Delivery Network (CDN) service APIs, see the following resources in the IBM Cloud Development Network:

* [SLAPI Overview](https://sldn.softlayer.com/article/softlayer-api-overview )
* [Getting Started with SLAPI](http://sldn.softlayer.com/article/getting-started )
* [SoftLayer_Product_Package API](http://sldn.softlayer.com/reference/services/SoftLayer_Product_Package )
* [PHP Soap API Guide](https://sldn.softlayer.com/article/PHP )

----

To get started, here is a recommended API call sequence to follow:
* `listVendors` - Provides the list of supported vendors
* `verifyOrder` - Verifies whether the order can be placed
* `placeOrder` - Creates the CDN account with a given vendor. Up to 10 CDN Mappings can be created after a successful placeOrder call.
* `createDomainMapping` - Creates the CDN Mappings
* `verifyDomainMapping` - Changes CDN status to _RUNNING_

You can use the other APIs after you've followed the previous sequence.

[Example Code is available for each step in this call sequence.](cdn-example-code.html#code-examples-using-the-cdn-api)

**NOTE**: You **must** use the API username and API Key of a user with `CDN_ACCOUNT_MANAGE` permission for most of the API calls shown in this document. Please check with your account's Master user if you require this permission to be enabled for you. (Each IBM Cloud customer account is provided with one Master user.)

----
## API for Vendor
### listVendors
This API allows the user to list the supported CDN Vendors. The `vendorName` is needed to create a CDN account and get started with ordering your CDN.

* **Parameters** None
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Vendor`

The Vendor Container and a usage example can be viewed here: [Vendor Container](vendor-container.html)

----
## API for Account
### verifyCdnAccountExists
Checks whether a CDN account exists for the user calling the API, for the given `vendorName`.

* **Parameters**: `vendorName`: Provide the name of a valid CDN provider.
* **Return**: `true` if an account exists, else `false`.

----
## API for Domain Mapping
### createDomainMapping
Using the provided inputs, this function creates a domain mapping for the given vendor and associates it with the {{site.data.keyword.BluSoftlayer_notm}} Account ID of the user. The CDN account must first be created using `createCustomerSubAccount` for this API to work. After successfully creating the CDN, a `defaultTTL` is created with a value of 3600 seconds.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. The complete list of attributes available in the Input Container and their descriptions can be viewed here: [Input Container](input-container.html)

  The following attributes are **required** for this collection:
    * `vendorName`: A valid vendor must be provided.
    * `origin`: Provide the Origin address as a string.
    * `originType`: Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain`: Provide your host name as a string.
    * `protocol`: Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `httpPort` and/or `httpsPort`: These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](faqs.html) for allowed port numbers.

  The following additional attributes are **required** if you are using Object Storage:
    * `bucketName`: Unique name of your bucket for S3 Object Storage.
    * `path`: Path to your S3 Object Store. Usually found in the object store URL or API section of your service.

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.
  The collection provides a `uniqueId` value, which needs to be sent as input for subsequent API calls related to Mapping and Origin Path.

  [View the Mapping Container](mapping-container.html)

----
### deleteDomainMapping
Deletes the domain mapping based on the `uniqueId`. The domain mapping must be in one of the following states: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME_CONFIGURATION_, or _SSL_CONFIGURATION_.

* **Parameters**: `uniqueId`: the uniqueId of the mapping to be deleted
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
### verifyDomainMapping
Verifies the status of the CDN, and updates the `status` of the CDN mapping if it has changed. When a CDN mapping is created initially, its status shows as _CNAME_CONFIGURATION_. At this point, you must update the DNS record so that the CDN mapping points the hostname to the CNAME. Check with your DNS provider if you have questions about how the update is done and how long it might take for the change to propagate on the internet. Typically, it should take 15 to 30 minutes. After that time, this `verifyDomainMapping` API must be called to verify whether the CNAME chain is complete. If the CNAME chain is complete, the CDN mapping status changes to _RUNNING_.

This API can be called at any time to get the latest CDN mapping status. The domain mapping must be in one of the following states: _RUNNING_ or _CNAME_CONFIGURATION_.

* **Parameters**: `uniqueId`: uniqueId of the mapping you want to verify
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
### startDomainMapping
Starts a CDN domain mapping based on the `uniqueId`. To be started, the domain mapping must be in a _STOPPED_ state.

* **Parameters**: `uniqueId`: uniqueId of the mapping to be started
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
### stopDomainMapping
Stops a CDN domain mapping based on the `uniqueId`. To initiate the stop, the domain mapping must be in a _RUNNING_ state.

* **Parameters**: `uniqueId`: uniqueId of the mapping to be stopped
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
### updateDomainMapping
Enables the user to update properties of the mapping identified by the `uniqueId`. The following fields may be changed: `originHost`, `httpPort`, `httpsPort`, `respectHeader`, and if your Origin Type is Object Storage, the `bucketName` and `fileExtension` also may be changed. For an update to occur, the domain mapping must be in a _RUNNING_ state.

* **Parameters**: `uniqueId`: uniqueId of the mapping to be updated
* **Return** a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
### listDomainMappings
Returns a collection of all domain mappings for current customer.

* **Parameters**: None
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
### listDomainMappingByUniqueId
Returns a collection with a single domain object based on a CDN's `uniqueId`.

* **Parameters**: `uniqueId`: uniqueId of the mapping to be returned
* **Return**: a single-object collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
[View the Mapping Container](mapping-container.html)

----
## APIs for Origin
### createOriginPath
Creates an Origin Path for an existing CDN and for a particular customer. The Origin Path can be based on a Host Server or Object Storage. To create the Origin Path, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. The complete list of attributes available in the Input Container and their descriptions can be viewed here: [Input Container](input-container.html)

  The following attributes are **required** for this collection:
    * `vendorName`: Provide the name of the CDN provider for this mapping
    * `domainName`: Name of the domain for which the Origin Path is being created
    * `path`: Path relative to the domain that can be used to reach this origin
    * `originType`: Type of the Origin host, currently `HOST_SERVER` or `OBJECT_STORAGE`
    * `origin`: Origin Server address

  The following attributes are **required** if the Origin Type is `HOST_SERVER`:
    * `httpPort` and/or `httpsPort`: These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](./faqs.md) for allowed port numbers

  If the Origin Type is `OBJECT_STORAGE`, `bucketName` must also be provided, along with an optional `fileExtension`.

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

[View the Origin Path Container](path-container.html)

----
### updateOrigin
Updates an existing Origin Path for an existing mapping and for a particular customer. The Origin type cannot be changed. The following properties can be changed: `path`, `origin`, `httpPort`, and `httpsPort`. To be updated, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. The complete list of attributes available in the Input Container and their descriptions can be viewed here: [Input Container](input-container.html)

  The following attributes are **required** for this collection:
    * `vendorName`: Provide the name of the CDN provider for this mapping
    * `domainName`: Name of the domain for which the Origin Path is being updated
    * `path`: Current path, or if being updated, the *new* path
    * `originType`: Type of the Origin host, currently `HOST_SERVER` or `OBJECT_STORAGE`
    * `origin`: Origin server address

  The following attributes are **required** if the Origin Type is `HOST_SERVER`:
    * `httpPort` and/or `httpsPort`: These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](./faqs.md) for allowed port numbers

  If the Origin Type is `OBJECT_STORAGE`, `bucketName` must also be provided, along with an optional `fileExtension`.

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

[View the Origin Path Container](path-container.html)

----
### deleteOriginPath
Deletes and existing Origin Path for an existing CDN, and for a particular customer. To be deleted, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**:
  * `uniqueId`: The uniqueId of the mapping to which this Origin Path belongs
  * `path`: Path to be deleted

* **Return**: a status message if the delete was successful, otherwise an exception is thrown.

----
### listOriginPath
Lists the Origin Paths for an existing mapping based on the `uniqueId`.

* **Parameters**: `uniqueId` - provide the uniqueid of the mapping for which you want to list Origin Paths.
* **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

[View the Origin Path Container](path-container.html)

----
## API for Purge
### Container class for Purge:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
Creates a purge record and inserts it into the database.

* **Parameters**:
  * `uniqueId`: uniqueId of the mapping to which the Purge will be created
  * `path`: path of the Purge to be created

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
Returns the Purge history for a CDN based on the `uniqueId` and `saved` status. (By default, the value of `saved` is set to _UNSAVED_.)

* **Parameters**:
  * `uniqueId`: uniqueId of the mapping for which retrieve Purge history
  * `saved`: return Purges that have been _SAVED_ or _UNSAVED_

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
Updates the status of the Purge path entry to indicate whether that Purge path should be saved. Creates a new `saved` Purge if a Purge path is saved. Deletes a saved Purge record if the path is `unsaved`.

* **Parameters**:
  * `uniqueId`: uniqueId of the mapping to which the Purge belongs
  * `path`: path of the Purge to be saved or unsaved
  * `saved`: _SAVED_ or _UNSAVED_

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
## API for Time to Live  
### TimeToLive class variables:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
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

 ----
## API for Metrics  
### Container class for Metrics:  
```  
class SoftLayer_Container_Network_CdnMarketplace_Metrics  
{  
    /**
     * @var string
     */
    public $type;

    /**
     * @var string[]
     */
    public $names;

    /**
     * @var string[]
     */   
     public $totals;

    /**
     * @var string[]
     */
    public $percentage;

    /**
     * @var string[]
     */
    public $time;

    /**
     * @var string[]
     */
    public $xaxis;

    /**
     * @var string[]
     */
    public $yaxis1;

    /**
     * @var string[]
     */
    public $yaxis2;

    /**
     * @var string[]
     */
    public $yaxis3;

    /**
     * @var string[]
     */
    public $yaxis4;

    /**
     * @var string[]
     */
    public $yaxis5;

    /**
     * @var string[]
     */
    public $yaxis6;

    /**
     * @var string[]
     */
    public $yaxis7;

    /**
     * @var string[]
     */
    public $yaxis8;

    /**
     * @var string[]
     */
    public $yaxis9;

    /**
     * @var string[]
     */
    public $yaxis10;

    /**
     * @var string[]
     */
    public $yaxis11;

    /**
     * @var string[]
     */
    public $yaxis12;

    /**
     * @var string[]
     */
    public $yaxis13;

    /**
     * @var string[]
     */
    public $yaxis14;

    /**
     * @var string[]
     */
    public $yaxis15;

    /**
     * @var string[]
     */
    public $yaxis16;

    /**
     * @var string[]
     */
    public $yaxis17;

    /**
     * @var string[]
     */
    public $yaxis18;

    /**
     * @var string[]
     */
    public $yaxis19;

    /**
     * @var string[]
     */
    public $yaxis20;
}  
```  
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
Returns the number of edge hits for an individual CDN. Regions may differ for each vendor. Per mapping.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a given mapping, over a given period of time. The value of `frequency` is 'aggregate' by default.

 * **Parameters**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics` 
