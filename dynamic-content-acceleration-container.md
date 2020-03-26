---

copyright:
  years: 2019
lastupdated: "2019-10-21"

keywords: input, container, class, API, mapping, origin, path, DCA

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Dynamic Content Acceleration container
{: #dynamic-content-acceleration-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Performance_DynamicContentAcceleration container` contains the attributes utilized by both the Mapping and (Origin) Path classes. This object is used to set the Dynamic Content Acceleration (DCA) behavior for a CDN by calling the API.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Performance_DynamicContentAcceleration`:

* `detectionPath`: A string value, will be used by CDN edge servers to find the best optimized route from edge to the origin server.
* `prefetchEnabled`: A boolean value that, if set to 'true', will inspects HTML responses and pre-fetches embedded objects in HTML files. The value is 'true' by default.
* `mobileImageCompressionEnabled`: A boolean value that, if set to 'true', will compress images to reduce the amount of content for mobile devices. The value is 'true' by default.
