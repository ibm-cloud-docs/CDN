---

copyright:
  years: 2018, 2019
lastupdated: "2019-08-06"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Troubleshooting
{: #troubleshooting}

In this document, you will find various ways to troubleshoot your {{site.data.keyword.cloud}} CDN. If you need to contact support, be sure to provide your CDN's reference number.

## How do I know my CDN is working?
{: #how-do-I-know-my-cdn-is-working}

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

The most common reason we have seen for the 503 error is due to an issue with a certificate in the SSL certificate chain.

This is the error you may see: `503 Service Unavailable`.  

In conjunction with the 503 error, you may also see a message similar to the following: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (the actual reference number may vary). In this case, the reference number in the error string translates to a SSL handshake failure.

To correct the issue, ensure that your Origin server's SSL certificate(s) meets the following criteria:
  * The certificate **must** be issued by a Certification Authority trusted by Akamai. You can view the list of Akamai trusted certificates at [this link ![External link icon](../../icons/launch-glyph.svg "External link icon")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)
  * It **must** match the *Host header* configured on the CDN
  * It must **not** be self-signed
  * It must **not** be expired

If you have verified your Origin's certificate chain using the previous criteria and you are still encountering the same error, please see our [Getting help and support](/docs/infrastructure/CDN?topic=CDN-gettinghelp) page. Make note of the Reference error string and include it in any communication with us.

## My hostname doesn't load on the browser when IBM Cloud Object Storage (COS) is the origin.
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

When your I{{site.data.keyword.cloud_notm}} CDN is configured to use COS as the object storage, direct access to the website will not work. You must specify the complete request path in the browser's address bar (for example, `www.example.com/index.html`). This behavior is caused by the index document limitation in IBM COS.

## I can't connect through a `curl` command or browser using the Hostname with HTTPS.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

If your CDN was created using HTTPS with a Wildcard certificate, the connection must be made using the CNAME; for example, `https://www.exampleCname.cdn.appdomain.cloud`. This includes **all** CDNs created with HTTPS prior to 18 June 2018. Trying to connect using the Hostname will result in an error.

## What is the expected behavior when loading the CNAME or hostname on your browser for the supported protocols?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

This table shows the behavior expected for the supported protocols when loading either the **hostname** or **CNAME** from your web browser.

<table>
<caption caption-side=“top”>Table of Expected Behaviors</caption>
<thead>
<tr>
<th rowspan=2 scope="col">Browser URL</th>
<th rowspan=2 scope="col">CDN with HTTP protocol only</th>
<th colspan=2 scope="col">CDN with HTTPS protocol only</th>
<th colspan=2 scope="col">CDN with both HTTP and HTTPS protocols</th>
</tr>
<tr>
<th scope="col"> Wildcard </th>
<th scope="col"> Shared SAN </th>
<th scope="col"> Wildcard </th>
<th scope="col"> Shared SAN </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> Successful load </td>
<td> 301 Moved permanently </td>
<td> Access denied<sup>&#42;</sup> </td>
<td> 301 Moved permanently </td>
<td> Successful load </td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> Access denied </td>
<td> Redirects to IBM Cloud Webpage </td>
<td> Successful load </td>
<td> Redirects to IBM Cloud Webpage </td>
<td> Successful load </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 Moved permanently </td>
		<td> Access denied<sup>&#42;</sup> </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> Redirects to IBM Cloud Webpage </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
		<td> Redirects to IBM Cloud Webpage </td>
</tr>
</tbody>
</table>

<small>\* The expected behavior was changed to `Access denied` for the domain mappings created since 08/05/2019. The expected behavior is keeping `Successful load` for the domain mappings created before 08/05/2019.</small>

**Common error messages:**

A `301 Moved permanently` message most likely indicates you are attempting to reach a CDN with an `HTTPS` or `HTTP_AND_HTTPS` protocol using the hostname. Due to a limitation with the HTTPS wildcard certificate, you **must** use the CNAME for access to your CDN.

With an HTTP **only** protocol, you will receive the `301 Moved permanently` message if you try to reach your CDN using the CNAME. In this case, you can _only_ gain access to your CDN using the hostname.

The `Access denied` message is seen when you're trying to reach a CDN using an incorrect protocol. Ensure that you're using `http` for CDNs created with HTTP protocol, or `https` for CDNs created with HTTPS protocol.

**A possible redirect error:**

The behavior of a URL redirecting to {{site.data.keyword.cloud_notm}} CDN webpage is seen most often when the URL is incorrect for the protocol. If your CDN is created with a protocol of HTTPS or HTTPS_AND_HTTPS, you must use the CNAME for access to your CDN. For example: `https://examplecname.cdn.appdomain.cloud` for HTTPS mappings or `http://examplecname.cdn.appdomain.cloud` or `https://examplecname.cdn.appdomain.cloud` for HTTP_AND_HTTPS mappings.

The URL redirects to {{site.data.keyword.cloud_notm}} CDN webpage in this case because both the protocol and domain are incorrect for the CDN's protocol. For a CDN created with HTTP as the _only_ protocol, it can be reached _only_ by means of the hostname. For example, `http://example.com`.
