---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 使用 Cache Control `max-age` 控制浏览器高速缓存持续时间

使用 CDN 时，对于每个可以进行高速缓存的位置，都有一种类型的高速缓存：
  * **浏览器级别高速缓存**，浏览器将一段内容高速缓存特定时间量时执行此高速缓存。
  * **边缘级别高速缓存**，CDN 边缘服务器将来自源的一段内容高速缓存特定（但不一定相同）时间量时执行此高速缓存。

有多种方法可以控制内容在浏览器级别高速缓存的时间长度。使用哪种方法取决于以下因素：
  * 在“Respect 头：开启”时，是否已更新或将[更新 CDN 的配置详细信息](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details)。
  * 源服务器是否针对一段给定内容在 Cache-Control 头中提供了 `max-age` 值。 

无论这些因素如何变化，源都需要为目标内容提供非空 Cache-Control 头。如果源没有为边缘服务器提供非空 Cache-Control 头，那么边缘服务器不会向浏览器提供其自己的 Cache-Control 头。

边缘服务器向浏览器发送 Cache-Control 头时，此 Cache-Control 头的 `max-age` 值对应于边缘服务器上的内容变得陈旧而需要通过源重新验证之前剩余的时间量。 

## Respect 头：关闭
Respect 头设置为**关闭**时，可以通过配置 CDN 的 TTL 设置来控制浏览器级别的高速缓存时间。对于每个文件或文件目录，您需要创建 TTL 规则以控制其路径和所需的高速缓存持续时间。

此设置执行以下两个操作：
  * TTL 规则指示边缘服务器将内容高速缓存指定的时间量。
  * 从边缘服务器提供内容时，边缘服务器根据 TTL 持续时间将自己的包含 `max-age` 值的 Cache-Control 头发送到浏览器。

## Respect 头：开启
**Respect 头**设置为**开启**时，可以选择不配置 TTL 规则来控制浏览器级别的内容高速缓存时间。在这种情况下，您需要配置源服务器，以便为一段给定内容提供包含 `max-age` 值的 Cache-Control 头。由于 Respect 头设置为“开启”，因此边缘服务器会使用来自源的 Cache-Control 的 `max-age` 值作为该特定内容的 TTL 持续时间。如果您的 CDN 已有涵盖该内容的 TTL 规则条目，那么边缘服务器实际上仍会使用 `max-age` 值并覆盖该内容的任何现有边缘级别的高速缓存持续时间。

此设置执行以下两个操作：
  * 来自源的 Cache-Control `max-age` 伪指令的值指示边缘服务器将内容高速缓存指定的时间量。
  * 从边缘服务器提供内容时，边缘服务器源将其自己的 Cache-Control 头发送到浏览器，该头包含基于源的 `max-age` 值的 `max-age` 值。

如果使用的是示例中所示的目标方法，那么不会影响该 TTL 规则所涵盖的其他任何内容。

**Respect 头**设置为**开启**时，如果源服务器未在 Cache-Control 头中发送 `max-age` 伪指令，那么浏览器上的高速缓存持续时间可以通过 TTL 规则进行控制。如果确实指定了 `max-age`，那么该值可能会意外覆盖 TTL 规则上的时间值。

## 一览表

|Respect 头|源提供 Cache-Control|特定内容在边缘服务器上的高速缓存持续时间|边缘服务器提供 Cache-Control|
|---|---|---|---|
|开启|是，指定 `max-age`|使用源的 `max-age` 值覆盖|是，`max-age` 表示边缘的内容需要通过源重新验证之前的时间|
|开启|是，不指定 `max-age`|基于 CDN 的 TTL 配置|是，`max-age` 表示边缘的内容需要通过源重新验证之前的时间|
|开启|否|基于 CDN 的 TTL 配置|否|
|关闭|是，指定 `max-age`|基于 CDN 的 TTL 配置|是，`max-age` 表示边缘的内容需要通过源重新验证之前的时间|
|关闭|是，不指定 `max-age`|基于 CDN 的 TTL 配置|是，`max-age` 表示边缘的内容需要通过源重新验证之前的时间|
|关闭|否|基于 CDN 的 TTL 配置|否|

## 更多信息
* 如何[管理 CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Cache-Control 在 [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt) 的第 14.9 节中进行定义
