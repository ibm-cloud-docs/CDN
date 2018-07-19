---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 常见问题及解答

## 什么是 Content Delivery Network (CDN)？

Content Delivery Network (CDN) 是边缘服务器的集合，这些服务器遍布在世界/国家/地区的众多位置。其 Web 内容通过边缘服务器来提供，该服务器所在的地理区域与请求内容的客户最近。通过此项技术，用户接收内容的延迟时间会比从一个集中位置交付内容所能达到的延迟时间更短。使用该技术，可为客户提供更好的整体体验。

## Content Delivery Network (CDN) 如何工作？

CDN 通过在世界各地的边缘服务器上高速缓存 Web 内容来达成其目的。当用户请求 Web 内容时，内容请求会路由到地理位置与该用户最近的边缘服务器。通过减少内容必须传输的距离，CDN 提供最优的吞吐量、最短的等待时间和提高的性能。

## 如何创建我的 IBM Cloud Content Delivery Network 服务帐户？

在 CDN 订购流程中，当浏览“供应商选择”菜单后单击**选择**时会创建您的帐户。

## 当 CDN 处于 CNAME_CONFIGURATION 状态时我该怎么办？

对于基于 HTTP 的 CDN，请更新 DNS 记录，以便您的 Web 站点指向与新 CDN 映射相关联的 `CNAME`。对于基于 HTTPS 的 CDN，**无需**此 CDN 更新。

**注**：可能最多需要 15-30 分钟才能使更新变为活动状态。请向您的 DNS 提供者咨询，以获取准确的时间估算。

## 需要付费的项有哪些？

您只需要按 IBM Cloud Content Delivery Network 实例所使用的带宽付费。如果 CDN 未使用带宽，则不会发生收费。带宽价格会根据边缘服务器的区域位置而有所不同。可以在此服务的[入门](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-)部分中，按地理区域查看带宽定价。

## 什么时候支付？

根据在 {{site.data.keyword.BluSoftlayer_notm}} 帐户中所设立的结算周期进行 IBM Cloud Content Delivery Network 收费。

## 如果从 CDN 的溢出菜单中选择`删除`，会删除我的帐户吗？

不会，此操作仅会删除该 CDN。您的帐户仍然存在，您可以创建其他 CDN。

## 高速缓存使用推送还是拉出？

内容高速缓存使用_源提取_模型完成。源提取是一种方法，通过该方法，数据由边缘服务器从源服务器“拉出”，而不是手动将内容上传至边缘服务器。

## 对于 CDN 服务配置，有建议使用的浏览器吗？

有，建议使用 Firefox 和 Chrome 浏览器。建议在使用 IBM Cloud Content Delivery Network 时，采用这些浏览器的最新版本。

## 创建 CDN 时提供路径的目的是什么？

如果在创建 CDN 时提供路径，那么就可以将可通过 CDN 提供的文件与特定源服务器分开。

## 我的 CDN 处于错误状态。我现在该怎么做？

请参阅[故障诊断](troubleshooting.html#troubleshooting)或[获取帮助和支持](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#getting-help)页面，或者在[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 中开具凭单。

## 如果我未提供 CNAME，那么在哪里能够找到它？

单击您的 CDN 以访问门户网站中的**概述**页面。在右下角，您可以查看具有 `CName` 信息的**详细信息**部分。

## 我对给定文件路径的清除请求正在进行。我能够对相同的文件路径提交新的请求吗？

不能。给定文件路径的活动清除请求一次只能有一个。

## IBM Cloud Content Delivery Network 服务支持因特网协议 V6 (IPv6) 吗？它是如何运作的？

Akamai 的边缘服务器支持 IPv6（或双堆栈支持）。其旨在帮助仅具有 IPv4 源的客户接受来自 IPv6 客户机的连接，在边缘服务器上将 IPv6 转换为 IPv4，并转发到具有 IPv4 的源。

**注：**不支持使用 IPv6 地址作为源服务器地址来创建 IBM Cloud CDN。

## 允许供 Akamai 使用的 HTTP 和 HTTPS 端口号有任何限制吗？

有。对于 Akamai 供应商，仅允许以下端口号：
72、80-89、443、488、591、777、1080、1088、1111、1443、2080、7001、7070、7612、7777、8000-9001、9090、9901-9908、11080-11110、12900-12949、20410 和 45002。

## 应该使用哪个 URL 来访问 CDN 或源路径下的数据？
CDN 映射或源的路径会被视为目录。因此，尝试访问源路径的用户应该将其作为目录（具有斜杠）来访问。例如，如果 `www.example.com` 使用包括 `/images` 目录的路径创建，那么到达它的 URL 应该为 `www.example.com/images/`

省略斜杠（例如，使用 `www.example.com/images`）将会导致**找不到页面**错误。

## 如何为 IBM Cloud Object Storage 设置 Content Delivery Network (COS)？

请参阅关于如何为 IBM Cloud Object Storage 创建 Content Delivery Network 的[教程](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn)。

## 我收到源证书即将到期的通知。我现在该怎么做？

请遵循 Akamai 中[此文章](https://community.akamai.com/docs/DOC-7708)所概述的步骤进行操作。

## 基于 Akamai 的 IBM CDN 解决方案包含哪些安全性？

通过使用分布式 Akamai 平台，您可以使用 130 多个国家/地区的 240,000 多台服务器，从而获得无与伦比的规模和弹性。Akamai 平台位于您的基础架构和最终用户之间，充当流量突然激增的第一道防线。Akamai Intelligent Platform 也是一个逆向代理服务器，只侦听和响应端口 80 和 443 上的请求，这意味着其他端口上的流量会在边缘上被丢弃，而不会转发到您的基础架构。
