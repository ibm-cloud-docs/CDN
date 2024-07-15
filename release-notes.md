---

copyright:
  years: 2018, 2024
lastupdated: "2024-07-15"

keywords:

subcollection: CDN

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for CDN
{: #release-notes}

{{site.data.keyword.cloud}} launched the CDN service with Akamai in September 2017, but we haven't stopped there. We continue to make enhancements and add new features to the service, in order to better serve you and your end-users. Check back periodically to see what's new.
{: shortdesc}

## July 2021
{: #CDN-jul-0121}
{: release-note}

Advanced rule policy
:    Released the [advanced rule policy](/docs/CDN?topic=CDN-setting-advanced-rules) for geographic access control, hotlink protection, and token authentication.

## March 2021
{: #CDN-mar-0121}
{: release-note}

New UI portal
:    Released a new UI portal for the CDN service.

## January 2021
{: #CDN-jan-0121}
{: release-note}

New API for Modify Response Header
:    New API are available for the [Modify Response Header](/docs/CDN?topic=CDN-about-content-delivery-networks#modify-response-header), which can modify the outgoing response headersthat are sent from the Edge server back to the client.

Changed default respect header
:    Changed the default [respect header](/docs/CDN?topic=CDN-about-content-delivery-networks#respect-headers) value from `ON` to `OFF`.

Added feedback widget
:    Added a [feedback widget](/docs/overview?topic=overview-feedback) for the CDN service.

## September 2020
{: #CDN-sep-0120}
{: release-note}

Performance optimization
:    Performance optimization [Akamai CNAME](/docs/CDN?topic=CDN-next-steps-after-ordering#akamai-cname) support

DNS based challenge domain validation method added
:    Added a DNS based [challenge domain](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#challenge-domain) validation method for SAN HTTPS.

## August 2020
{: #CDN-aug-0120}
{: release-note}

Token authentication support added
:    Token authentication support [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#token-authentication) (available through API).

## July 2020
{: #CDN-ju-0120}
{: release-note}

Integrated CDN mapping metrics
:    Integrated CDN mapping metrics API support [API reference](/docs/CDN?topic=CDN-cdn-api-reference#api-get-mapping-integrated-metrics).

## March 2020
{: #CDN-mar-0120}
{: release-note}

Added multiple files purge
:    Multiple files purge [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#purge-cached-content) (available through UI and API).

Added real-time metrics
:    Real-time metrics API [feature description](/docs/CDN?topic=CDN-metrics) (available through UI and API).

Added activity tracker events for audit log
:    Activity Tracker Events for Audit log [feature description](/docs/CDN?topic=CDN-at_events) (available through UI).

## October 2019
{: #CDN-oct-0119}
{: release-note}

Dynamic content acceleration and metrics report added
:    Dynamic Content Acceleration [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#dynamic-content-acceleration-description) (available through UI and API).

Metrics reports for overall CDN mappings added
:    [Metrics reports](/docs/CDN?topic=CDN-metrics) for overall CDN mappings (available through UI and API).

## August 2019
{: #CDN-aug-0119}
{: release-note}

New IBM CNAME suffix move
:    Move to the new IBM CNAME suffix `.cdn.appdomain.cloud.` from `.cdnedge.bluemix.net.` to unify user experience (Existing CDNs that are mapped to `.cdnedge.bluemix.net.` are not impacted).

## March 2019
{: #CDN-mar-0119}
{: release-note}

Hotlink protection added for UI
:    Hotlink Protection [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#hotlink-protection) (available through UI).

## November 2018
{: #CDN-nov-0118}
{: release-note}

Hotlink protection added for API
:    Hotlink Protection [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#hotlink-protection) (available through the API).

## October 2018
{: #CDN-dec-0118}
{: release-note}

Max number of supported origins paths increased
:    Increased the [maximum number of supported origins paths](/docs/CDN?topic=CDN-limits-and-maximum-values#is-there-a-limit-on-the-number-of-origin-and-ttl-entries) from 25 to 75.

Origin paths prefix change
:    [Origin paths](/docs/CDN?topic=CDN-adding-origin-path-details) no longer need to start with Mapping Path Prefix.

Minimum TTL value updated
:    The minimum possible [TTL](/docs/CDN?topic=CDN-setting-content-caching-time-using-time-to-live) time value is now 0. This would preserve the cookies from the origin for content under the associated TTL path.

## September 2018
{: #CDN-sep-0118}
{: release-note}

New POP locations
:    We now have POPs in these newly added locations: Bahrain, Belgium, Colombia, Ecuador, Finland, Greece, Hong Kong S.A.R. of the PRC, Indonesia, Macau, Norway, Oman, Romania, South Korea, Sri Lanka, United Arab Emirates, and Uruguay. Look at the complete list of countries [here](/docs/CDN?topic=CDN-list-of-edge-servers#list-of-edge-servers).

Specify TTL
:    Ability to specify TTL of 0 seconds for a given path.

## August 2018
{: #CDN-aug-0118}
{: release-note}

Added Geographical access control to the UI
:    Geographical Access Control [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#geographical-access-control) (available through UI).

Vendor selection step removed from ordering
:    Vendor Selection step is not needed anymore for [Ordering a CDN](/docs/CDN?topic=CDN-order-a-cdn).

## July 2018
{: #CDN-jul-0118}
{: release-note}

Added Geographical access control to the API
:    Geographical Access Control [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#geographical-access-control) (available through the API).

## June 2018
{: #CDN-jun-0118}
{: release-note}

Added HTTPS with DV SAN support
:    HTTPS with DV SAN support [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#https-protocol-support).

GDPR compliance
:    IBM Content Delivery Network adheres to the EU General Data Protection Regulation (GDPR) and compliance requirements. Our privacy practices are detailed in the [IBM Privacy Statement](https://www.ibm.com/us-en/privacy).

## March 2018
{: #CDN-mar-0118}
{: release-note}

Large file optimization
:    Large File Optimization [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#large-file-optimization).

Cache key optimization
:    Cache Key Optimization [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#cache-key-optimization).

Video on demand optimization
:    Video On Demand Optimization [feature description](/docs/CDN?topic=CDN-about-content-delivery-networks#video-on-demand).
