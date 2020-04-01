---

copyright:
  years: 2018, 2020
lastupdated: "2020-02-07"

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

# FAQs for limits and maximum values
{: #limits-and-maximum-values}

## Is there a maximum value for Time To Live? A minimum?
{: #is-there-a-maximum-value-for-time-to-live}

The maximum value for Time To Live is 2,147,483,647 seconds, which equates to roughly 68 years! The minimum value is 0 seconds.

## Is there a limit on the number of Origin and TTL entries?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Yes, the combined limit is 75 entries per CDN.

## What is the largest file size that can be delivered through Akamai CDN?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Attempts to retrieve or deliver files larger than 1.8 GB receive a `403 Access Forbidden` response for the default performance configuration. If Large File Optimization is enabled, file downloads up to 320 GB are possible.

## What is the maximum size for the origin response headers?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

The maximum response header size is 16 KB. In case certain origins try to set cookies that are too large for the client to return in subsequent requests, Akamai sets this limit in the edge servers. If the response headers are larger than 16 KB, the request gets a `502 Bad Gateway` response.

## What is the maximum size for the client's request headers?
{: #what-is-the-maximum-size-for-the-client-request-headers}

The maximum request headers size is 32 KB. If the request headers are larger than 32 KB, the Akamai edge server returns a `400 Bad Request` response.

## How long do configuration changes take before they become active on the Akamai side?
{: #how-long-do-configuration-changes-take-active-in-akamai}

You can expect changes to become active on Akamai in 10 minutes.

## What's the rate limit for the multiple file purge?
{: #purge-rate-limit}

For the multiple file purge, there is a limit on the file number in the purge actions. The limiting algorithm that is used for sustained rate and burst follows a Token Bucket model.  
There is one bucket that is applied for all the purge paths under your account. The total token number in the bucket represents burst file numbers can be purged. In the following table, the X-RateLimit-Purge-Paths-Limit-Burst represents how many tokens the bucket can hold. The bucket starts with a full of tokens. Tokens are constantly added to the bucket based on the sustained rate (X-RateLimit-Purge-Paths-Limit-Per-Second). If the bucket is full, no more tokens can be added.  
When a multiple file purge request is made, the remaining tokens number is checked against the number of file paths. The paths bucket must contain enough tokens to satisfy all of the paths in the request. If there are enough tokens, then tokens are removed from the bucket and the request is accepted. If there are not enough tokens in the purge bucket, no tokens are removed, and the request is denied.  

This [example](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#create-group-example) shows that the rate limiting response headers returned by the purge group API.

Here is default value for rate limit headers:

|  Rate Header   | Value  | Description |
|  ----  | ----  | ----  |
| X-RateLimit-Purge-Paths-Limit-Per-Second  | 20 | The token number added into bucket per second. |
| X-RateLimit-Purge-Paths-Limit-Burst | 1000 | The maximum paths can be purged in a burst. Capability of the bucket. |
| X-RateLimit-Purge-Paths-Remaining| N/A | Number of paths remaining before the rate limit is exceeded. |
