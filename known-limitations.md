---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-09"

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
* Limit on total number of Origins and TTL entries is 75 per CDN.
* Serve Stale Content Option functionality will always be **On**.
* HTTPS certificate type cannot be changed once a mapping is created - for example, from Wildcard to DV SAN.
* HTTPS with DV SAN certificate is available only for new CDNs.
* A CDN created with a DV SAN certificate cannot be deleted unless it is in a RUNNING, CNAME_Configuration, CREATE_ERROR, or DELETE_ERROR state.
