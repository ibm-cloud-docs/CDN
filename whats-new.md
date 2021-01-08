---

copyright:
  years: 2018, 2021
lastupdated: "2021-01-06"

keywords:

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Release notes
{: #what-s-new}

{{site.data.keyword.cloud}} launched the CDN service with Akamai in September 2017, but we haven't stopped there. We continue to make enhancements and add new features to the service, in order to better serve you and your end-users. Check back periodically to see what's new.
{:shortdesc}

## January 2021
{: #january-2021}

  * New API are available for the [Modify Response Header](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#modify-response-header), which can modify the outgoing response headers that are sent from the Edge server back to the client.
  * Changed the default [respect header](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#respect-headers) value from `ON` to `OFF`.
  * Added a [feedback widget](/docs/overview?topic=overview-feedback) for the CDN service.

## September 2020
{: #september-2020}

  * Performance optimization [Akamai CNAME](/docs/CDN?topic=CDN-getting-to-running-status#akamai-cname) support
  * Added a DNS based [challenge domain](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#challenge-domain) validation method for SAN HTTPS.

## August 2020
{: #august-2020}

  * Token authentication support [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#token-authentication) (available through API).

## July 2020
{: #july-2020}

  * Integrated CDN mapping metrics API support [API reference](/docs/CDN?topic=CDN-cdn-api-reference#getmappingintegratedmetrics).

## March 2020
{: #march-2020}

  * Multiple files purge [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#purge-cached-content) (available through UI and API).
  * Real-time metrics API [feature description](/docs/CDN?topic=CDN-metrics) (available through UI and API).
  * Activity Tracker Events for Audit log [feature description](/docs/CDN?topic=CDN-at_events) (available through UI).

## October 2019
{: #october-2019}

  * Dynamic Content Acceleration [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#dynamic-content-acceleration-description) (available through UI and API).
  * [Metrics reports](/docs/CDN?topic=CDN-metrics) for overall CDN mappings (available through UI and API).

## August 2019
{: #august-2019}

  * Move to the new IBM CNAME suffix `.cdn.appdomain.cloud.` from `.cdnedge.bluemix.net.` to unify user experience (Existing CDNs that are mapped to `.cdnedge.bluemix.net.` are not impacted).

## March 2019
{: #march-2019}

  * Hotlink Protection [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#hotlink-protection) (available through UI).

## November 2018
{: #november-2018}

  * Hotlink Protection [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#hotlink-protection) (available through the API).

## October 2018
{: #december-2018}

  * Increased the [maximum number of supported origins paths](/docs/CDN?topic=CDN-known-limitations#known-limitations) from 25 to 75.
  * [Origin paths](/docs/CDN?topic=CDN-adding-origin-path-details) no longer need to start with Mapping Path Prefix.
  * The minimum possible [TTL](/docs/CDN?topic=CDN-setting-content-caching-time-using-time-to-live) time value is now 0. This would preserve the cookies from the origin for content under the associated TTL path.

## September 2018
{: #september-2018}

  * We now have POPs in these newly added locations: Bahrain, Belgium, Colombia, Ecuador, Finland, Greece, Hong Kong S.A.R. of the PRC, Indonesia, Macau, Norway, Oman, Romania, South Korea, Sri Lanka, United Arab Emirates, and Uruguay. Look at the complete list of countries [here](/docs/CDN?topic=CDN-list-of-edge-servers#list-of-edge-servers).
  * Ability to specify TTL of 0 seconds for a given path.

## August 2018
{: #august-2018}

  * Geographical Access Control [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#geographical-access-control) (available through UI).
  * Vendor Selection step is not needed anymore for [Ordering a CDN](/docs/CDN?topic=CDN-order-a-cdn).

## July 2018
{: #july-2018}

  * Geographical Access Control [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#geographical-access-control) (available through the API).

## June 2018
{: #june-2018}

  * HTTPS with DV SAN support [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#https-protocol-support).
  * IBM Content Delivery Network adheres to the EU General Data Protection Regulation (GDPR) and compliance requirements. Our privacy practices are detailed in the [IBM Privacy Statement](https://www.ibm.com/privacy/us/en/).

## March 2018
{: #march-2018}

  * Large File Optimization [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#large-file-optimization).
  * Cache Key Optimization [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#cache-key-optimization).
  * Video On Demand Optimization [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#video-on-demand).
