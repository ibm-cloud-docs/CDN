---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  Edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Controlling an HTTP client's cache duration
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

When using a CDN, two levels of caching are available:

  * **Caching at the Edge** occurs when a CDN Edge server caches a piece of content from the origin.
  * **Caching downstream** from the Edge network of servers occurs when an user or HTTP client, such as a requesting browser, caches a piece of content from an Edge server.

The method that you choose to control how long content is cached at the requester, such as a browser, depends on the following factors:

  * Whether the Respect Header setting is ON or OFF. By default it is set to ON.
  * Whether the Origin server provides a `max-age` value in the Cache-Control header for a particular piece of content.

Regardless of how these factors change, your origin must provide a Cache-Control header for the intended content to the Edge, if you want Edge servers to send HTTP responses with the Cache-Control header for that content.

Essentially, Cache-Control headers sent from an Edge server downstream ask the requester to cache the associated content according to the caching directives or values that are specified by the Edge server.
{:shortdesc}

## Respect Header: Off
{: #respect-header-off}

If your origin does provide a Cache-Control header with a `max-age` directive and value for a specific piece of content, then the cache duration for the specific piece of content that is cached on the Edge is still derived from your CDN's TTL settings. Additionally, the Edge server responds to the requester downstream with a Cache-Control `max-age` value equal to the lesser of:
  * The origin's Cache-Control `max-age` value.
  * The remaining time left until the content goes stale on the Edge.

However, if your origin does not provide a Cache-Control header to the Edge server, then Edge servers will not provide a Cache-Control header to the requester. The Edge cache duration for your content is still derived from your CDN's TTL Settings.

## Respect Header: On
{: #respect-header-on}

If your origin does provide a Cache-Control header with `max-age` for a specific piece of content, the origin's Cache-Control `max-age` value becomes the cache duration for that specific piece of content cached on the Edge, overriding any applicable TTL settings for that piece of content. Additionally, the Edge responds to the requester with a Cache-Control `max-age` value equal to the remaining time left until the content goes stale on the Edge server.

However, if your origin does not provide a Cache-Control header to the Edge server, then the Edge server will not provide a Cache-Control header to the requester. The Edge cache duration for your content is still derived from your CDN's TTL settings.

## Summary
{: #summary}

|Respect Header|Origin Provides Cache-Control|Specific Content's Cache Duration on Edge Server|Edge Server Provides Cache-Control|
|---|---|---|---|
|On|Yes, origin specifies a `max-age`|Edge cache duration overridden with Origin's `max-age` value|Yes, Edge also provides a `max-age` with a value that is the (overridden) time before the Edge needs to refresh the content from the origin|
|On|Yes, but origin does not specify a `max-age`|Edge cache duration based on the CDN's TTL configuration|Yes, Edge also provides a `max-age` with a value that is the time before the Edge needs to refresh the content from the origin|
|On|No|Edge cache duration based on the CDN's TTL configuration|No|
|Off|Yes, origin specifies a `max-age`|Edge cache duration base on the CDN's TTL configuration|Yes, Edge also provides a `max-age` value that is the lesser of origin's `max-age` value and the time before the Edge needs to refresh the content from the origin|
|Off|Yes, but origin does not specify a `max-age`|Edge cache duration based on the CDN's TTL configuration|Yes, Edge also provides a `max-age` that is the time before the Edge needs to refresh the content from the origin|
|Off|No|Edge cache duration based on the CDN's TTL configuration|No|

## More information
{: #more-information-on-cache-control}

See cache-control as defined in section 14.9 of [RFC 2616 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ietf.org/rfc/rfc2616.txt).
