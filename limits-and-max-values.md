---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

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
{: #is-there-a-maximum-value-for-time-to-live}

The maximum value for Time To Live is 2,147,483,647 seconds, which equates to roughly 68 years! The minimum value is 0 seconds.

## Is there a limit on the number of Origin and TTL entries?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Yes, the combined limit is 75 entries per CDN.

## What is the largest file size that can be delivered through Akamai CDN?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Attempts to retrieve or deliver files larger than 1.8GB will receive a `403 Access Forbidden` response for the default performance configuration. If Large File Optimization is enabled, file downloads up to 320GB are possible.

## What is the maximum size for the origin response headers?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

The maximum response header size is 16KB. In case certain origins try to set cookies that are too large for the client to return in subsequent requests, Akamai sets this limit in the edge servers. If the response headers are larger than 16KB, the request will get a `502 Bad Gateway` response.

## What is the maximum size for the client's request headers?
{: #what-is-the-maximum-size-for-the-client-request-headers}

The maximum request headers size is 32KB. If the request headers are larger than 32KB, the Akamai edge server will return a `400 Bad Request` response.
