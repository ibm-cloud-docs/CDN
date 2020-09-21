---

copyright:
  years: 2018, 2019
lastupdated: "2020-01-08"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

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
{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Troubleshooting
{: #troubleshooting}

In this document, find various ways to troubleshoot your {{site.data.keyword.cloud}} CDN. If you must contact support, be sure to provide your CDN's reference number.
{:shortdesc}

## How do I know that my CDN is working?
{: #how-do-I-know-my-cdn-is-working}
{: troubleshoot}
{: support}

Run the following `curl` command by replacing `http://your.cdn.domain/uri` with the respective file path on your CDN:

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

If the output of the `curl` command is similar to the following example format, the CDN is working as expected:

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```
{: screen}

## I received a 503 error. Why?
{: #i-received-a-503-error-why}
{: troubleshoot}
{: support}

The most common reason observed for the 503 error is due to an issue with a certificate in the SSL certificate chain.
{:tsCauses}

This is the error that you might see: `503 Service Unavailable`.
{:tsSymptoms}

With the 503 error, you might also see a message similar to the following: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (the actual reference number might vary). In this case, the reference number in the error string translates to an SSL handshake failure.

To correct the issue, ensure that your origin server's SSL certificates meets the following criteria:
  * The certificate must be issued by a certificate authority trusted by Akamai. You can view the list of Akamai trusted certificates at [this link](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates){:external}.
  * It must have a complete certificate chain (leaf, intermediate, root).
  * It must match the *Host header that is configured* on the CDN
  * It must not be self-signed
  * It must not be expired
  {:tsResolve}

If you have verified your origin's certificate chain using the previous criteria and you are still encountering the same error, please see our [Getting help and support](/docs/CDN?topic=CDN-gettinghelp) page. Make note of the Reference error string and include it in any communication with us.

## My hostname doesn't load on the browser when IBM Cloud Object Storage (COS) is the origin.
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}
{: troubleshoot}
{: support}

When your {{site.data.keyword.cloud_notm}} CDN is configured to use Cloud Object Storage (COS) as the object storage, direct access to the website will not work.
{:tsSymptoms}

You must specify the complete request path in the browser's address bar (for example, `www.example.com/index.html`).
{:tsResolve}

This behavior is caused by the index document limitation in IBM COS.
{:tsCauses}

## I can't connect through a `curl` command or browser using the hostname with HTTPS.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}
{: troubleshoot}
{: support}

If your CDN was created using HTTPS with a Wildcard certificate, the connection must be made using the CNAME; for example, `https://www.exampleCname.cdn.appdomain.cloud`. This includes **all** CDNs created with HTTPS before 18 June 2018. Trying to connect using the hostname results in an error.

## What is the expected behavior when loading the CNAME or hostname on your browser for the supported protocols?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}
{: troubleshoot}
{: support}

This table shows the behavior that is expected for the supported protocols when loading either the **hostname** or **CNAME** from your web browser.

| Browser URL | CDN with HTTP protocol only |
|:-----|:-----|
| `http://hostname` | Successful load |
| `https://hostname` | Access denied<sup>1</sup> |
| `http://cname` | 301 Moved Permanently |
| `https://cname` | Redirects to IBM Cloud webpage |
{: caption="Table 1. Expected behavior for CDN with HTTP protocol only" caption-side="left"}
{: #simpletabtable1}
{: tab-title="HTTP only"}
{: tab-group="cdr-troubleshooting"}
{: class="simple-tab-table"}

| Browser URL | Wildcard | Shared SAN |
|-----|-----|-----|  
| `http://hostname` | 301 Moved Permanently | Access denied<sup>1</sup> |
| `https://hostname` | Redirects to IBM Cloud webpage | Successful load |
| `http://cname` | Access denied<sup>1</sup> | 301 Moved Permanently |
| `https://cname` | Successful load | 301 Moved Permanently |
{: caption="Table 2. Expected behavior for CDN with HTTPS protocol only" caption-side="left"}
{: #simpletabtable2}
{: tab-title="HTTPS only"}
{: tab-group="cdr-troubleshooting"}
{: class="simple-tab-table"}

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

<sup>1</sup> The expected behavior was changed to `Access denied` for the domain mappings that are created since 08/05/2019. The expected behavior is keeping `Successful load` for the domain mappings created before 08/05/2019.

**Common error messages:**

A `301 Moved Permanently` message most likely indicates you are attempting to reach a CDN with an `HTTPS` or `HTTP_AND_HTTPS` protocol using the hostname. Due to a limitation with the HTTPS wildcard certificate, you must use the CNAME for access to your CDN.

With an HTTP **only** protocol, you receive the `301 Moved Permanently` message if you try to reach your CDN using the CNAME. In this case, you can _only_ gain access to your CDN using the hostname.

The `Access denied` message is seen when you're trying to reach a CDN using an incorrect protocol. Ensure that you're using `http` for CDNs created with HTTP protocol, or `https` for CDNs created with HTTPS protocol.

**A possible redirect error:**

The behavior of a URL redirecting to {{site.data.keyword.cloud_notm}} CDN webpage is seen most often when the URL is incorrect for the protocol. If your CDN is created with a protocol of HTTPS or HTTPS_AND_HTTPS, you must use the CNAME for access to your CDN. For example,: `https://examplecname.cdn.appdomain.cloud` for HTTPS mappings or `http://examplecname.cdn.appdomain.cloud` or `https://examplecname.cdn.appdomain.cloud` for HTTP_AND_HTTPS mappings.

The URL redirects to {{site.data.keyword.cloud_notm}} CDN webpage in this case because both the protocol and domain are incorrect for the CDN's protocol. For a CDN created with HTTP as the _only_ protocol, it can be reached _only_ by using the hostname. For example, `http://example.com`.
