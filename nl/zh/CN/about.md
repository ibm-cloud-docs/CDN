---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: content delivery network, web content, caching, edge servers, streaming content

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 关于 Content Delivery Network (CDN)
{: #about-content-delivery-networks-cdn-}

Content Delivery Network (CDN) 是边缘服务器的集合，这些服务器遍布在世界/国家/地区的众多位置。客户请求内容后，哪个地理区域离该客户最近，就会从那个地理区域中的边缘服务器为客户提供 Web 内容。通过这项技术，最终用户收到内容的延迟时间缩短，为客户带来更好的总体体验。

## CDN 如何工作？
{: #how-does-a-cdn-work}

CDN 通过在世界各地的边缘服务器上高速缓存 Web 内容来达成其目的。当客户请求 Web 内容时，内容请求会路由到地理位置与该客户最近的边缘服务器。通过减少内容必须传输的距离，CDN 提供最优的吞吐量、最短的等待时间和提高的性能。使用基于 Akamai 的 {{site.data.keyword.cloud}} Content Delivery Network，内容提供商能以最少的配置实现从全球各地请求的内容的高效传递。

![高级别 CDN 图](images/high-level-cdn-diagram.png)

## 功能
{: #features}

{{site.data.keyword.cloud}} Content Delivery Network 服务包含以下主要功能：
  * 主机服务器源支持
  * Object Storage 源支持
  * 支持具有非重复路径的多个源
  * 基于路径的 CDN 映射
  * 清除高速缓存的内容
  * 生存时间 (TTL)
  * 以图形视图显示度量值
  * 使用通配符和 DV SAN 证书支持 HTTPS 协议
  * 主机头支持
  * 提供旧内容
  * 高速缓存键查询自变量
  * 内容压缩
  * 大型文件优化
  * 视频点播
  * 地理访问控制
  * 热链接保护

请参阅[功能描述文档](/docs/infrastructure/CDN?topic=CDN-feature-descriptions)以获取完整的功能描述。

## 体系结构图
{: #architectural-diagram}

下图提供了 IBM Cloud CDN 的三层体系结构的简要图表概述。

![体系结构图](images/3-tier-architecture.png)
