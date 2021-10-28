---

copyright:
  years: 2017, 2019
lastupdated: "2019-12-09"

keywords:

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Hotlink Protection class
{: #hotlink-protection-class}

The `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection class` contains the attributes that are used by our Hotlink Protection APIs. This object is used to set the Hotlink Protection behavior for a CDN by calling the API. It is also returned by Hotlink Protection APIs after a successful API call.
{: shortdesc}

**class** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`:

* `://` in the first label of the URL is not supported.

    * **Valid**: `http*www.example.com`
    * **Invalid**: `http://www.example.com`

* `protectionType`: specifies to allow or deny access to content when an HTTP request has a Referer Header value matching one of the terms in `refererValues`. The opposite occurs when there is no match.

    Possible value for protectionType:
    * `ALLOW`
    * `DENY`
* `refererValues`: is a single-space-separated list of referer URLs, which might also have wildcard matching, in the form of a string.

    Some examples of a **valid** string for `refererValues`:
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`

    Some examples of an **invalid** string for `refererValues`:

    | Description of invalid example | Example |
    |---------|-----|
    |Contains more than 2100 characters, total| `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
    |Contains at least one URL match term that has greater than 255 characters | `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
    |Contains duplicates in the list | `domain1.example.com domain1.example.com`|
    |Using characters not specified in [RFC-3986](https://tools.ietf.org/html/rfc3986#section-2){: external} | `domain1.exa}mple.com domain1.example.com`|
    |A `refererValues` string with at least one URL match term that has a character set in front of the first `.` character that contains `://` is not supported| `www.example.org http://www.example.com`|
    {: caption="Table 1. Examples of an invalid string for `refererValues`" caption-side="bottom"}
