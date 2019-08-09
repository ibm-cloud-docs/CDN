---

copyright:
  years: 2018, 2019
lastupdated: "2019-08-09"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

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
{:DomainName: data-hd-keyref="DomainName"}

# Feature Descriptions
{: #feature-descriptions}

This page highlights many of the features included with {{site.data.keyword.cloud}} CDN powered by Akamai.

## Host Server Origin support
{: #host-server-origin-support}

IBM Cloud Content Delivery Network (CDN) can be configured to serve content from a Host Server Origin by providing the Origin hostname, protocol, port number, and optionally, the path from which to serve the content. The default path is `/*`. Protocol can be HTTP, HTTPS, or both. Only certain port numbers are supported by Akamai. Please see the [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for supported port numbers/ranges.

## Object Storage Origin support
{: #object-storage-file-support}

IBM Cloud CDN can be configured to serve content from an Object Storage endpoint by providing the endpoint, the Bucket name, protocol, and port. Optionally, a list of file extensions can be specified to only allow caching for files with those extensions. All objects in the bucket need to be set with anonymous read or public read access.

This tutorial on [How to set up IBM Cloud Object Storage with CDN](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) provides more detailed information.

## Support for Multiple Origins with distinct paths
{: #support-for-multiple-origins-with-distinct-paths}

In certain cases, you may want to deliver certain content from a different Origin server. For example, you may want certain photos or videos served from different Origin servers. IBM Cloud CDN provides the option to set up multiple Origin servers with multiple paths. This allows flexibility regarding how and where the data is stored. The path provided for the Origin server should be unique with respect to the CDN. The Origin server itself does not need to be unique.

## Path-based CDN mappings
{: #path-based-cdn-mappings}

Your IBM Cloud CDN service can be restricted to a particular directory path on the Origin server by providing the path when creating the CDN. An end-user is allowed access only to those contents in that directory path. For example if a CDN `www.example.com` is created with Path `/videos`, it is accessible **only** through `www.example.com/videos/*`.

## Purge cached content
{: #purge-cached-content}

IBM Cloud CDN provides the capability to conveniently and quickly remove, or "purge", the cached content from the Edge servers. At this time, the purge is allowed only for a file path. Currently, the Purge implementation with Akamai purges the file in about 5 seconds. The UI also provides the ability to view your Purge history, and to tag specific Purge paths as Favorites.

## Time to Live (TTL)
{: #time-to-live-ttl}

The Time To Live indicates the amount of time (in seconds) the Edge server will retain the cached content for that particular file or directory path. When a CDN is first created, a global Time To Live (TTL) is created for path `/\*` with a default time of 3600 seconds. The minimum value for TTL is 0 seconds, and the maximum value is 2147483647 seconds. For each entry, the TTL path should be unique for the CDN. If multiple paths match a given content, the most recently configured path match applies to that content. For example, consider two TTLs, `/example/file` created first with a time to live value of 3000 seconds and `/example/*` is created later, with a value of 4000 seconds. Although `/example/file` is more specific, `/example/*` was created most recently, so the TTL for `/example/file` will be 4000 seconds. Once created, TTL entries can be edited for path and/or time. They can be deleted as well.

## Metrics with graphical views
{: #metrics-with-graphical-views}

Metrics for an individual CDN are provided on the Overview tab of the Customer Portal for that CDN Mapping. Two types of metrics are calculated from the CDN's usage: those that show the metrics over a time period as a graph; and those that are shown as aggregate values.

For metrics that display the change over a period of time as a graph, you can see three line graphs and a pie chart. The three line graphs are: **Bandwidth**, **Hits by Mapping**, and **Hits by Type**. They display the activity on a daily basis over the course of your specified time-frame. The graphs for **Bandwidth** and **Hits by Mapping** are single-line graphs, whereas the breakdown of **Hits by Type** shows a line for each of the hit types provided. The pie chart displays a regional breakdown of the bandwidth for a CDN mapping on a percentage basis.

Metrics shown for aggregate values include **Bandwidth Usage** in GB, **Total Hits** to the CDN Edge server, and the **Hit Ratio**. Hit Ratio indicates the percentage of times content is delivered by the Edge server, _not_ through its Origin. Hit ratio currently is shown as a function of all your CDN mappings, not just the one being viewed.

By default, both the aggregate numbers and the graphs default to show metrics for the last 30 days, but you have the ability to change this through the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/). Both categories are capable of displaying metrics for 7-, 15-, 30-, 60-, or 90-day periods.

## Host Header support
{: #host-header-support}

The Edge server uses the **Host Header** when communicating with the Origin host. This feature provides flexibility in how the web service is configured on the Origin host. Specifically it enables a use case where a client has multiple web servers configured on the same Origin Host. If Host Header input is not provided, the service uses the Origin Server Hostname as default HTTP Host Header if the Origin Server is specified as Hostname (rather than as an IP address). If Host header is not provided as input AND the Origin Server is provided as an IP address, the CDN Hostname (also called the CDN Domain Name) is used as the default HTTP Host Header.

## HTTPS Protocol support
{: #https-protocol-support}

CDN can be configured to use HTTPS protocol to serve the content securely to the end users. This configuration requires that an SSL certificate must be set up as part of the CDN configuration. Two types of SSL certificate options are available for HTTPS: [Wildcard certificate](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) and [Domain validated (DV) Subject Alternative Name (SAN) certificate](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport). This type will be referred to also as a _SAN certificate_ in this documentation.

The type of SSL Certificate to use is an important consideration for HTTPS CDN. Wildcard certificate configuration setup is fast, but it has the downside that the CDN is accessible only by means of a CNAME. The SAN certificate process takes 4 to 8 hours to complete, but it provides the ability to use the CDN with the CDN Domain (that is, the Hostname). The SAN Certificate also requires an additional step of [**Domain Control Validation**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) during configuration. No cost is associated with using either of these certificates. Please refer to the [Troubleshooting document](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-) to understand the implication of selecting a given Certificate type.

The Origin Host also must have its own SSL certificate for the CDN hostname, and it must be signed by a recognized Certificate Authority (CA).

As an industry best practice, Akamai only trusts the root certificates and not the intermediate certificates, because the set of intermediary certificates that is trusted changes frequently. You can find the Akamai trusted certificates [on the Web at this location](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Respect Headers
{: #respect-headers}

The **Respect Headers** option allows the HTTP Header configuration in the Origin to override the CDN configuration. This feature is ON by default, but can be turned OFF.

## Serve Stale content
{: #serve-stale-content}

When the CDN Edge server receives a user request, and the requested content is not cached, the Edge server reaches out to the Origin host to fetch the content. That content is then cached for the Time To Live (TTL) duration specified for the content. If a user request is received after the TTL has expired, the Edge server reaches out to the Origin host to fetch the content. If the Origin server cannot be reached for some reason (for instance, the Origin host is down, or there is a network issue), the Edge server serves the expired (stale) content to the request. This feature is supported by Akamai and **cannot** be turned off.

## Cache Key Optimization
{: #cache-key-optimization}

Akamai Edge servers cache content on a **Cache Store**. To use the content from the **Cache Store**, Edge servers use a **Cache Key**. Typically, a **Cache Key** is generated based on a portion of an end-user's URL. In some cases, the URL contains query function arguments that are different for individual users, but the content delivered is the same. By default, Akamai uses the query function's arguments to generate the Cache Key, and therefore, to generate a unique Cache Key for each user. This method is not optimal, because it causes the Edge server to contact the Origin server for content that is already cached, but using a different Cache Key. The **Cache Key Optimization** feature allows you to specify which query args to include or ignore when generating a Cache Key. This feature applies to any `create` or `update` of a CDN Mapping configuration, as well as for any `create` or `update` of an Origin path.

The value of **Cache Key Optimization** can be configured from the **Settings** tab after creation of a CDN mapping. For Origin path, they can be configured during the `create` or `update` operations of an Origin path.
{: note}

## Content Compression
{: #content-compression}

**Content Compression** is enabled in Akamai CDN by default for the following content types:
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

## Large File Optimization
{: #large-file-optimization}

Large file optimization allows the CDN network to optimize the delivery of content greater than 10MB. This enablement increases performance for large files and reduces latency and download times. Without this feature, the CDN cannot service files greater than 1.8GB in size. This feature allows file downloads greater than 1.8GB, up to a maximum of 320GB.

For Large File Optimization to work, byte-range requests **must** be enabled on the Origin server. Akamai CDN employs a technique called _Partial Object Caching_ for this optimization. When a large file is requested, the edge server checks whether the file meets the size requirements. This means that files larger than 10MB will be requested from the Origin server in chunks. Once the chunk arrives at the edge server, it is cached and immediately served to the user. The next chunk is pre-fetched in parallel by the edge server, thus reducing latency. This process continues until the entire file is retrieved, or the connection is terminated.

When this feature is enabled, there is a slight performance cost associated with serving content smaller than 10MB. Therefore, this feature is recommended only for serving large files. A typical use case would be to create a new Origin Path in the CDN configuration and enable Large File Optimization for that path.

## Video on Demand
{: #video-on-demand}

**Video on Demand** performance optimization delivers high-quality streaming across a variety of network types. By leveraging pre-configured cache control settings and the distributed network's ability to distribute the load dynamically, IBM Cloud CDN with Akamai gives you the ability to scale rapidly for large audiences, whether you've planned for them or not.

**Video on Demand** is optimized for distribution of segmented streaming formats such as HLS, DASH, HDS, and HSS. Live video streaming is **not** supported at this time. You can enable the **Video on Demand** feature by selecting the option from the drop-down menu under **Optimize for** on the Settings tab, or while creating a new Origin Path. You should enable this feature only when optimizing delivery of video files.

## Geographical Access Control
{: #geographical-access-control}

Geographical Access Control is a rule-based behavior that lets you set the `access-type` parameter for a group of users, based on their geographical location. Two types of behaviors are available: **Allow** and **Deny**.

The access-type `Allow` lets you specifically allow traffic to selected regions, based on the type of region. Allowing traffic for specific regions implicitly blocks traffic for all others. For example, you might choose to `Allow` traffic to selected continents, such as Europe and Oceania, which blocks access for all other continents.

On the other hand, the `Deny` behavior blocks access to your service for the specified group, but allows access for all other, non-specified, regions. For example, if you set the Geographical Access Control access-type to `Deny` for the continents of Europe and Oceania, users on those continents will **not** be able to use your service, whereas users on all other continents will have access to it.

This feature is accessible from the **Settings** page of your CDN Configuration.

## Hotlink Protection
{: #hotlink-protection}

Hotlink Protection is a rules-based behavior that lets you control whether certain websites are allowed access to your content from your CDN. The browser typically includes a `Referer` Header when an HTTP request is made from a link on a webpage, and when that link points to a remote asset. The link that one website uses for access to an asset from another website is called a _hotlink_.  Two types of behaviors are available: **ALLOW** and **DENY**.

If your `protectionType` is set to `ALLOW`:
* If the `Referer` header value in a request sent to your CDN matches one of your specified `refererValues`, your CDN **will** serve the content requested.
* Otherwise, your CDN will **not** serve the content.

If your `protectionType` is set to `DENY`:
* If the `Referer` header value in a request sent to your CDN matches one of your specified `refererValues`, your CDN **will not** serve the content requested.
* Otherwise, your CDN will serve the content.
