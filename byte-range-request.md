---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Working with byte-range requests
{: #working-with-byte-range-requests}

Using a byte-range request, you can retrieve partial content from an origin server. This document can help you understand the response status codes you might see.
{:shortdesc}

When a **Byte-range request** is sent by using {{site.data.keyword.cloud}} CDN with Akamai, the user might receive a `200 (OK)` response code for the first request, and a `206` response code for all subsequent requests, because Akamai Edge servers request content from the origin in compressed format. Therefore, when an Edge server doesn't have an object in its cache, nor does it have any information regarding the content length of the object, it forwards to the origin and requests the entire object. In return the origin serves the object without the content length header to Akamai, and the user is served the whole object even though it was a byte-range request. Thus the `200` Status code. On subsequent requests, the Edge server has the object in its cache and serves the `206` status code.

The **Range HTTP request** header indicates which part of the content the server should return. Several parts can be requested with one range header at one time, and the server can send back these ranges in a multipart response. If the server sends back ranges, it responds with a 206 (Partial Content) status.

One way to ensure a 206 response, even for the first byte-range request, is to disable `Transfer-Encoding: chunked` on your origin server.
