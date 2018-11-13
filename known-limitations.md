---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Known Limitations

The following limitations apply to the new CDN service for the Akamai Vendor:
* Purge of directory level content or multiple files is currently not supported.
* Limit of 75 total number of Origins per CDN.
* HTTPS with DV SAN certificate is available only for new CDNs.
* Hotlink Protection does not support `refererValues` URL match terms where the character set in front of the first `.` character contain `://`, for example `http://www.example.com` or `www.example.com http://www.example.com`
