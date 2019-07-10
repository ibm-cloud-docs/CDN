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


# 使用字节范围请求
{: #working-with-byte-range-requests}

使用字节范围请求可以从源服务器中检索部分内容。此文档可帮助您了解您可能会看到的响应状态码。

使用基于 Akamai 的 {{site.data.keyword.cloud}} CDN 来发送**字节范围请求**时，对于第一个请求，用户可能会收到 `200（正常）`响应代码，而对于所有后续请求，会收到 `206` 响应代码。原因是 Akamai 边缘服务器以压缩格式请求源中的内容，所以当边缘服务器在其高速缓存中没有对象，也不具有关于对象内容长度的任何信息时，该服务器会转发到源并请求整个对象。作为回应，源会向 Akamai 提供没有内容长度头的对象，并且向最终用户提供整个对象，尽管这是一个字节范围请求。因此会出现 `200` 状态码。在后续请求中，边缘服务器在其高速缓存中具有对象，因此将提供 `206` 状态码。

**范围 HTTP 请求**头指示服务器应该返回哪部分内容。可以使用一个范围头来一次请求多个部分，然后服务器可以通过多部分响应发回这些范围。如果服务器发回这些范围，它会使用 206（部分内容）状态进行响应。

确保提供 206 响应（对于第一个字节范围请求也是如此）的一种方法是在源服务器上禁用 `Transfer-Encoding: chunked`。
