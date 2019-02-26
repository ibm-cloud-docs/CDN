---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limits and Maximum Values
{: #limits-and-maximum-values}

## Is there a maximum value for Time To Live? A minimum?

The maximum value for Time To Live is 2,147,483,647 seconds, which equates to roughly 68 years! The minimum value is 0 seconds.

## Is there a limit on the number of Origin and TTL entries?

Yes, the combined limit is 75 entries per CDN.

## What is the largest file size that can be delivered through Akamai CDN?

Attempts to retrieve or deliver files larger than 1.8GB will receive a `403 Access Forbidden` response for the default performance configuration. If Large File Optimization is enabled, file downloads up to 320GB are possible.
