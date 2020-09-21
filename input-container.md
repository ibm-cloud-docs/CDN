---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-06"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Input container
{: #input-container}

The Input container is a collection used by both the Mapping and (Origin) Path classes. It provides a consistent set of input attributes for both classes.
{:shortdesc}

* `vendorName`: The name of a valid {{site.data.keyword.cloud}} CDN provider.
* `oldPath`: Used by updateOriginPath(). This property stores the name of the current, or 'old', path.

The following attributes are common to the Mapping and (Origin) Path classes:
* `originType`: Type of the origin host, currently `HOST_SERVER` or `OBJECT_STORAGE`.
* `origin`: Origin server address (either the hostname or the IPv4 address of the origin server), the endpoint from which to fetch content, or the name of the bucket where content is stored. Must be fewer than 511 characters.
* `httpPort`: Number of the port used for HTTP protocol. Akamai has certain limitations on port numbers for HTTP and HTTPS ports. See the [FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for a list of allowed port numbers.
* `httpsPort`: Number of the port used for HTTPS protocol. Akamai has certain limitations on port numbers for HTTP and HTTPS ports. See the [FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for a list of allowed port numbers.
* `status`:  The status of the mapping or path. Status can be CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED, or ERROR.
* `path`: Path from which the cached content is served. The default path is /\* When used by the `updateOriginPath` API, this attribute refers to the new path to be added.
* `performanceConfiguration`: Specifications for the performance configuration of the mapping. We support the following performance configurations:
  * `General web delivery`: The default performance option to optimize the general web page contents, such as the HTML, pictures, CSS, JS files.
  * `Dynamic content acceleration`: Optimizes the forward path to the origin server using Akamai’s “SureRoute” feature, and supports embedded object pre-fetching and situational image compression.
  * `Video on demand optimization`: Optimizes cache/offload and media retrieval path through the Akamai network, MIME types, network timeout conditions.
  * `Large file optimization`: Optimizes for delivery of large file downloads with improved cache/offload (partial object caching with pre-fetched object data), object retrieval path.  
* `cacheKeyQueryRule`: The following options are available to configure Cache Key behavior:
  * `include-all`: Include all query arguments
  * `ignore-all`: Ignore all query arguments
  * `ignore: space separated query-args`: Ignores those specific query arguments. For example, `ignore: query1 query2`
  * `include: space separated query-args`: Includes those specific query arguments. For example, `include: query1 query2`
* `geoblockingRule`

The following attributes are specific to the Mapping class:

* `uniqueId`: A 10-digit, system-generated, identifier that is unique to each mapping. Generated when a mapping is created.
* `domain`: Primary CDN name. Also referred to as hostname.
* `protocol`: Protocol used to set up services. Can be HTTP, HTTPS, or a combination of the two, HTTP_AND_HTTPS.
* `cname`: Canonical name record aliases the hostname. Can be provided by the user, or system-generated. If user-provided, it should be less than 64 alphanumeric characters and must be unique. If not provided, one is generated when the mapping is created.
* `certificateType`: Type of certificate being used by a mapping. Can be `WILDCARD_CERT` or `SHARED_SAN_CERT`. Value is 0 for HTTP mappings.
* `respectHeaders`: A Boolean value that, if set to 'true', causes TTL settings in the origin to override CDN TTL settings.
* `header`: Specifies host header information that is used by the origin.

The following attributes are related to Cloud Object Storage (COS):  
* `bucketName`: Unique name of your bucket for S3 object storage.  
* `fileExtension`: File extensions that are allowed.

The following attribute is related to configuring Hotlink Protection:
* `hotlinkProtection`: See the [hotlink protection class](/docs/CDN?topic=CDN-hotlink-protection-class) for more details.

The following attribute is related to Dynamic Content Acceleration (DCA):
* `dynamicContentAcceleration`: See the [Dynamic Content Acceleration class](/docs/CDN?topic=CDN-dynamic-content-acceleration-container) for more details.
