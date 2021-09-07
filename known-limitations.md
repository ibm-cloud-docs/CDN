---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-28"

keywords:  

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

# Known limitations
{: #known-limitations}

The following limitations apply to the CDN service for the Akamai vendor:
{: shortdesc}

* Purge contents of directory-level and domain-level is not supported.
* There's a limit of 75 as the total number of origins per CDN.
* HTTPS with DV SAN certificate is available only for new CDNs.
* Hotlink Protection does not support `refererValues` URL match terms in which the character set in front of the first `.` character contains the grouping of characters `://` (for example, `http://www.example.com` or `www.example.com http://www.example.com`.
* Currently HTTPS using Wildcard certificates is not supported.

Stop CDN functionality is intended for maintenance windows not exceeding seven days. After seven days, the CDN must be started, or it is disabled and traffic to the CDN CNAME is not redirected to the origin server.
{: important}
