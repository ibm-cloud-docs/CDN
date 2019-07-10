---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 度量值
{: #metrics}

首次从列表中选择 CDN 时，将打开“概述”页面。在此可以看到所选时间段（缺省值为 30 天）的“总带宽”、“总命中数”和“命中率”。

  ![度量值概述](images/metrics-overview.png)

在概述的正下方，您将看到“总带宽”、“每个区域的带宽”、“总命中数”和“命中数（按类型）”的图形表示。

  ![度量值图形](images/metrics-graphs.png)

**注**：创建 CDN 后，可能最多需要 48 小时才能显示度量值。

## 有没有我可以查看度量值的最少天数？有最多天数吗？

您可以使用基于 Akamai 的 {{site.data.keyword.cloud}} Content Delivery Network 来查看度量值的天数存在最小值和最大值。可以收集度量值的最少天数是 7 天。可以查看度量值的最多天数是 90 天。对于使用 API 的人来说，建立使用 89 天作为最大值，以将时区的任何差异考虑进去。

## 当总命中数为零时，为什么命中率不为零。
命中率代表从边缘服务器高速缓存传递而非从源服务器传递内容的次数百分比。其计算方式如下：

`((边缘命中数 - Ingress 命中数)/边缘命中数) * 100`

其中：

_边缘命中数_定义为“从最终用户对边缘服务器的所有命中数”。  
_Ingress 命中数_定义为“流量从源流至 Akamai 边缘服务器的源或 Ingress 命中数”。

因为命中率在帐户级别而非按 CDN 计算，因此命中率对于帐户中的所有 CDN 来说都是相同的。此事实还解释了当特定 CDN 的边缘命中数为零时，为什么命中率可能不为零。

## 度量值是实时更新吗？

不是。度量值每 24 小时更新一次。
