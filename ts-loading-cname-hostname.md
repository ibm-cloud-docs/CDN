---

copyright:
  years: 2018, 2020
lastupdated: "2020-01-08"

keywords: troubleshooting

subcollection: CDN

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# What is the expected behavior when loading the CNAME or hostname on your browser for the supported protocols?
{: #troubleshoot-cdn-loading-cname-hostname}
{: troubleshoot}
{: support}

This table shows the behavior that is expected for the supported protocols when loading either the **hostname** or **CNAME** from your web browser.
{: shortdesc}

| Browser URL | CDN with HTTP protocol only |
|:-----|:-----|
| `http://hostname` | Successful load |
| `https://hostname` | Access denied [^A] |
| `http://cname` | 301 Moved Permanently |
| `https://cname` | Redirects to IBM Cloud webpage |
{: caption="Table 1. Expected behavior for CDN with HTTP protocol only" caption-side="left"}
{: #simpletabtable1}
{: tab-title="HTTP only"}
{: tab-group="cdr-troubleshooting"}
{: class="simple-tab-table"}

| Browser URL | Wildcard | Shared SAN |
|-----|-----|-----|  
| `http://hostname` | 301 Moved Permanently | Access denied[^B] |
| `https://hostname` | Redirects to IBM Cloud webpage | Successful load |
| `http://cname` | Access denied [^C] | 301 Moved Permanently |
| `https://cname` | Successful load | 301 Moved Permanently |
{: caption="Table 2. Expected behavior for CDN with HTTPS protocol only" caption-side="left"}
{: #simpletabtable2}
{: tab-title="HTTPS only"}
{: tab-group="cdr-troubleshooting"}
{: class="simple-tab-table"}

[^A] The expected behavior was changed to `Access denied` for the domain mappings that are created since 08/05/2019. The expected behavior is keeping `Successful load` for the domain mappings created before 08/05/2019.

[^B] The expected behavior was changed to `Access denied` for the domain mappings that are created since 08/05/2019. The expected behavior is keeping `Successful load` for the domain mappings created before 08/05/2019.

[^C] The expected behavior was changed to `Access denied` for the domain mappings that are created since 08/05/2019. The expected behavior is keeping `Successful load` for the domain mappings created before 08/05/2019.

| Browser URL | Wildcard | Shared SAN |
|-----|-----|-----|  
| `http://hostname` | 301 Moved Permanently | Successful load |
| `https://hostname` | Redirects to IBM Cloud webpage | Successful load |
| `http://cname` | Successful load | 301 Moved Permanently |
| `https://cname` | Successful load | Redirects to IBM Cloud webpage |
{: caption="Table 3. Expected behavior for CDN with both HTTP and HTTPS protocol" caption-side="left"}
{: #simpletabtable3}
{: tab-title="HTTP and HTTPS"}
{: tab-group="cdr-troubleshooting"}
{: class="simple-tab-table"}

**Common error messages:**

A `301 Moved Permanently` message most likely indicates you are attempting to reach a CDN with an `HTTPS` or `HTTP_AND_HTTPS` protocol using the hostname. Due to a limitation with the HTTPS wildcard certificate, you must use the CNAME for access to your CDN.

With an HTTP **only** protocol, you receive the `301 Moved Permanently` message if you try to reach your CDN using the CNAME. In this case, you can _only_ gain access to your CDN using the hostname.

The `Access denied` message is seen when you're trying to reach a CDN using an incorrect protocol. Ensure that you're using `http` for CDNs created with HTTP protocol, or `https` for CDNs created with HTTPS protocol.

**A possible redirect error:**

The behavior of a URL redirecting to {{site.data.keyword.cloud_notm}} CDN webpage is seen most often when the URL is incorrect for the protocol. If your CDN is created with a protocol of HTTPS or HTTPS_AND_HTTPS, you must use the CNAME for access to your CDN. For example,: `https://examplecname.cdn.appdomain.cloud` for HTTPS mappings or `http://examplecname.cdn.appdomain.cloud` or `https://examplecname.cdn.appdomain.cloud` for HTTP_AND_HTTPS mappings.

The URL redirects to {{site.data.keyword.cloud_notm}} CDN webpage in this case because both the protocol and domain are incorrect for the CDN's protocol. For a CDN created with HTTP as the _only_ protocol, it can be reached _only_ by using the hostname. For example, `http://example.com`.
