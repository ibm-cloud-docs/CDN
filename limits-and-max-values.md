---

copyright:
  years: 2018, 2020
lastupdated: "2020-04-21"

keywords: time to live, entries, large file, size, optimization, downloads, years

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

Have a question about CDN and limits or maximum? Review these frequently asked questions, which provide answers to common inquiries.
{: shortdesc}

## Is there a maximum value for Time To Live? A minimum?
{: #is-there-a-maximum-value-for-time-to-live}

The maximum value for Time To Live is 2,147,483,647 seconds, which equates to roughly 68 years! The minimum value is 0 seconds.

## Is there a limit on the number of origin and TTL entries?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Yes, the combined limit is 75 entries per CDN.

## What is the largest file size that can be delivered through Akamai CDN?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Attempts to retrieve or deliver files larger than 1.8 GB receive a `403 Access Forbidden` response for the default performance configuration. If Large File Optimization is enabled, file downloads up to 320 GB are possible.

## What is the maximum size for the origin response headers?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

The maximum response header size is 16 KB. In case certain origins try to set cookies that are too large for the client to return in subsequent requests, Akamai sets this limit in the Edge servers. If the response headers are larger than 16 KB, the request gets a `502 Bad Gateway` response.

## What is the maximum size for the client's request headers?
{: #what-is-the-maximum-size-for-the-client-request-headers}

The maximum request headers size is 32 KB. If the request headers are larger than 32 KB, the Akamai Edge server returns a `400 Bad Request` response.

## How long do configuration changes take before they become active on the Akamai side?
{: #how-long-do-configuration-changes-take-active-in-akamai}

You can expect changes to become active on Akamai in 10 minutes.

## What's the rate limit for a multiple file purge?
{: #purge-rate-limit}

For a multiple file purge, there is a limit on the number of files that can be purged at a given time. The limiting algorithm that is used for the sustained rate and burstiness follows the Token Bucket model, where there is one fixed-capacity bucket that is applied for all the purge paths in your account. The total number of tokens in the bucket represents the number of paths that can be purged in a burst.

In the following table, `X-RateLimit-Purge-Paths-Limit-Burst` represents the number of tokens that the bucket can hold. The initial token bucket is full. Tokens are constantly added to this bucket based on the sustained rate (`X-RateLimit-Purge-Paths-Limit-Per-Second`). However, if the bucket is full, no more tokens can be added.

When a multiple file purge request is made, the remaining number of tokens is checked against the number of file paths. The paths bucket must contain enough tokens to satisfy all of the paths in the request. If there are enough tokens in the purge bucket, the tokens are removed from the bucket and the request is accepted. If there are not enough tokens, tokens are not removed, and the request is denied.  

This [example](/docs/CDN?topic=CDN-code-examples-using-the-cdn-api#create-group-example) shows the rate limiting response headers returned by the purge group API.

The following table lists the default values for rate limit headers:

|  Rate Header   | Value  | Description |
|  ----  | ----  | ----  |
| `X-RateLimit-Purge-Paths-Limit-Per-Second`  | 20 | The token number added into the bucket per second. |
| `X-RateLimit-Purge-Paths-Limit-Burst` | 1000 | The maximum paths can be purged in a burst. Capability of the bucket. |
| `X-RateLimit-Purge-Paths-Remaining` | N/A | Number of paths remaining before the rate limit is exceeded. |
{: caption="Table 1. Default values for rate limit headers" caption-side="bottom"}

## Is there a limit on the number of purge group entries?
{: #is-there-a-limit-on-the-number-of-purge-group-entries}

The number of purge group entries doesn't have any restrictions.

## What is the maximum size for purge group name?
{: #what-is-the-maximum-size-for-purge-group-name}

The maximum purge group name size is 999 characters.

## I received an error similar to `"Could not allocate more resource to build new rule in Akamai: x behaviors are needed while only x can be used."` What should I do?
{: #error-max-resource-limit}
{: faq}

When you reach the maximum resource limit, the console shows this error message. To resolve this issue, you can:

* Delete unused resources (TTL, origin, and so on).
* Create a domain to store new resources.
