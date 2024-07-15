---

copyright:
  years: 2019, 2024
lastupdated: "2024-07-15"

keywords:

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Dynamic Content Acceleration container
{: #dynamic-content-acceleration-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Performance_DynamicContentAcceleration container` contains the attributes that are used by both the Mapping and (Origin) Path classes. This object is used to set the Dynamic Content Acceleration (DCA) behavior for a CDN by calling the API.
{: shortdesc}

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Performance_DynamicContentAcceleration`:

* `detectionPath`: A string value is used by CDN Edge servers to find the best optimized route from Edge to the origin server.
* `prefetchEnabled`: A Boolean value that, if set to 'true', will inspects HTML responses and pre-fetches embedded objects in HTML files. The value is 'true' by default.
* `mobileImageCompressionEnabled`: A Boolean value that, if set to 'true', compresses images to reduce the amount of content for mobile devices. The value is 'true' by default.
