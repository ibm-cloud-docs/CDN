---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 使用 Cache Control 控制 HTTP 客户机的高速缓存持续时间
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

在使用 CDN 时，有两个级别的高速缓存可用：

  * **边缘高速缓存**，在 CDN 边缘服务器高速缓存一段来自源的内容时，发生此级别的高速缓存。
  * **下游高速缓存**，在最终用户或 HTTP 客户机（如提交请求的浏览器）高速缓存一段来自边缘服务器的内容时，从服务器的边缘网络发生此级别的高速缓存。

您选择用哪种方法来控制内容在请求者处（比如浏览器）高速缓存多长时间取决于下列因素：

  * [考虑标题设置](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details)是“打开”还是“关闭”。缺省情况下，设为“打开”。
  * 源服务器是否针对一段特定内容在 Cache-Control 头中提供了 `max-age` 值。 

如果希望边缘服务器针对该内容发送带 Cache-Control 头的 HTTP 响应，那么无论这些因素如何变化，源都必须为边缘提供目标内容的 Cache-Control 头。

尤其重要的是，从边缘服务器下游发送的 Cache-Control 头会要求请求者根据边缘服务器指定的高速缓存伪指令或值来高速缓存关联的内容。

## Respect 头：关闭
{: #respect-header-off}

如果源提供的 Cache-Control 头带有特定内容片段的 `max-age` 伪指令和值，那么该特定内容片段在边缘的高速缓存持续时间仍派生自 CDN 的 TTL 设置。此外，边缘服务器以 Cache-Control `max-age` 值（下面较小的一个）对请求者下游做出响应：
  * 源的 Cache-Control `max-age` 值。
  * 内容在边缘变旧之前剩余的时间。

但是，如果源没有为边缘服务器提供 Cache-Control 头，那么边缘服务器不会向请求者提供 Cache-Control 头。内容的边缘高速缓存持续时间仍派生自 CDN 的 TTL 设置。

## Respect 头：开启
{: #respect-header-on}

如果源提供的 Cache-Control 头带有特定内容片段的 `max-age`，那么源的 Cache-Control `max-age` 值会变成高速缓存在边缘的该特定内容片段的高速缓存持续时间，覆盖适用于该内容片段的任何 TTL 设置。此外，边缘以 Cache-Control `max-age` 值（等于内容在边缘服务器上变旧之前剩余的时间）对请求者做出响应。

但是，如果源没有为边缘服务器提供 Cache-Control 头，那么边缘服务器不会向请求者提供 Cache-Control 头。内容的边缘高速缓存持续时间仍派生自 CDN 的 TTL 设置。

## 一览表
{: #summary}

|Respect 头|源提供 Cache-Control|特定内容在边缘服务器上的高速缓存持续时间|边缘服务器提供 Cache-Control|
|---|---|---|---|
|开启|是，源指定 `max-age`|使用源的 `max-age` 值覆盖边缘高速缓存持续时间|是，边缘也为 `max-age` 提供值，表明边缘需要从源刷新内容的（覆盖）时间。|
|开启|是，但源不指定 `max-age`|基于 CDN 的 TTL 配置的边缘高速缓存持续时间|是，边缘也为 `max-age` 提供值，表明边缘需要从源刷新内容的时间。|
|开启|否|基于 CDN 的 TTL 配置的边缘高速缓存持续时间|否|
|关闭|是，源指定 `max-age`|基于 CDN 的 TTL 配置的边缘高速缓存持续时间|是，边缘也提供 `max-age` 值，是下列两个值中较小的一个：源的 `max-age` 值和边缘需要从源刷新内容的时间。|
|关闭|是，但源不指定 `max-age`|基于 CDN 的 TTL 配置的边缘高速缓存持续时间|是，边缘也为 `max-age` 提供值，表明边缘需要从源刷新内容的时间。|
|关闭|否|基于 CDN 的 TTL 配置的边缘高速缓存持续时间|否|

## 有关高速缓存控制的更多信息
{: #more-information-on-cache-control}

* 如何[管理 CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* Cache-Control 在 [RFC 2616 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ietf.org/rfc/rfc2616.txt) 的第 14.9 节中进行定义
