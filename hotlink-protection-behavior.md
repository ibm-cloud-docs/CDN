---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Hotlink Protection class

The `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking class` contains the attributes utilized by our Hotlink Protection APIs. This object is used to set the Hotlink Protection behavior for a CDN by calling the API.  It is also returned by Hotlink Protection APIs after a successful API call.

class `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`:

* `protectionType`: specifies to allow or deny access to content when an HTTP request has a Referer Header value matching one the of the terms in `refererValues`. The opposite will occur when there is no match.
  * Possible value for protectionType:
    * `ALLOW`
    * `DENY`
* `refererValues`: is a single-space-separated list of referer URLs, which may also have wildcard matching, in the form of a string.
  * Some examples of a **valid** string for `refererValues`:
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * Some examples of an **invalid** string for `refererValues`:
    * `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...
      * Contains more than 2100 characters
    * `domain1.example.com domain1.example.com`
      * Contains duplicates in the list
    * ` `
      * Empty refererValues
    * `domain1.exa}mple.com domain1.example.com`
      * Using character(s) not specified in [RFC-3986](https://tools.ietf.org/html/rfc3986#section-2)
    * `www.example.com/path&`
      * `&` character is not supported
    * `www.example.org http://www.example.com`
      * A `refererValues` string with at least one URL match term that has a character set in front of the first `.` character that contains `://` is not supported
