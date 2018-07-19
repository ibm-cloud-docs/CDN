---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 度量值

## 如何查看度量值和使用情况？

您可以在**概述**页面查看度量值和使用情况，通过从 **Content Delivery Network** 页面选择 CDN 可访问该页面。**注**：创建 CDN 后，可能最多需要 48 小时才能显示度量值。

## 我已创建 CDN，并且有通过 CDN 的数据流量。为什么不显示度量值？

创建 CDN 后，需要 48 小时才能显示度量值。


## 有没有我可以查看度量值的最少天数？有最多天数吗？

有可以使用基于 Akamai 的 IBM Cloud Content Delivery Network 来查看度量值的最少天数和最多天数。可以收集度量值的最少天数是 7 天。可以查看度量值的最多天数是 90 天。对于使用 API 的人来说，建立使用 89 天作为最大值，以将时区的任何差异考虑进去。

## 当总命中数为零时，为什么命中率不为零。
命中率代表从边缘服务器高速缓存传递而非从源服务器传递内容的次数百分比。其计算方式如下：

> ((边缘命中数 - Ingress 命中数)/边缘命中数) * 100



其中：
边缘命中数定义为“从最终用户对边缘服务器的所有命中数”
  
Ingress 命中数定义为“流量从源流至 Akamai 边缘服务器的源或 Ingress 命中数”

因为命中率在帐户级别而非按 CDN 计算，因此命中率对于帐户中的所有 CDN 来说都是相同的。此事实还解释了当特定 CDN 的边缘命中数为零时，为什么命中率可能不为零。

