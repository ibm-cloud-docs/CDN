---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 限制和最大值
{: #limits-and-maximum-values}

## 生存时间有最大值吗？有最小值吗？
{: #is-there-a-maximum-value-for-time-to-live}

生存时间的最大值为 2,147,483,647 秒，大概等于 68 年！最小值为 0 秒。

## 源和 TTL 条目的数目有限制吗？
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

有，组合限制为每个 CDN 75 个条目。

## 可以通过 Akamai CDN 传递的最大文件大小是多少？
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

对于缺省性能配置，尝试检索或传递大于 1.8 GB 的文件时，会收到 `403 访问被拒绝`响应。如果启用了“大型文件优化”，那么可以下载最大 320 GB 的文件。

## 源响应头的最大大小是多少？
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

最大响应头大小为 16 KB。Akamai 在边缘服务器中设置了此限制，以防某些源尝试设置过大的 cookie，导致客户无法在后续请求中返回这些 cookie。如果响应头大于 16 KB，请求将收到 `502 Bad Gateway` 响应。

## 客户机请求头的最大大小是多少？
{: #what-is-the-maximum-size-for-the-client-request-headers}

最大请求头大小为 32 KB。如果请求头大于 32 KB，Akamai 边缘服务器将返回 `400 Bad Request` 响应。
