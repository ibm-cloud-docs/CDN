---

copyright:
  years: 2020
lastupdated: "2020-03-29"

keywords:

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Stopping a CDN
{: #stopping-a-cdn}

After a mapping is stopped, the DNS lookup is switched to the origin. Traffic skips the CDN Edge servers and content is fetched directly from the origin. After a mapping is stopped, there might be a brief period of time where your content is not accessible. This is because the DNS cache might still be directing traffic to the Akamai Edge servers. However, during this time, the Akamai Edge server denies traffic for the domain. How long this period lasts depends on how often the DNS cache is refreshed, and varies depending on your DNS provider.
{: shortdesc}

**Important**:

* You can only stop a CDN when in `Running` state.
* Stopping a CDN is **NOT** recommended for a CDN configured with an HTTPS SAN certificate because HTTPS traffic might not work when you move the CDN back to `Running` status.
* Stop CDN functionality is intended for maintenance windows not exceeding seven days. After seven days, the CDN must be started, or it is disabled and traffic to the CDN CNAME is notredirected to the origin server.
* Currently, stopping a Wildcard CDN with the CNAME suffix `.cdnedge.bluemix.net` is **NOT** allowed.


To stop a CDN, follow these steps:

1. Click **Stop CDN** from the Overflow ![Overflow menu](images/overflow.png) menu.
2. A window appears, asking you to confirm that you want to stop the service. Select **Confirm** to proceed.

    After 5 to 15 seconds, the status changes to `Stopped`.
