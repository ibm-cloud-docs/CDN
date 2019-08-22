---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Using Cache Control `max-age` to control browser caching duration

When using a CDN, there is one type of caching for each location where caching can occur:
  * **Browser-level caching** occurs when the browser caches a piece of content for a specific amount of time.
  * **Edge-level caching** occurs when a CDN edge server caches a piece of content from the origin for a specific, but not necessarily the same, amount of time.

There are a few methods to control how long content is cached at the browser level. The method to use depends on the following factors:
  * Whether you have updated or will `[update your CDN’s configuration details](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details)` with “Respect Header: On”.
  * Whether the origin server provides a `max-age` value in the Cache-Control header for a given piece of content. 

Regardless of how these factors change, your origin needs to provide a non-empty Cache-Control header for your targeted content. If your origin does not provide a non-empty Cache-Control header for the edge server, the edge server will not provide its own Cache-Control header to the browser.

When the edge server sends a Cache-Control header to the browser, the `max-age` value for this Cache-Control header corresponds to the amount of time left before the content on the edge server goes stale and needs revalidation from the origin. 

## Respect Header: Off
When Respect Header is set to **Off**, you can control the browser-level cache time by configuring your CDN’s TTL settings. For each file or directory of files, you need to create a TTL rule for its path and desired cache duration.

This setup does two things:
  * The TTL rule tells the edge server to cache the content for the specified amount of time.
  * When the content is served from the edge server, the edge server sends its own Cache-Control header with a `max-age` value, based on the TTL duration, to the browser.

## Respect Header: On
When **Respect Header** is set to **On**, you can choose not to configure a TTL rule to control the browser-level caching time for your content. In that case, you need to configure your origin server to provide a Cache-Control header with a `max-age` value for a given piece of content. Because Respect Header is On, the edge server uses the Cache-Control's `max-age` value from the origin as the TTL duration for that specific content. If you already have a TTL rule entry for your CDN that covers the content, the edge server actually still uses the `max-age` value and overwrites any existing edge-level caching duration for that content.

This setup does two things:
  * The value of the Cache-Control `max-age` directive from the origin tells the edge server to cache the content for the specified amount of time.
  * When the content is served from the edge server, the edge server sends its own Cache-Control header to the browser with a `max-age` value based on the `max-age` value from the origin.

No other content covered by that TTL rule is affected if you're using the targeted method shown in the example.

When **Respect Header** is set to **On**, the caching duration on the browser can be controlled by a TTL rule, if the Origin server does not send the `max-age` directive in the Cache-Control header. If it does specify a `max-age`, then that value may accidentally override the time value on the TTL rule.

## Summary

|Respect Header|Origin Provides Cache-Control|Specific Content's Caching Duration on Edge Server|Edge Server Provides Cache-Control|
|---|---|---|---|
|On|Yes, specifies a `max-age`|Overwritten with Origin's `max-age` value|Yes, `max-age` is the time before the edge's content needs revalidation from the origin|
|On|Yes, does not specify a `max-age`|Based on the CDN's TTL configuration|Yes, `max-age` is the time before the edge's content needs revalidation from the origin|
|On|No|Based on the CDN's TTL configuration|No|
|Off|Yes, specifies a `max-age`|Based on the CDN's TTL configuration|Yes, `max-age` is the time before the edge's content needs revalidation from the origin|
|Off|Yes, does not specify a `max-age`|Based on the CDN's TTL configuration|Yes, `max-age` is the time before the edge's content needs revalidation from the origin|
|Off|No|Based on the CDN's TTL configuration|No|

## More Information
* How to `[manage your CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)`
* Cache-Control as defined in section 14.9 of [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
