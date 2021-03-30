---

copyright:
  years: 2017, 2021
lastupdated: "2021-01-19"

keywords: slapi

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
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:table: .aria-labeledby="caption"}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# CDN API reference
{: #cdn-api-reference}

The SoftLayer Application Programming Interface (commonly called the SLAPI), provided by {{site.data.keyword.cloud}}, is the development interface that gives developers and system administrators direct interaction with the {{site.data.keyword.cloud_notm}} Infrastructure backend system.
{:shortdesc}

The SLAPI implements many of the features in the customer portal: if an interaction is possible in the Customer Portal, it also can be accomplished in the SLAPI. Because you can interact with all portions of the {{site.data.keyword.cloud_notm}} Infrastructure environment programmatically, within the SLAPI, you can use the API to automate tasks.

The SLAPI is a Remote Procedure Call (RPC) system. Each call involves sending data toward an API endpoint and receiving structured data in return. The format used to send and receive data with the SLAPI depends on which implementation of the API you choose. The SLAPI currently uses SOAP, XML-RPC, or REST for data transmission.

For more information about SLAPI, or about the {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN) service APIs, see the following resources in the {{site.data.keyword.cloud_notm}} Development Network:

* [SLAPI Overview](https://softlayer.github.io/ ){:external}
* [Getting Started with SLAPI](https://softlayer.github.io/article/getting-started/ ){:external}
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ ){:external}
* [PHP Soap API Guide](https://softlayer.github.io/article/php/ ){:external}

----

To get started, here is a recommended API call sequence to follow:
* `listVendors` - Provides the list of supported vendors.
* `verifyOrder` - Verifies whether the order can be placed.
* `placeOrder` - Creates the CDN account with a given vendor. Up to 10 CDN mappings can be created after a successful placeOrder call.
* `createDomainMapping` - Creates the CDN Mappings
* `verifyDomainMapping` - Changes CDN status to _RUNNING_

You can use the other APIs after you've followed the previous sequence.

[Example Code is available for each step in this call sequence.](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api)

You must use the API username and API Key of a user with `CDN_ACCOUNT_MANAGE` permission for most of the API calls shown in this document. Check with your account's Master user if you require this permission to be enabled for you. (Each IBM Cloud customer account is provided with one Master user.)
{: note}

----
## API for Vendor
{: #api-for-vendor}

### listVendors
This API allows the user to list the supported CDN Vendors. The `vendorName` is needed to create a CDN account and get started with ordering your CDN.

* **Parameters**- None
* **Return**- (Required) A collection of type `SoftLayer_Container_Network_CdnMarketplace_Vendor`.

  The Vendor container and a usage example can be viewed here: [Vendor container](/docs/CDN?topic=CDN-vendor-container)

----
## API for Account
{: #api-for-account}

### verifyCdnAccountExists
Checks whether a CDN account exists for the user calling the API, for the given `vendorName`.

* **Parameters**- `vendorName`- (Required) Provide the name of a valid CDN provider.
* **Return**- `true` if an account exists, else `false`.

----
## API for Domain Mapping
{: #api-for-domain-mapping}

### createDomainMapping
{: #create-domain-mapping}
Using the provided inputs, this function creates a domain mapping for the given vendor and associates it with the {{site.data.keyword.cloud_notm}} Infrastructure Account ID of the user. The CDN account must first be created by using `placeOrder` for this API to work (see an example of the `placeOrder` API call in the [Code examples](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api). After successfully creating the CDN, a `defaultTTL` is created with a value of 3600 seconds.

* **Parameters**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input container here:

  [Input container](/docs/CDN?topic=CDN-input-container)

  The following attributes are part of the Input container and can be provided when creating a domain mapping (attributes are optional unless otherwise noted):
    * `vendorName` - (Required) Provide the name of a valid IBM Cloud CDN provider.
    * `origin` - (Required) Provide the origin server address as a string.
    * `originType` - (Required) Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain` - (Required) Provide your hostname as a string.
    * `protocol` - (Required) Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `certificateType` - (Required) for HTTPS protocol. `SHARED_SAN_CERT` or `WILDCARD_CERT`.
    * `path` - Path from which the cached content is served. Default path is `/*`.
    * `httpPort`, `httpsPort`, or both - (Required for Host Server) These two options must correspond to the wanted protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. See the [FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for allowed port numbers.
    * `header` - Specifies host header information that is used by the origin server.
    * `respectHeader` - A Boolean value that, if set to `true`, causes TTL settings in the origin to override CDN TTL settings.
    * `cname` - Provide an alias to the hostname. The CNAME is generated if one is not provided.
    * `bucketName` - (Required for Object Storage only) Bucket name for your S3 Object Storage.
    * `fileExtension` - (optional for Object Storage) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule` - The following options are available to configure Cache Key behavior. If no `cacheKeyQueryRule` arguments are supplied, it defaults to "include-all".
      * `include-all` - Includes all query arguments **default**.
      * `ignore-all` - Ignores all query arguments.
      * `ignore: space separated query-args` - Ignores those specific query arguments (for example, `ignore: query1 query2`).
      * `include: space separated query-args` - Includes those specific query arguments (for example, `include: query1 query2`).
    * `dynamicContentAcceleration` - (Required for Dynamic Content Acceleration only) Provide the DCA parameters. [DCA container](/docs/CDN?topic=CDN-dynamic-content-acceleration-container)

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  The collection provides a `uniqueId` value, which must be sent as input for subsequent API calls related to mapping and origin path.
  {: note}

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Deletes the domain mapping based on the `uniqueId`. The domain mapping must be in one of the following states: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME_CONFIGURATION_, or _SSL_CONFIGURATION_.

* **Parameters**- `uniqueId`- (Required) the unique ID of the mapping to be deleted.
* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.
  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Verifies the status of the CDN, and updates the `status` of the CDN mapping if it changed. When a CDN mapping is created initially, its status shows as _CNAME_CONFIGURATION_. At this point, you must update the DNS record so that the CDN mapping points the hostname to the CNAME. Check with your DNS provider if you have questions about how the update is done and how long it might take for the change to propagate on the internet. Typically, it takes 15 - 30 minutes. After that time, this `verifyDomainMapping` API must be called to verify whether the CNAME chain is complete. If the CNAME chain is complete, the CDN-mapping status changes to _RUNNING_.

This API can be called at any time to get the latest CDN-mapping status. The domain mapping must be in one of the following states: _RUNNING_ or _CNAME_CONFIGURATION_.

* **Parameters**- `uniqueId` - (Required) The unique ID of the mapping that you want to verify.
* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Starts a CDN domain mapping based on the `uniqueId`. To be started, the domain mapping must be in a _STOPPED_ state.

* **Parameters**- `uniqueId` - (Required) Provide the unique ID of the mapping to be started.
* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Stops a CDN domain mapping based on the `uniqueId`. To initiate the stop, the domain mapping must be in a _RUNNING_ state.

* **Parameters**- `uniqueId` - (Required) Provide the unique ID of the mapping to be stopped.
* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### retryHttpsActionRequest
You can attempt to enable the HTTPS `SHARED_SAN_CERT` type mapping again or disable when mapping is in `DELETE_ERROR` state. For the retry or disable request to occur, the specified mapping should be an HTTPS-related error, or in `DELETE_ERROR` state. To re-attempt an enable or disable action, you must specify the `uniqueId` of the HTTPS `SHARED_SAN_CERT` type mapping.

* **Parameters**- `uniqueId` - (Required) Provide the unique ID of the HTTPS mapping to be enabled.
* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
You can update properties of the mapping identified by the `uniqueId`. The following fields can be changed: `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header`, `cacheKeyQueryRule` arguments, and if your origin type is Object Storage, the `bucketName` and `fileExtension` also can be changed. For an update to occur, the domain mapping must be in a _RUNNING_ state.

* **Parameters**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input container here:
  [Input container](/docs/CDN?topic=CDN-input-container)

  The following attributes are part of the Input container and are **required** to be provided when updating a domain mapping:
    * `vendorName` - Provide the name of the CDN provider for this mapping.
    * `path` - Provide the current path for this mapping
    * `origin` - Provide the origin server address as a string.
    * `originType` - Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain` - Provide your hostname.
    * `protocol` - Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `httpPort`, `httpsPort`, or both - These two options must correspond to the wanted protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. See the [FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for allowed port numbers.
    * `header` - Specifies host header information that is used by the origin server.
    * `respectHeader` - A Boolean value that, if set to `true`, causes TTL settings in the origin to override CDN TTL settings.
    * `uniqueId` - Generated after the mapping is created.
    * `cname` - Provide the cname. One was generated when the mapping was created if you did not provide one.
    * `bucketName` - (Required for Object Storage only) Bucket name for your S3 Object Storage.
    * `fileExtension` - (Required for Object Storage only) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule` - Cache key behavior rules can be updated only for CDN mappings created _after_ 11/16/17. The following options are available to configure Cache Key behavior:
      * `include-all` - Includes all query arguments **default**.
      * `ignore-all` - Ignores all query arguments.
      * `ignore: space separated query-args` - Ignores those specific query arguments (for example, `ignore: query1 query2`).
      * `include: space separated query-args`- Includes those specific query arguments (for example, `include: query1 query2`).
    * `dynamicContentAcceleration`- (Required for Dynamic Content Acceleration only) Provide the DCA parameters. [DCA container](/docs/CDN?topic=CDN-dynamic-content-acceleration-container)
* **Return** A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Returns a collection of all domain mappings for current customer.

* **Parameters**- None
* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Returns a collection with a single domain object based on a CDN's `uniqueId`.

* **Parameters**- `uniqueId`- Provide the unique ID of the mapping to be returned.
* **Return**- A single-object collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  [Mapping container](/docs/CDN?topic=CDN-mapping-container)

----
## APIs for origin
{: #apis-for-origin}

### createOriginPath
Creates an origin path for an existing CDN and for a particular customer. The origin path can be based on a Host Server or Object Storage. To create the origin path, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input container here:

  [Input container](/docs/CDN?topic=CDN-input-container)

  The following attributes are part of the Input container and can be provided when creating an origin path (attributes are optional unless otherwise noted):
    * `vendorName`- (Required) Provide the name of a valid IBM Cloud CDN provider.
    * `origin`- (Required) Provide the origin server address as a string.
    * `originType`- (Required) Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `domain`- (Required) Provide your hostname as a string.
    * `protocol`- (Required) Supported protocols are `HTTP`, `HTTPS`, or `HTTP_AND_HTTPS`.
    * `path`- Path from which the cached content is served. Must begin with the mapping path. For example, if the mapping path is `/test`, then your origin path might be `/test/media`.
    * `httpPort`, `httpsPort`, or both- (Required) These two options must correspond to the wanted protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. See the [FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for allowed port numbers.
    * `header`- Specifies host header information that is used by the origin server.
    * `uniqueId`- (Required) Generated after the mapping is created.
    * `cname`- Provide an alias to the hostname. If you did not provide a unique CNAME, one was generated for you when the mapping was created.
    * `bucketName`- (Required for Object Storage) Bucket name for your S3 Object Storage.
    * `fileExtension`- (optional for Object Storage) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule`- The following options are available to configure Cache Key behavior:
      * `include-all` - Includes all query arguments **default**.
      * `ignore-all` - Ignores all query arguments.
      * `ignore: space separated query-args` - Ignores those specific query arguments (for example, `ignore: query1 query2`).
      * `include: space separated query-args`: Includes those specific query arguments (for example, `include: query1 query2`).
    * `dynamicContentAcceleration`- (Required for Dynamic Content Acceleration only) Provide the DCA parameters. [DCA container](/docs/CDN?topic=CDN-dynamic-content-acceleration-container)

* **Return**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`.

  [Origin path container](/docs/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Updates an existing origin path for an existing mapping and for a particular customer. The origin type cannot be changed with this API. The following properties can be changed: `path`, `origin`, `httpPort`, and `httpsPort`, `header` `cacheKeyQueryRule` arguments. To be updated, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**- A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  You can view all of the attributes in the Input container here:

  [Input container](/docs/CDN?topic=CDN-input-container)

  The following attributes are part of the Input container and can be provided when updating an origin path (attributes are optional unless otherwise noted):
    * `oldPath`- (Required) Current path to be changed
    * `origin`- (Required if being updated) Provide the origin server address as a string.
    * `originType`- (Required) Origin type can be `HOST_SERVER` or `OBJECT_STORAGE`.
    * `path`- (Required) New path to be added. Relative to the mapping path.
    * `httpPort`, `httpsPort`, or both - (Required for Host Server, if being updated) These two options must correspond to the wanted protocol. If the protocol is `HTTP`, then `httpPort` must be set, and `httpsPort` must _not_ be set. Likewise, if the protocol is `HTTPS`, then `httpsPort` must be set, and `httpPort` must _not_ be set. If the protocol is `HTTP_AND_HTTPS`, then _both_ `httpPort` and `httpsPort` _must_ be set. Akamai has certain limitations on port numbers. See the [FAQs](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for allowed port numbers.
    * `uniqueId`- (Required) the unique ID of the mapping to which this origin belongs.
    * `bucketName`- (Required for Object Storage only) Bucket name for your S3 Object Storage.
    * `fileExtension`- (Required for Object Storage only) File extensions that are allowed to be cached.
    * `cacheKeyQueryRule`- (Required if being updated) Cache key behavior rules can be updated only for origin paths created _after_ 11/16/17. The following options are available to configure Cache Key behavior:
      * `include-all` - Includes all query arguments **default**.
      * `ignore-all` - Ignores all query arguments.
      * `ignore: space separated query-args` - Ignores those specific query arguments (for example, `ignore: query1 query2`).
      * `include: space separated query-args` - Includes those specific query arguments (for example, `include: query1 query2`).
    * `dynamicContentAcceleration`- (Required for Dynamic Content Acceleration only) Provide the DCA parameters. [DCA container](/docs/CDN?topic=CDN-dynamic-content-acceleration-container)
* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`.

  [Origin path container](/docs/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Deletes and existing origin path for an existing CDN, and for a particular customer. To be deleted, the domain mapping must be in either a _RUNNING_ or _CNAME_CONFIGURATION_ state.

* **Parameters**:
  * `uniqueId`- (Required) The unique ID of the mapping to which this origin path belongs.
  * `path`- (Required) Path to be deleted.

* **Return** - A status message if the delete was successful, otherwise an exception is thrown.

----
### listOriginPath
Lists the origin paths for an existing mapping based on the `uniqueId`.

* **Parameters**:
  * `uniqueId`- (Required) Provide the unique ID of the mapping for which you want to list origin paths.
* **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`.

  [Origin path container](/docs/CDN?topic=CDN-path-origin-container)

----
## API for Multiple File Purge
{: #api-for-multiple-file-purge}

### createPurgeGroup
{: #api-for-createpurgegroup}

Creates a purge group. A purge group contains multiple file paths and allows you to purge all of these files in one API call. You can save a purge group to be a favorite (permanent) group or not. For more information, see [With multiple file purges, what's the difference between a favorite group and an unfavorite group?](/docs/CDN?topic=CDN-faqs#what-different-two-type-favorite-group).   

There's also a rate limit to the purge actions. For more information, see [What's the rate limit for a multiple file purge?](/docs/CDN?topic=CDN-limits-and-maximum-values#purge-rate-limit).  

For an example of how to create a purge group, see [Example code for creating a purge group](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#create-group-example).  

* **Parameters**:
   * `uniqueId` - The unique ID of the CDN mapping for which the purge group was created.
   * `name` - Purge group name. The favorite group name must be unique. Unfavorite groups do not have this limitation.
   * `paths` - A collection of purge paths.
   * `option` - The following options are available to create a purge group:
      * `1` - Create the purge group as an unfavorite group, and run a purge action. **Default**
      * `2` - Create the purge group as a favorite group, and do not run a purge action.
      * `3` - Create the purge group as a favorite group, and run a purge action.

* **Return** - `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`.

  [Purge Group container](/docs/CDN?topic=CDN-purge-group-container)

----
### getPurgeGroupByGroupId
{: #api-for-getpurgegroup}

Gets a purge group.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping.
  * `groupUniqueId` - The unique ID of the purge group.

* **Return** - `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`.

  [Purge Group container](/docs/CDN?topic=CDN-purge-group-container)

----
### listFavoriteGroup
{: #api-for-listfavoritegroup}

Lists favorite purge groups.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping.

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`.

  [Purge Group container](/docs/CDN?topic=CDN-purge-group-container)

----
### listUnfavoriteGroup
{: #api-for-listunfavoritegroup}

Lists unfavorite purge groups.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping.

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`.

  [Purge Group container](/docs/CDN?topic=CDN-purge-group-container)

----
### savePurgeGroupAsFavorite
{: #save-purgegroup-as-favorite}

Saves a specific purge group as favorite.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping to which the purge group belongs.

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`.

  [Purge Group container](/docs/CDN?topic=CDN-purge-group-container)

----
### removePurgeGroupFromFavorite
{: #remove-purgegroup-as-favorite}

Removes a specific purge group from favorite.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping to which the purge group belongs.

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup`.

  [Purge Group container](/docs/CDN?topic=CDN-purge-group-container)

----
### purgeByGroupIds
{: #purge-by-group}

Purges all files in purge groups by a collection of group IDs.  
Purge actions have a rate limit check. See the [FAQ](/docs/CDN?topic=CDN-limits-and-maximum-values#purge-rate-limit) for more details.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping.
  * `groupUniqueIds` - A collection of purge group IDs.

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeHistory`.

  [Purge History container](/docs/CDN?topic=CDN-purge-history-container)

----
### getPurgeGroupQuota
{: #get-purgegroupquota}

Returns the file path quota of a purge group.

* **Return** - An int type of quota information.

----
## API for Purge History
{: #api-for-purge-history}

----
### listPurgeGroupHistory
{: #list-purgegroup-history}

Returns the purge actions history for a domain mapping.  
The purge history retains the last 15 days of records.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping.

* **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeHistory`.

  [Purge History container](/docs/CDN?topic=CDN-purge-history-container)

----
## API for Single File Purge (Deprecated)
{: #api-for-purge}

The Purge API for purging a single file is deprecated. For efficiency, it is recommended to use [Multiple file Purge](#api-for-multiple-file-purge) instead.
{:deprecated}

### Container class for Purge
{: #container-class-for-purge}

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
{: #create-purge}

Create a single file purge.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping to which the Purge is created.
  * `path` - Path of the Purge to be created.

* **Return** - a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
{: #get-purge-history-per-mapping}

Returns the Purge history for a CDN based on the `uniqueId` and `saved` status. (By default, the value of `saved` is set to _UNSAVED_.)

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping for which retrieve Purge history.
  * `saved` - Return Purges that were _SAVED_ or _UNSAVED_.

* **Return** - a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
{: #save-or-unsave-purge-path}

Updates the status of the Purge path entry to indicate whether that Purge path should be saved. Creates a new `saved` Purge if a Purge path is saved. Deletes a saved Purge record if the path is `unsaved`.

* **Parameters**:
  * `uniqueId` - The unique ID of the mapping to which the Purge belongs.
  * `path` - Path of the Purge to be saved or unsaved.
  * `saved` - _SAVED_ or _UNSAVED_

* **Return** - a collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

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
Creates a `TimeToLive` object.

 * **Parameters** - `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Return** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`.
___
### updateTtl
Updates an existing `TimeToLive` object. If the _oldTtl_ and _newTtl_ inputs are equal, it exits early.

 * **Parameters** - `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Returns** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`.
___
### deleteTtl
Deletes an existing `TimeToLive` object.

 * **Parameters** - `string` `uniqueId`, `string` `pathName`
 * **Return** - A string with the status of the delete.
___
### listTtl
Lists existing `TimeToLive` objects based on a CDN's `uniqueId`.

 * **Parameters** - `string` `uniqueId`
 * **Return** - An array of objects of type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`.

 ----
## API for Metrics
{: #api-for-metrics}

[Metrics container](/docs/CDN?topic=CDN-metrics-container)

All metrics time is UTC+0.
{:note}

### getCustomerInvoicingMetrics
Returns all mappings' bandwidth and hits (including static and dynamic separately) of predetermined statistics for direct display for a customer's account, over a given period of time with granularity of day.

 * **Parameters**:
   * `vendorName` - Provide the name of the CDN provider for this customer. Currently, only `akamai` is supported.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - Daily metrics data from `startDate` to `endDate`.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getCustomerUsageMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a customer's account, over a given period of time.

 * **Parameters**:
   * `vendorName` - Provide the name of the CDN provider for this customer. Currently, only `akamai` is supported.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:
      * `aggregate` - aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - daily metrics data from `startDate` to `endDate`.

 * **Return** - a collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingIntegratedMetrics
Returns the integrated metrics data for the given mapping, which includes the total number of predetermined statistics, detailed hits and bandwidth, hits by type, and the bandwidth by region. The value of `frequency` is `day` by default. For more details, see [View the examples](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#example-code-for-getting-integrated-metrics).

 * **Parameters**:
   * `mappingUniqueId` - Provide a unique ID of the mapping for which metrics are queried.
   * `startTime` - Provide the start date timestamp (UTC+0) for query.
   * `endTime` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format. The following options are available to configure frequency:
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate`.
      * `day` - Daily metrics data from `startDate` to `endDate` **default**.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getMappingUsageMetrics
Returns the total number of predetermined statistics for direct display for the given mapping. The value of `frequency` is `aggregate` by default.

 * **Parameters**:
   * `mappingUniqueId` - Provide a unique ID of the mapping for which metrics are queried.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - Daily metrics data from `startDate` to `endDate`.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getMappingHitsMetrics
Returns the total number of hits at a certain frequency over a given range of time, per domain mapping. Frequency can be `day`, `week`, and `month`, where each interval is one plot point for a graph. Return data is ordered based on `startDate`, `endDate`, and `frequency`. The value of `frequency` is `aggregate` by default.

 * **Parameters**:
   * `mappingUniqueId` - Provide a unique ID of the mapping for which metrics are queried.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - Daily metrics data from `startDate` to `endDate`.
      * `week` - Weekly metrics data from `startDate` to `endDate`.
      * `month` - Monthly metrics data from `startDate` to `endDate`.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Returns the total number of hits at a certain frequency over a given range of time. Frequency can be `day`, `week`, and `month`, where each interval is one plot point for a graph. Return data must be ordered based on `startDate`, `endDate`, and `frequency`. The value of `frequency` is `aggregate` by default.

 * **Parameters**:
   * `mappingUniqueId` - Provide a unique ID of the mapping for which metrics are queried.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - Daily metrics data from `startDate` to `endDate`.
      * `week` - Weekly metrics data from `startDate` to `endDate`.
      * `month` - Monthly metrics data from `startDate` to `endDate`.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getMappingBandwidthMetrics
Returns the quantity of bandwidth in GB for an individual CDN. Regions might differ for each vendor, per mapping.

 * **Parameters**:  
   * `mappingUniqueId` - Provide a unique ID of the mapping for which metrics are queried.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:   
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - Daily metrics data from `startDate` to `endDate`.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getMappingBandwidthByRegionMetrics
Returns the total number of predetermined statistics for direct display (no graph) for a given mapping, over a given period of time. The value of `frequency` is `aggregate` by default.

 * **Parameters**:
   * `mappingUniqueId` - Provide a unique ID of the mapping for which metrics are queried.
   * `startDate` - Provide the start date timestamp (UTC+0) for query.
   * `endDate` - Provide the end date timestamp (UTC+0) for query.
   * `frequency` - Provide the data frequency for data format, the following options are available to configure frequency:
      * `aggregate` - Aggregated metrics data from `startDate` to `endDate` **default**.
      * `day` - Daily metrics data from `startDate` to `endDate`.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getCustomerRealTimeMetrics
Returns the real-time metrics for the current account, including the total data (bandwidth and hits) over the given period of time and the detailed data divided with the given time interval. For more details, [View the examples](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#example-code-for-getting-real-time-metrics).

 * **Parameters**:
   * `vendorName` - The name of the CDN provider. Currently, only `akamai` is supported.
   * `startTime` - The start time timestamp (UTC+0) for query. The start time must be later than 48 hours ago. For example, if it's 9:00 AM 08 March 2020 now, the start time should be later than 9:00 AM 06 March 2020 in the same time zone(The timestamp should be more than 1583485200).
   * `endTime` - The end time timestamp (UTC+0) for query must be later than the start time.
   * `timeInterval` - An optional parameter to provide the time interval in seconds.
        The time interval must be a multiple of 60 and should be less than the time range from start time to end time.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.
___
### getMappingRealTimeMetrics
Returns the real-time metrics for the given mapping, including the total data (bandwidth and hits) over the given period of time and the detailed data divided with the given time interval. For more details, [View the examples](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#example-code-for-getting-real-time-metrics).

 * **Parameters**:
   * `mappingUniqueId` - Unique ID of the mapping for which metrics are queried.
   * `startTime` - The start time timestamp (UTC+0) for query. The start time must be later than 48 hours ago. For example, if it's 9:00 AM March 08 2020 now, the start time should be later than 9:00 AM March 06 2020 in the same time zone(the timestamp should be more than 1583485200).
   * `endTime` - The end time timestamp (UTC+0) for query must be later than the start time.
   * `timeInterval` - An optional parameter to provide the time interval in seconds.
        The time interval must be a multiple of 60 and should be less than the time range from start time to end time.

 * **Return** - A collection of objects of type `SoftLayer_Container_Network_CdnMarketplace_Metrics`.

----
## API for Geographical Access Control
{: #api-for-geographical-access-control}

### createGeoblocking
Creates a new Geographical Access Control rule, and returns the newly created rule.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input container here:

    [Input container](/docs/CDN?topic=CDN-input-container)

    The following attributes are part of the Input container and are **required** when creating a new Geographical Access Control rule:
    * `uniqueId` - Provide a unique ID of the mapping to assign the rule.
    * `accessType` - Specifies whether the rule will `ALLOW` or `DENY` traffic to the given region.
    * `regionType` - The type of region to apply the Geographical Access Control rule to, either `CONTINENT` or `COUNTRY_OR_REGION`.
    * `regions` - An array listing the locations that the `accessType` applies to.

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/CDN?topic=CDN-geoblocking-class) page to see a list of possible regions.

  * **Returns** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`.

    [Geo-blocking class](/docs/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Updates an existing Geographical Access Control rule for an existing domain mapping and returns the updated rule.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input container here:

    [Input container](/docs/CDN?topic=CDN-input-container)

    The following attributes are part of the Input container and can be provided when updating a GeographicalAccess Control rule (parameters are optional unless otherwise noted):
    * `uniqueId` -  (Required) Provide a unique ID of the mapping the rule to be updated belongs.
    * `accessType` - Specifies whether the rule will `ALLOW` or `DENY` traffic to the given region.
    * `regionType` - The type of region to apply the rule to, either `CONTINENT` or `COUNTRY_OR_REGION`.
    * `regions` - An array listing the locations that the `accessType` applies to.

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/CDN?topic=CDN-geoblocking-class) page to see a list of possible regions.

  * **Returns** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`.

    [Geo-blocking class](/docs/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Removes an existing Geographical Access Control rule from an existing domain mapping.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input container here:

    [Input container](/docs/CDN?topic=CDN-input-container)

    The following attribute is part of the Input container and is **required** when deleting a Geographical Access Control rule:
    * `uniqueId` - Provide a unique ID of the mapping that the rule to be deleted belongs to.

  * **Returns** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`.

    [Geo-blocking class](/docs/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Retrieves a mapping's Geographical Access Control behavior.

  * **Parameters**:
    * `uniqueId` - The unique ID of the mapping that the rule belongs to.

  * **Returns** - An object of type
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`.

    [Geo-blocking class](/docs/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Returns a list of the types and regions that are allowed for creating Geographical Access Control rules.

  * **Parameters**:
    * `uniqueId`: The unique ID of your domain mapping.

  * **Returns**: An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`.

    [Geo-blocking class](/docs/CDN?topic=CDN-geoblocking-class)
----
## API for Hotlink Protection
{: #api-for-hotlink-protection}

### createHotlinkProtection
Creates a new Hotlink Protection, and returns the newly created behavior.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input container here:

    [Input container](/docs/CDN?topic=CDN-input-container)

    The following attributes are part of the Input container and are **required** when creating a new Hotlink Protection:
    * `uniqueId` - Provide the unique ID of the mapping to which to assign the behavior.
    * `protectionType` - Specifies whether to "ALLOW" or "DENY" access to your content when a webpage makes a request for content with a Referer header value matching one of the terms in the specified referer values.
    * `refererValues` - A single-space-separated list of Referer URL match terms for which the `protectionType` behavior takes effect.

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/CDN?topic=CDN-hotlink-protection-class) page to see a list of valid Hotlink Protection values.

  * **Returns** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`.

    [Hotlink Protection class](/docs/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Updates an existing Hotlink Protection behavior for an existing domain mapping and returns the updated behavior.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input container here:

    [Input container](/docs/CDN?topic=CDN-input-container)

    The following attributes are part of the Input container and are **required** when updating an existing Hotlink Protection:
    * `uniqueId` - The unique ID of the mapping to which the existing behavior belongs.
    * `protectionType` - Specifies whether to "ALLOW" or "DENY" access to your content when a webpage makes a request for content with a Referer header value matching one of the terms in the specified refererValues.
    * `refererValues` - A single-space-separated list of Referer URL match terms for which the `protectionType` behavior takes effect.

      See the [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/CDN?topic=CDN-hotlink-protection-class) page to see a list of valid Hotlink Protection values.

  * **Returns** - An object of type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`.

    [Hotlink Protection class](/docs/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Removes an existing Hotlink Protection behavior from an existing domain mapping.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    You can view all of the attributes in the Input container here:

    [Input container](/docs/CDN?topic=CDN-input-container)

    The following attributes are part of the Input container and are **required** when creating a new Hotlink Protection:
    * `uniqueId` - Provide the unique ID of the mapping from which to remove the behavior.

  * **Returns** - null

----
### getHotlinkProtection
Retrieves a mapping's current Hotlink Protection behavior.

  * **Parameters**:
    * `uniqueId` - Provide the unique ID of the mapping to which the behavior belongs.

  * **Returns** - An object of type
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`.

    [Hotlink Protection class](/docs/CDN?topic=CDN-hotlink-protection-class)

----
## API for Token Authentication
{: #api-for-token-authentication}

### createTokenAuth
Creates a Token Authentication path for a domain mapping and returns the newly created behavior.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth`.
    You can view all of the attributes in the Input container here:

    [Token authentication input container](/docs/CDN?topic=CDN-token-auth-container)

    The following attributes are part of the Token Authentication container input, which can be provided when creating a new Token Authentication. Attributes are optional unless otherwise noted.
    * `uniqueId` - (Required) The unique ID of the mapping to which the existing behavior belongs.
    * `path` - (Required) The path, relative to the domain that is accessed through token authentication.
    * `name` - The token name. If this value is empty, then it is set to the default value `__token__`.
    * `tokenKey` - (Required) The token encryption key, which specifies an even number of hex digits for the token key. An entry can be up to 64 characters in length.
    * `escapeTokenInputs` - Possible values `0` and `1`. If set to `1`, input values are escaped before adding them to the token. Default value is `1`.
    * `ignoreQueryString` - Possible values `0` and `1`. If set to `1`, query strings are removed from a URL when computing the token’s HMAC algorithm. Default value is `0`.
    * `tokenDelimiter` - Specifies a single character to separate the individual token fields. The default value is `~`.
    * `aclDelimiter` - Specifies a single character to separate access control list (ACL) fields. The default value is `!`.
    * `hmacAlgorithm` - Specifies the algorithm to use for the token’s hash-based message authentication code (HMAC) field. Valid entries are `SHA256`, `SHA1`, or `MD5`. The default value is `SHA256`.
    * `transitionKey` - The token transition key, which specifies an even number of hex digits for the token transition key. An entry can be up to 64 characters in length.

  * **Returns** - An object of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth`.

See this example to [create a token auth](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#create-token-auth-example).

----
### updateTokenAuth
Updates an existing Token Authentication path for an existing domain mapping and returns the updated behavior.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth`.
    You can view all of the attributes in the Input container here:

    [Token authentication input container](/docs/CDN?topic=CDN-token-auth-container)

    The following attributes are part of the Token Authentication container input, which can be provided when updating a Token Authentication. Attributes are optional unless otherwise noted.
    * `uniqueId` - (Required) The unique ID of the mapping to which the existing behavior belongs.
    * `path` - (Required) The path, relative to the domain that is accessed via token authentication.
    * `name` - The token name.
    * `tokenKey` - The token key, which specifies an even number of hex digits for the token key. An entry can be up to 64 characters in length.
    * `escapeTokenInputs` - Possible values `0` and `1`. If set to `1`, input values are escaped before adding them to the token. Default value is `1`.
    * `ignoreQueryString` - Possible values `0` and `1`. If set to `1`, query strings are removed from a URL when computing the token’s HMAC algorithm. Default value is `0`.
    * `tokenDelimiter` - Specifies a single character to separate the individual token fields.
    * `aclDelimiter` - Specifies a single character to separate access control list (ACL) fields.
    * `hmacAlgorithm` - Specifies the algorithm to use for the token’s hash-based message authentication code (HMAC) field.
    * `transitionKey` - The token transition key, which specifies an even number of hex digits for the token transition key. An entry can be up to 64 characters in length.

  * **Returns** - An object of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth`.

----
### deleteTokenAuth
Delete an existing Token Authentication behavior for an existing domain mapping and returns the updated behavior.

  * **Parameters**:
    * `uniqueId` - (Required) The unique ID of the mapping to which this origin path belongs.
    * `path` - (Required) Token Authentication Path to be deleted.

  * **Return** - A status message if the deletion was successful; otherwise, an exception is thrown.
----
## API for modify-response-header
{: #api-for-modify-response-header}

### listModifyResponseHeader
Returns a collection of `modify-response-header` behaviors for a domain mapping.

  * **Parameters**:
    * `uniqueId` - (Required) The unique ID of the mapping to which the `modify-response-header` belongs.

  * **Return** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`.
---
### createModifyResponseHeader
Creates a `modify-response-header` behavior for a domain mapping and returns the newly created behavior.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`.
    You can view the input attributes in the [Modify-response-header input container](/docs/CDN?topic=CDN-modify-response-header-container).

    The following attributes are part of `modify-response-header` container input, which can be provided when creating a `modify-response-header`. Attributes are optional unless otherwise noted.

    * `uniqueId` - (Required) The unique ID of the mapping to which the existing behavior belongs.
    * `path` - (Required) The path from which the `modify-response-header` is served.
    * `type` - (Required) The type of `modify-response-header`. Types are `append`, `modify` or `delete`.
    * `headers` - (Required) A collection of key-value pairs that specify the headers and associated values to be modified. The header name and header value must be separated by a colon (:). For example: ['header1:value1','header2:Value2']
    * `delimiter` - The delimiter to be used when indicating multiple values for a header. Valid values are a space, comma (default), semicolon, comma followed by a space, or a semicolon followed by a space.
    * `description` - The description of `modify-response-header`.

  * **Returns** - An object of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`.

See this example to [create a `modify-response-header`](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#create-modify-response-header-example).

----
### updateModifyResponseHeader
Updates an existing `modify-response-header` for an existing domain mapping and returns the updated behavior.

  * **Parameters**: A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`.
    You can view all of the attributes in the Input container here:

    [`Modify-response-header` input container](/docs/CDN?topic=CDN-modify-response-header-container)

    The following attributes are part of the `modify-response-header` container input, which can be provided when a `modify-response-header` is updated. Attributes are optional unless otherwise noted.

    * `uniqueId` - (Required) The unique ID of the mapping to which the existing behavior belongs.
    * `modResHeaderUniqueId` - (Required) The unique ID of the existing `modify-response-header`.
    * `path` - The path from which the `modify-response-header` is served.
    * `type` - (Required) The type of `modify-response-header`. Values are `append`, `modify` or `delete`.
    * `headers` - A collection of key-value pairs that specify the headers and associated values to be modified. The header name and header value must be separated by a colon (:). For example: ['header1:value1','header2:Value2'].
    * `delimiter` - The delimiter to be used when indicating multiple values for a header. Valid values are a space, comma (default), semicolon, comma followed by a space, or a semicolon followed by a space.
    * `description` - The description of modify-response-header.

  * **Returns** - A collection of type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_ModifyResponseHeader`.

----
### deleteModifyResponseHeader
Deletes an existing modify-response-header behavior for an existing domain mapping and returns the updated behavior.

  * **Parameters**:
    * `uniqueId` - (Required) The unique ID of the mapping to which the existing behavior belongs.
    * `modResHeaderUniqueId` - (Required) The unique ID of the existing `modify-response-header`.

  * **Return** - A status message if the deletion was successful; otherwise, an exception is thrown.
