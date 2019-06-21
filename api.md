---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-06-19"

keywords: application programming interface, api, slapi, reference, development interface

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}


# CDN API Reference
{: #cdn-api-reference}

The SoftLayer Application Programming Interface (commonly called the SLAPI), provided by {{site.data.keyword.cloud}}, is the development interface that gives developers and system administrators direct interaction with the {{site.data.keyword.cloud_notm}} Infrastructure backend system.

The SLAPI implements many of the features in the Customer Portal: if an interaction is possible in the Customer Portal, it also can be accomplished in the SLAPI. Because you can interact with all portions of the {{site.data.keyword.cloud_notm}} Infrastructure environment programmatically, within the SLAPI, you can use the API to automate tasks.

The SLAPI is a Remote Procedure Call (RPC) system. Each call involves sending data toward an API endpoint and receiving structured data in return. The format used to send and receive data with the SLAPI depends on which implementation of the API you choose. The SLAPI currently uses SOAP, XML-RPC, or REST for data transmission.

For more information about SLAPI, or about the {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN) service APIs, see the following resources in the {{site.data.keyword.cloud_notm}} Development Network:

* [SLAPI Overview](https://softlayer.github.io/ )
* [Getting Started with SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API Guide](https://softlayer.github.io/article/php/ )

----

To get started, here is a recommended API call sequence to follow:
* `listVendors` - Provides the list of supported vendors
* `verifyOrder` - Verifies whether the order can be placed
* `placeOrder` - Creates the CDN account with a given vendor. Up to 10 CDN Mappings can be created after a successful placeOrder call.
* `createDomainMapping` - Creates the CDN Mappings
* `verifyDomainMapping` - Changes CDN status to _RUNNING_

You can use the other APIs after you've followed the previous sequence.

[Example Code is available for each step in this call sequence.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

You **must** use the API username and API Key of a user with `CDN_ACCOUNT_MANAGE` permission for most of the API calls shown in this document. Please check with your account's Master user if you require this permission to be enabled for you. (Each IBM Cloud customer account is provided with one Master user.)
{: note}

----
## API for Vendor
{: #api-for-vendor}

### listVendors
This API allows the user to list the supported CDN Vendors. The `vendorName` is needed to create a CDN account and get started with ordering your CDN.

* **Required Parameters**: None
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Vendor`

  The Vendor Container and a usage example can be viewed here: [Vendor Container](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## API for Account
{: #api-for-account}

### verifyCdnAccountExists
Checks whether a CDN account exists for the user calling the API, for the given `vendorName`.

* **Required Parameters**: `vendorName`: Provide the name of a valid CDN provider.
* **Return**: `true` if an account exists, else `false`.

----
## API for Domain Mapping
{: #api-for-domain-mapping}

### createDomainMapping
Using the provided inputs, this function creates a domain mapping for the given vendor and associates it with the {{site.data.keyword.cloud_notm}} Infrastructure Account ID of the user. The CDN account must first be created using `placeOrder` for this API to work (see an example of the `placeOrder` API call in the [Code Examples](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api). After successfully creating the CDN, a `defaultTTL` is created with a value of 3600 seconds.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input Container here:

  [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

  The following attributes are part of the Input Container and may be provided when creating a domain mapping (attributes are optional unless otherwise noted):
    * `vendorName`: **required** Provide the name of a valid IBM Cloud CDN provider.
    * `origin`: **required** Provide the Origin server address as a string.
    * `originType`: **required** Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain`: **required** Provide your host name as a string.
    * `protocol`: **required** Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `certificateType`: **required** for HTTPS protocol. `SHARED_SAN_CERT` or `WILDCARD_CERT`
    * `path`: Path from which the cached content will be served. Default path is `/*`
    * `httpPort` and/or `httpsPort`: (**required** for Host Server) These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for allowed port numbers.
    * `header`: Specifies host header information used by the Origin Server
    * `respectHeader`: A boolean value that, if set to `true`, will cause TTL settings in the Origin to override CDN TTL settings.
    * `cname`: Provide an alias to the hostname. Will be generated if one is not provided.
    * `bucketName`: (**required** for Object Storage only) Bucket name for your S3 Object Storage.
    * `fileExtension`: (optional for Object Storage) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule`: The following options are available to configure Cache Key behavior. If no `cacheKeyQueryRule` arguments are supplied, it will default to "include-all"
      * `include-all` - includes all query arguments **default**
      * `ignore-all` - ignores all query arguments
      * `ignore: space separated query-args` - ignores those specific query arguments. For example, `ignore: query1 query2`
      * `include: space separated query-args`: includes those specific query arguments. For example, `include: query1 query2`

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  **NOTE**: The collection provides a `uniqueId` value, which needs to be sent as input for subsequent API calls related to Mapping and Origin Path.

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Deletes the domain mapping based on the `uniqueId`. The domain mapping must be in one of the following states: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME_CONFIGURATION_, or _SSL_CONFIGURATION_.

* **Required Parameters**: `uniqueId`: the uniqueId of the mapping to be deleted
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Verifies the status of the CDN, and updates the `status` of the CDN mapping if it has changed. When a CDN mapping is created initially, its status shows as _CNAME_CONFIGURATION_. At this point, you must update the DNS record so that the CDN mapping points the hostname to the CNAME. Check with your DNS provider if you have questions about how the update is done and how long it might take for the change to propagate on the internet. Typically, it should take 15 to 30 minutes. After that time, this `verifyDomainMapping` API must be called to verify whether the CNAME chain is complete. If the CNAME chain is complete, the CDN mapping status changes to _RUNNING_.

This API can be called at any time to get the latest CDN mapping status. The domain mapping must be in one of the following states: _RUNNING_ or _CNAME_CONFIGURATION_.

* **Required Parameters**: `uniqueId`: uniqueId of the mapping you want to verify
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Starts a CDN domain mapping based on the `uniqueId`. To be started, the domain mapping must be in a _STOPPED_ state.

* **Required Parameters**: `uniqueId`: uniqueId of the mapping to be started
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Stops a CDN domain mapping based on the `uniqueId`. To initiate the stop, the domain mapping must be in a _RUNNING_ state.

* **Required Parameters**: `uniqueId`: uniqueId of the mapping to be stopped
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
Enables the user to update properties of the mapping identified by the `uniqueId`. The following fields may be changed: `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header`, `cacheKeyQueryRule` arguments, and if your Origin Type is Object Storage, the `bucketName` and `fileExtension` also may be changed. For an update to occur, the domain mapping must be in a _RUNNING_ state.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input Container here:
  [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

  The following attributes are part of the Input Container and are **required** to be provided when updating a domain mapping:
    * `vendorName`: Provide the name of the CDN provider for this mapping.
    * `path`: Provide the current path for this mapping
    * `origin`: Provide the Origin server address as a string.
    * `originType`: Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain`: Provide your host name.
    * `protocol`: Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `httpPort` and/or `httpsPort`: These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for allowed port numbers.
    * `header`: Specifies host header information used by the Origin Server
    * `respectHeader`: A Boolean value that, if set to `true`, will cause TTL settings in the Origin to override CDN TTL settings.
    * `uniqueId`: Generated after the mapping is created.
    * `cname`: Provide the cname. One was generated when the mapping was created if you did not provide one.
    * `bucketName`: (**required** for Object Storage only) Bucket name for your S3 Object Storage.
    * `fileExtension`: (**required** for Object Storage only) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule`: Cache key behavior rules can only be updated for CDN mappings created _after_ 11/16/17. The following options are available to configure Cache Key behavior:
      * `include-all` - includes all query arguments **default**
      * `ignore-all` - ignores all query arguments
      * `ignore: space separated query-args` - ignores those specific query arguments. For example, `ignore: query1 query2`
      * `include: space separated query-args`: includes those specific query arguments. For example, `include: query1 query2`
* **Return** a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Returns a collection of all domain mappings for current customer.

* **Required Parameters**: None
* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Returns a collection with a single domain object based on a CDN's `uniqueId`.

* **Required Parameters**: `uniqueId`: uniqueId of the mapping to be returned
* **Return**: a single-object collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [View the Mapping Container](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## APIs for Origin
{: #apis-for-origin}

### createOriginPath
Creates an Origin Path for an existing CDN and for a particular customer. The Origin Path can be based on a Host Server or Object Storage. To create the Origin Path, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input Container here:

  [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

  The following attributes are part of the Input Container and may be provided when creating an Origin Path (attributes are optional unless otherwise noted):
    * `vendorName`: **required** Provide the name of a valid IBM Cloud CDN provider.
    * `origin`: **required** Provide the Origin server address as a string.
    * `originType`: **required** Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain`: **required** Provide your host name as a string.
    * `protocol`: **required** Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `path`: Path from which the cached content will be served. Must begin with the mapping path. For example, if the mapping path is `/test`, then your Origin Path may be `/test/media`
    * `httpPort` and/or `httpsPort`: **required** These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for allowed port numbers.
    * `header`: Specifies host header information used by the Origin Server
    * `uniqueId`: **required** Generated after the mapping is created.
    * `cname`: Provide an alias to the hostname. If you did not provide a unique cname, one was generated for you when the mapping was created.
    * `bucketName`: (**required** for Object Storage) Bucket name for your S3 Object Storage.
    * `fileExtension`: (optional for Object Storage) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule`: The following options are available to configure Cache Key behavior:
      * `include-all` - includes all query arguments **default**
      * `ignore-all` - ignores all query arguments
      * `ignore: space separated query-args` - ignores those specific query arguments. For example, `ignore: query1 query2`
      * `include: space separated query-args`: includes those specific query arguments. For example, `include: query1 query2`

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [View the Origin Path Container](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Updates an existing Origin Path for an existing mapping and for a particular customer. The Origin type cannot be changed with this API. The following properties can be changed: `path`, `origin`, `httpPort`, and `httpsPort`, `header` and `cacheKeyQueryRule` arguments. To be updated, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input Container here:

  [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

  The following attributes are part of the Input Container and may be provided when updating an Origin Path (attributes are optional unless otherwise noted):
    * `oldPath`: **required** Current path to be changed
    * `origin`: (**required** if being updated) Provide the Origin server address as a string.
    * `originType`: **required** Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `path`: **required** New path to be added. Relative to the mapping path.
    * `httpPort` and/or `httpsPort`: (**required** for Host Server, if being updated) These two options must correspond to the desired protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. Please see the [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for allowed port numbers.
    * `uniqueId`: **required** uniqueId of the mapping to which this Origin belongs
    * `bucketName`: (**required** for Object Storage only) Bucket name for your S3 Object Storage.
    * `fileExtension`: (**required** for Object Storage only) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule`: (**required** if being updated) Cache key behavior rules can only be updated for Origin Paths created _after_ 11/16/17.The following options are available to configure Cache Key behavior:
      * `include-all` - includes all query arguments **default**
      * `ignore-all` - ignores all query arguments
      * `ignore: space separated query-args` - ignores those specific query arguments. For example, `ignore: query1 query2`
      * `include: space separated query-args`: includes those specific query arguments. For example, `include: query1 query2`

* **Return**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [View the Origin Path Container](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Deletes and existing Origin Path for an existing CDN, and for a particular customer. To be deleted, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Required Parameters**:
  * `uniqueId`: The uniqueId of the mapping to which this Origin Path belongs
  * `path`: Path to be deleted

* **Return**: a status message if the delete was successful, otherwise an exception is thrown.

----
### listOriginPath
Lists the Origin Paths for an existing mapping based on the `uniqueId`.

* **Required Parameters**:
  * `uniqueId`: provide the uniqueId of the mapping for which you want to list Origin Paths.
* **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [View the Origin Path Container](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## API for Purge
{: #api-for-purge}

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
{: #api-for-time-to-live}

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
{: #api-for-metrics}

[View the Metrics Container](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a customer's account, over a given period of time.

 * **Parameters**:
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingUsageMetrics
Returns the total number of predetermined statistics for direct display for the given mapping. The value of `frequency` is 'aggregate' by default.

 * **Parameters**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsMetrics
Returns the total number of hits at a certain frequency over a given range of time, per domain mapping. Frequency can be 'day', 'week', and 'month', where each interval is one plot point for a graph. Return data is ordered based on `startDate`, `endDate`, and `frequency`. The value of `frequency` is 'aggregate' by default.

 * **Parameters**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Returns the total number of hits at a certain frequency over a given range of time. Frequency can be 'day', 'week', and 'month', where each interval is one plot point for a graph. Return data must be ordered based on `startDate`, `endDate`, and `frequency`. The value of `frequency` is 'aggregate' by default.

 * **Parameters**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Returns the number of edge hits for an individual CDN. Regions may differ for each vendor. Per mapping.

 * **Parameters**:  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a given mapping, over a given period of time. The value of `frequency` is 'aggregate' by default.

 * **Parameters**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Return**: a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`

----
## API for Geographical Access Control
{: #api-for-geographical-access-control}

### createGeoblocking
Creates a new Geographical Access Control rule, and returns the newly created rule.

  * **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input Container here:

    [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

    The following attributes are part of the Input Container and are **required** when creating a new Geographical Access Control rule:
    * `uniqueId`: uniqueId of the mapping to assign the rule
    * `accessType`: specifies whether the rule will `ALLOW` or `DENY` traffic to the given region
    * `regionType`: the type of region to apply the Geographical Access Control rule to, either `CONTINENT` or `COUNTRY_OR_REGION`
    * `regions`: an array listing the locations that the `accessType` will apply to

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) page to see a list of possible regions.

  * **Returns**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [View the Geo-blocking class](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Updates an existing Geographical Access Control rule for an existing domain mapping and returns the updated rule.

  * **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input Container here:

    [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

    The following attributes are part of the Input Container and may be provided when updating a  Geographical Access Control rule (parameters are optional unless otherwise noted):
    * `uniqueId`: **required** uniqueId of the mapping the rule to be updated belongs
    * `accessType`: specifies whether the rule will `ALLOW` or `DENY` traffic to the given region.
    * `regionType`: the type of region to apply the rule to, either `CONTINENT` or `COUNTRY_OR_REGION`
    * `regions`: an array listing the locations that the `accessType` will apply to

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) page to see a list of possible regions.

  * **Returns**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [View the Geo-blocking class](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Removes an existing Geographical Access Control rule from an existing domain mapping.

  * **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input Container here:

    [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

    The following attribute is part of the Input Container and is **required** when deleting a Geographical Access Control rule:
    * `uniqueId`: provide the uniqueId of the mapping that the rule to be deleted belongs to.

  * **Returns**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [View the Geo-blocking class](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Retrieves a mapping's Geographical Access Control behavior from the database.

  * **Parameters**:
    * `uniqueId`: the uniqueId of the mapping that the rule belongs to.

  * **Returns**: an object of type
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [View the Geo-blocking class](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Returns a list of the types and regions that are allowed for creating Geographical Access Control rules.

  * **Parameters**:
    * `uniqueId`: the uniqueId of your domain mapping.

  * **Returns**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`

    [View the Geo-blocking class](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## API for Hotlink Protection
{: #api-for-hotlink-protection}

### createHotlinkProtection
Creates a new Hotlink Protection, and returns the newly created behavior.

  * **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input Container here:

    [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

    The following attributes are part of the Input Container and are **required** when creating a new Hotlink Protection:
    * `uniqueId`: uniqueId of the mapping to which to assign the behavior
    * `protectionType`: specifies whether to "ALLOW" or "DENY" access to your content when a webpage makes a request for content with a Referer header value matching one of the terms in the specified refererValues
    * `refererValues`: a single-space-separated list of Referer URL match terms for which the `protectionType` behavior will take effect

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) page to see a list of valid Hotlink Protection values.

  * **Returns**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [View the Hotlink Protection class](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Updates an existing Hotlink Protection behavior for an existing domain mapping and returns the updated behavior.

  * **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input Container here:

    [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

    The following attributes are part of the Input Container and are **required** when updating an existing Hotlink Protection:
    * `uniqueId`: the uniqueId of the mapping to which the existing behavior belongs
    * `protectionType`: specifies whether to "ALLOW" or "DENY" access to your content when a webpage makes a request for content with a Referer header value matching one of the terms in the specified refererValues 
    * `refererValues`: a single-space-separated list of Referer URL match terms for which the `protectionType` behavior will take effect

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) page to see a list of valid Hotlink Protection values.

  * **Returns**: an object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [View the Hotlink Protection class](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Removes an existing Hotlink Protection behavior from an existing domain mapping.

  * **Parameters**: a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input Container here:

    [View the Input Container](/docs/infrastructure/CDN?topic=CDN-input-container)

    The following attributes are part of the Input Container and are **required** when creating a new Hotlink Protection:
    * `uniqueId`: uniqueId of the mapping from which to remove the behavior

  * **Returns**: null

----
### getHotlinkProtection
Retrieves a mapping's current Hotlink Protection behavior.

  * **Parameters**:
    * `uniqueId`: the uniqueId of the mapping to which the behavior belongs

  * **Returns**: an object of type
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [View the Hotlink Protection class](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
