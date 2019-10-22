---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-30"

keywords: limitations, Akamai, multiple files, purge, hotlink, limit

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Known Limitations
{: #known-limitations}

The following limitations apply to the CDN service for the Akamai Vendor:
* Purge of directory-level content or multiple files currently is not supported.
* There's a limit of 75 as the total number of Origins per CDN.
* HTTPS with DV SAN certificate is available only for new CDNs.
* Hotlink Protection does not support `refererValues` URL match terms in which the character set in front of the first `.` character contains the grouping of characters `://`, for example `http://www.example.com` or `www.example.com http://www.example.com`
* HTTPS using Wildcard certificates is not supported at this time.

STOP CDN functionality is intended for maintenance windows not exceeding 7 days. After 7 days, the CDN must be started, or it will be disabled, and traffic to the CDN CNAME will not be redirected to the origin server.
{: important}
