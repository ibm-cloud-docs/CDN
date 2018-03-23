---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# About Content Delivery Networks (CDN)

A Content Delivery Network (CDN) is a collection of edge servers that are distributed through various parts of the country or the world. Web content is served from an edge server, which is located in the geographic area closest to the customer who requests the content. This technique lets your end-users receive the content with less delay, and it delivers a better overall experience for your customers.

## How does a CDN work?

A CDN achieves its purpose by caching web content on edge servers around the world. When a customer requests web content, the content request is routed to the edge server that is geographically closest to that customer. By reducing the distance the content must travel, the CDN offers optimized throughput, minimized latency, and increased performance. Using IBM Cloud Content Delivery Network with Akamai, content providers can realize efficient delivery of requested content from around the globe, with minimal configuration.

The key features of the IBM Cloud Content Delivery Network service include:
  * Host Server Origin support
  * Object Storage Origin support
  * Support for Multiple Origins with distinct paths
  * Path-based CDN mappings
  * Purge cached content
  * Time to Live (TTL)
  * Metrics with graphical views
  * HTTPS support with wildcard certificate
  * Host Header support
  * Serve Stale content
  * "Ignore Query Args in Cache Key" feature
  * Content Compression
  * Large File Optimization
  * Video on Demand

## Feature Descriptions

### Host Server Origin support

IBM Cloud Content Delivery Network (CDN) can be configured to serve content from a Host Server Origin by providing the Origin hostname, protocol, port number, and optionally, the path from which to serve the content. The default path is '/\*'. Protocol can be HTTP, HTTPS, or both. Only certain port numbers are supported by Akamai. Please see the [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for supported port numbers/ranges. Currently, HTTPS is supported using Wildcard Certificate, which means the service will be accessible only through the CNAME ending with `.cdnedge.bluemix.net`. Please refer to the [HTTPS with Wildcard Certificate](about.html#https-with-wildcard-certificate-) feature description for more information.

### Object Storage Origin support

IBM Cloud CDN can be configured to serve content from an Object Storage endpoint by providing the endpoint, the Bucket name, protocol, and port. Optionally, a list of file extensions can be specified to only allow caching for files with those extensions. This tutorial on [How to setup IBM Bluemix Object Storage with CDN](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) provides more detailed information.

### Support for Multiple Origins with distinct paths

In certain cases, you may want to deliver certain content from a different Origin server. For example, you may want certain photos or videos served from different Origin servers. IBM Cloud CDN provides the option to set up multiple Origin servers with multiple paths. This allows flexibility regarding how and where the data is stored. The path provided for the Origin server should be unique with respect to the CDN. The Origin server itself does not need to be unique.

### Path-based CDN mappings

Your IBM Cloud CDN service can be restricted to a particular directory path on the Origin server by providing the path when creating the CDN. An end-user is allowed access only to those contents in that directory path. For example if a CDN www.example.com is created with Path '/videos', it is accessible only via `www.example.com/videos/`.

### Purge cached content

IBM Cloud CDN provides the capability to conveniently and quickly remove, or "purge", the cached content from the Edge servers. At this time, the purge is allowed only for a file path. Currently, the Purge implementation with Akamai purges the file in about 5 seconds. The UI also provides the ability to view your Purge history, and to tag specific Purge paths as Favorites.

### Time to Live (TTL)

The Time To Live indicates the amount of time (in seconds) the Edge server will cache the content for that particular file or directory path. When a CDN is first created, a global Time To Live (TTL) is created for path ('/\*') with a default time of 3600 seconds. The minimum value for TTL is 30 seconds, and the maximum value is 2147483647 seconds. For each entry, the TTL path should be unique for the CDN. If multiple paths match a given content, the most recently configured path match applies to that content. For example, consider two TTLs, `/example/file` created first with a time to live value of 3000 seconds and `/example/*` is created later, with a value of 4000 seconds. Although `/example/file` is more specific, `/example/*` was created most recently, so the TTL for `/example/file` will be 4000 seconds. Once created, TTL entries can be edited for path and/or time. They can be deleted as well.

### Metrics with graphical views

Metrics for an individual CDN are provided on the Overview tab of the Customer Portal for that CDN Mapping. Two types of metrics are calculated from the CDN's usage: those that show the metrics over a time period as a graph; and those that are shown as aggregate values.

For metrics that display the change over a period of time as a graph, you can see three line graphs and a pie chart. The three line graphs are: **Bandwidth**, **Hits by Mapping**, and **Hits by Type**. They display the activity on a daily basis over the course of your specified time-frame. The graphs for **Bandwidth** and **Hits by Mapping** are single-line graphs, whereas the breakdown of **Hits by Type** shows a line for each of the hit types provided. The pie chart displays a regional breakdown of the bandwidth for a CDN mapping on a percentage basis.

Metrics shown for aggregate values include **Bandwidth Usage** in GB, **Total Hits** to the CDN Edge server, and the **Hit Ratio**. Hit Ratio indicates the percentage of times content is delivered by the Edge server, not through its Origin. Hit ratio currently is shown as a function of all your CDN mappings, not just the one being viewed.

By default, both the aggregate numbers and the graphs default to show metrics for the last 30 days, but you have the ability to change this through the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/). Both categories are capable of displaying metrics for 7-, 15-, 30-, 60-, or 90-day periods.

### HTTPS support with wildcard certificate

The wildcard certificate is the most economical way to deliver web content to your end-users securely. This feature is enabled by selecting `HTTPS port` when configuring your CDN or Origin Path. After the CDN mapping is enabled for HTTPS, using the wildcard certificate, the edge server contacts the Origin server through HTTPS. If the Origin server is specified as a hostname, the Edge server uses the Origin hostname as the Server Name Indication (SNI) header for the Transport Layer Security (TLS) negotiation with the Origin server by default. However, if the Origin server is specified as an IP address, the CDN's hostname will be used as the SNI header. The origin certificate must be signed by a recognized Certificate Authority (CA).

When using the wildcard certificate, an end-user **only** has access to the service using the CNAME. For example, if the domain is `example.com` and the CNAME is `example.cdnedge.bluemix.net`, your CDN service is **only** accessible through `https://example.cdnedge.bluemix.net`. Please refer to the [FAQ](faqs.html#what-should-be-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-cdn-protocols-) to understand the implications of using HTTPS with Wildcard Certificate.

As an industry best practice, Akamai only trusts the root certificates and not the intermediate certificates, because the set of intermediary certificates that is trusted changes frequently. For security purposes, Akamai only includes root Certificate Authorities in the Akamai trust store. Therefore, the Origin must send the entire certificate chain including the leaf certificate and all the intermediate certificates (not including the Root Certificate) as part of SSL handshake with the Edge Server. You can find the Akamai trusted certificates [here](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

### Host Header support

The Edge server uses the Host Header when communicating with the Origin host. This feature provides flexibility in how the web service is configured on the Origin host. If Host Header input is not provided, the service uses the Origin Server Hostname as default HTTP Host Header if the Origin Server is specified as Hostname (rather than as an IP address). If Host header is not provided as input AND the Origin Server is provided as an IP address, the CDN Hostname (also called the CDN Domain Name) is used as the default HTTP Host Header.

### Respect Headers

The Respect Headers option allows the HTTP Header configuration in the Origin to override the CDN configuration. This feature is ON by default, but can be turned OFF.

### Serve Stale content

When the CDN Edge server receives a user request, and the requested content is not cached, the Edge server reaches out to the Origin host to fetch the content. That content is then cached for the Time To Live (TTL) duration specified for the content. If a user request is received after the TTL has expired, the Edge server reaches out to the Origin host to fetch the content. If the Origin server cannot be reached for some reason (for instance, the Origin host is down, or there is a network issue), the Edge server serves the expired (stale) content to the request. This feature is supported by Akamai and **cannot** be turned off.

### "Ignore Query Args in Cache Key" feature

Akamai Edge servers cache content on a so-called "Cache Store." To use the content from the "Cache Store," Edge servers use a "Cache Key." Typically, a Cache Key is generated based on a portion of an end-user's URL. In some cases, the URL contains query function arguments that are different for individual users, but the content delivered is the same. By default, Akamai uses the query function's arguments to generate the Cache Key, and therefore, to generate a unique Cache Key for each user. This method is not optimal, because it causes the Edge server to contact the Origin server for content that is already cached, but using a different Cache Key. The **Ignore Query Args in Cache Key** feature allows you to specify whether the query args should be ignored when generating a Cache Key. This feature applies to any `create` or `update` of a CDN Mapping configuration, as well as for any `create` or `update` of an Origin path.

**Note:** Cache Key Query Args can be configured from the Settings tab after creation of a CDN mapping. For Origin path, they can be configured during `create` or `update` of an Origin path.

### Content Compression

Content Compression is enabled in Akamai CDN by default for the following content types:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

When compression is handled by the Edge Server, then the content must be at least 10kB.  In some cases, compression is taken care of by the Origin Server, and in those cases, there is no limit on the size of the files to be compressed. If the content is already being compressed by the Origin Server, it will not be compressed again. To enable Content Compression, the request header must define `Accept-Encoding: gzip`.

### Large File Optimization

Large file optimization allows the CDN network to optimize the delivery of content greater than 10MB. This enablement increases performance for large files and reduces latency and download times. Without this feature, the CDN cannot service files greater than 1.8GB in size. This feature allows file downloads greater than 1.8GB up to a maximum of 320GB.

For Large File Optimization to work, byte-range requests **must** be enabled on the origin server. Akamai CDN employs a technique called Partial Object Caching for this optimization. When a large file is requested, the edge server checks whether the file meets the size requirements. This means that files larger than 10MB will be requested from the origin server in chunks. Once the chunk arrives at the edge server, it is cached and immediately served to the user. The next chunk is pre-fetched in parallel by the edge server, thus reducing latency. This process continues until the entire file is retrieved, or the connection is terminated.  

When this feature is enabled, there is a slight performance cost associated with serving content smaller than 10MB. Therefore, this feature is recommended only for serving large files. A typical use case would be to create a new Origin Path in the CDN configuration and enable Large File Optimization for that path.

### Video on Demand

**Video on Demand** performance optimization delivers high-quality streaming across a variety of network types. By leveraging the distributed network's ability to distribute the load dynamically, IBM Cloud CDN with Akamai gives you the ability to scale rapidly for large audiences, whether you've planned for them or not.

**Video on Demand** is optimized for distribution of segmented streaming formats such as HLS, DASH, HDS, and HSS. Live video streaming is **not** supported at this time. You can enable the **Video on Demand** feature by selecting the option from the drop-down menu under **Optimize for** on the Settings tab, or while creating a new Origin Path. You should enable this feature only when optimizing delivery of video files.
