---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Metrics

## How do I view the metrics and usage?

You can view metrics and usage on the **Overview** page, which can be reached by selecting your CDN from the **Content Delivery Networks** page. **NOTE**: After you create your CDN, it may take up to 48 hours for metrics to appear.

## I created a CDN and there was data traffic through the CDN. Why don't my metrics show up?

After a CDN is created, it takes 48 hours for metrics to appear.


## Is there a minimum number of days for which I can view metrics? Is there a maximum?

There are a minimum and a maximum number of days for which you can view metrics using IBM Cloud Content Delivery Network with Akamai. Metrics can be gathered for a minimum of 7 days. Metrics can be viewed for a maximum of 90 days. For those using the API, it is recommended to use 89 days as the maximum, to account for any differences in time zones.

## Why is the hit ratio non-zero when total hits are zero?
Hit ratio represents the percentage of times the content was delivered from the Edge Server Cache, rather than being delivered from the Origin Server. It is calculated as follows:

> ((Edge hits - Ingress hits)/Edge hits) * 100

where:
Edge hits is defined as "All hits to the edge servers from the end-users"  
Ingress hits is defined as "Origin or Ingress hits are for traffic from your origin to Akamai edge servers"

Because Hit Ratio is calculated at the Account level and not per CDN, the Hit Ratio will be the same for all the CDNs in your account. This fact also explains why the Hit Ratio may be non-zero when the number of Edge hits for a particular CDN is zero.

