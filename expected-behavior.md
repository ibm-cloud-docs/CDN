---

copyright:
  years: 2018
lastupdated: "2018-06-08"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Expected behaviors

This table shows the behavior expected when loading either the **hostname** or **CNAME** from your web browser for the supported protocols.

<html>
	<table>
	<tr>
		<th rowspan=2>Browser URL</th>
		<th rowspan=2>CDN with HTTP protocol only</th>
		<th colspan=2>CDN with HTTPS protocol only</th>
		<th colspan=2>CDN with both HTTP and HTTPS protocols</th>
	</tr>
	<tr>
		<th> Wildcard </th>
		<th> Shared SAN </th>
		<th> Wildcard </th>
		<th> Shared SAN </th>
	</tr>
	<tr>
		<td> http://hostname </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
	</tr>
  <tr>
    <td> https://hostname</td>
		<td> Access denied </td>
		<td> Redirects to IBM Cloud Webpage </td>
		<td> Successful load </td>
		<td> Redirects to IBM Cloud Webpage </td>
		<td> Successful load </td>
	</tr>
	<tr>
		<td> http://cname </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
	</tr>
	<tr>
		<td> https://cname </td>
		<td> Redirects to IBM Cloud Webpage </td>
		<td> Successful load </td>
		<td> 301 Moved permanently </td>
		<td> Successful load </td>
		<td> Redirects to IBM Cloud Webpage </td>
	</tr>
	</table>
</html>

**Notes:**

A `301 Moved permanently` message most likely indicates you are attempting to reach a CDN with an `HTTPS` or `HTTP_AND_HTTPS` protocol using the hostname. Due to a limitation with the HTTPS wildcard certificate, you **must** use the CNAME for access to your CDN.

With an HTTP **only** protocol, you will receive the `301 Moved permanently` message if you try to reach your CDN using the CNAME. In this case, you can _only_ gain access to your CDN using the hostname.

The `Access denied` message is seen when you're trying to accreachess a CDN using an incorrect protocol. Ensure that you're using `http` for CDNs created with HTTP protocol, or `https` for CDNs created with HTTPS protocol.

The behavior of a URL redirecting to IBM Cloud CDN webpage is seen most often when the URL is incorrect for the protocol. If your CDN is created with a protocol of HTTPS or HTTPS_AND_HTTPS, you must use the CNAME for access to your CDN. For example: `https://examplecname.cdnedge.bluemix.net` for HTTPS mappings or `http://examplecname.cdnedge.bluemix.net` or `https://examplecname.cdnedge.bluemix.net` for HTTP_AND_HTTPS mappings.

The URL redirects to IBM Cloud CDN webpage in this case because both the protocol and domain are incorrect for the CDN's protocol. For a CDN created with HTTP as the _only_ protocol, it can be reached _only_ by means of the hostname. For example, `http://example.com`.
