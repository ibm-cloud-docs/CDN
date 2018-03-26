---

copyright:
  years: 2017
lastupdated: "2017-09-10"

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
* HTTPS is currently supported only through wildcard certificate.
* Purge of directory level content or multiple files is currently not supported.
* Limit of 10 CDNs currently allowed for a given {{site.data.keyword.BluSoftlayer_notm}} Account.
* Limit on total number of Origins and TTL entries is 75 per CDN.
* Serve Stale Content Option functionality will always be **On**, even if the CDN is created with it as **Off** 
* If the CDN was created with **Server** and **HTTP Port**, the Origin can only be added with **Server** option.

The following limitations apply specifically to the Origin Server's SSL Certificate for HTTPS connections from Akamai <-> Origin:
* Certificate **MUST** match the *Host* header configured on the CDN,
* Certificate must not be self-signed,
* Certificate must not be expired, and
* Certificate must be issued by a Certification Authority that Akamai trusts
