---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# 常见问题及解答
{: #faqs}

## 什么是 Content Delivery Network (CDN)？
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Content Delivery Network (CDN) 是边缘服务器的集合，这些服务器遍布在世界/国家/地区的众多位置。其 Web 内容通过边缘服务器来提供，该服务器所在的地理区域与请求内容的客户最近。通过此项技术，用户接收内容的延迟时间会比从一个集中位置交付内容所能达到的延迟时间更短。使用该技术，可为客户提供更好的整体体验。

## Content Delivery Network (CDN) 如何工作？
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

CDN 通过在世界各地的边缘服务器上高速缓存 Web 内容来达成其目的。当用户请求 Web 内容时，内容请求会路由到地理位置与该用户最近的边缘服务器。通过减少内容必须传输的距离，CDN 提供最优的吞吐量、最短的等待时间和提高的性能。

## 如何创建我的 IBM Cloud Content Delivery Network 服务帐户？
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

您的帐户是在 CDN 订购过程中创建的。如果是从原有门户网站创建 CDN，那么单击**网络 -> CDN** 页面下的**订购 CDN** 按钮时，将创建您的帐户。如果是从 {{site.data.keyword.cloud}} 门户网站创建 CDN，那么单击**目录 -> 网络 -> 内容交付网络**页面下的**创建**按钮时，将创建您的帐户。

## 当 CDN 处于 CNAME 配置状态时我该怎么办？
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

对于基于 HTTP 和 SAN 证书的 HTTPS CDN，请更新 DNS 记录，以便您的 Web 站点指向与新 CDN 映射相关联的 `CNAME`。对于基于通配符证书的 HTTPS CDN，**无需**此 DNS 更新。

**注**：可能最多需要 15-30 分钟才能使更新变为活动状态。请向您的 DNS 提供者咨询，以获取准确的时间估算。

## 如何在 DNS 中为我的 CDN 域添加 CNAME 记录？
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

在 CDN 域的 DNS 配置页面中，可以创建 CNAME 记录，用 CDN 域名作为主机，用您用来配置 CDN 的 IBM CNAME 作为 CNAME。IBM CNAME 以 `cdnedge.bluemix.net` 结尾。

在“DNS 配置”页面上，典型的 CNAME 记录如下所示：

|**资源类型**|**主机**|**指向 (CNAME)**|**TTL**|
|------------------|---------|-------------|----------------|
|CNAME|www.example.com|example.cdnedge.bluemix.net|15 分钟|


## CDN 中需要付费的项有哪些
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

您只需要按 IBM Cloud Content Delivery Network 实例所使用的带宽付费。如果 CDN 未使用带宽，则不会发生收费。带宽价格会根据边缘服务器的区域位置而有所不同。可以在此服务的[定价文档](/docs/infrastructure/CDN?topic=CDN-pricing)中，按地理区域查看带宽定价。

## 什么时候要为 CDN 付费？
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

根据在 {{site.data.keyword.BluSoftlayer_notm}} 帐户中所设立的结算周期进行 IBM Cloud Content Delivery Network 收费。

## 如果从 CDN 的溢出菜单中选择`删除`，会删除我的帐户吗？
{: faq}

不会，此操作仅会删除该 CDN。您的帐户仍然存在，您可以创建其他 CDN。

## 内容高速缓存使用推送还是拉出？
{: #does-content-caching-use-push-or-pull}
{: faq}

内容高速缓存使用_源提取_模型完成。源提取是一种方法，通过该方法，数据由边缘服务器从源服务器“拉出”，而不是手动将内容上传至边缘服务器。

## 对于 CDN 服务配置，有建议使用的浏览器吗？
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

有，建议使用 Firefox 和 Chrome 浏览器。建议在使用 IBM Cloud Content Delivery Network 时，采用这些浏览器的最新版本。

## 创建 CDN 时提供路径的目的是什么？
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

如果在创建 CDN 时提供路径，那么就可以将可通过 CDN 提供的文件与特定源服务器分开。

## 我的 CDN 处于错误状态。我现在该怎么做？
{: faq}

请参阅[故障诊断](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting)或[获取帮助和支持](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp)页面，或者在[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 中开具凭单。

## 如果我没有为 CDN 提供 CNAME，那么在哪里能够找到它？
{: faq}

单击您的 CDN 以访问门户网站中的**概述**页面。在右下角，您可以查看具有 `CName` 信息的**详细信息**部分。

## 我对给定文件路径的清除请求正在进行。我能够对相同的文件路径提交新的请求吗？

不能。给定文件路径的活动清除请求一次只能有一个。

## IBM Cloud Content Delivery Network 服务支持因特网协议 V6 (IPv6) 吗？它是如何运作的？
{: faq}

Akamai 的边缘服务器支持 IPv6（或双堆栈支持）。其旨在帮助仅具有 IPv4 源的客户接受来自 IPv6 客户机的连接，在边缘服务器上将 IPv6 转换为 IPv4，并转发到具有 IPv4 的源。

**注：**不支持使用 IPv6 地址作为源服务器地址来创建 IBM Cloud CDN。

## 允许供 Akamai 使用的 HTTP 和 HTTPS 端口号有任何限制吗？
{: faq}

有。对于 Akamai 供应商，仅允许以下端口号：
72、80-89、443、488、591、777、1080、1088、1111、1443、2080、7001、7070、7612、7777、8000-9001、9090、9901-9908、11080-11110、12900-12949、20410 和 45002。

## 应该使用哪个 URL 来访问 CDN 或源路径下的数据？
{: faq}

CDN 映射或源的路径会被视为目录。因此，尝试访问源路径的用户应该将其作为目录（具有斜杠）来访问。例如，如果 `www.example.com` 使用包括 `/images` 目录的路径创建，那么到达它的 URL 应该为 `www.example.com/images/`

省略斜杠（例如，使用 `www.example.com/images`）将会导致**找不到页面**错误。

## 如何为 IBM Cloud Object Storage 设置 Content Delivery Network (COS)？
{: faq}

请参阅关于如何为 IBM Cloud Object Storage 创建 Content Delivery Network 的[教程](/docs/tutorials?topic=solution-tutorials-static-files-cdn)。

## 我收到源证书即将到期的通知。我现在该怎么做？
{: faq}

请遵循 Akamai 中[此文章 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://community.akamai.com/docs/DOC-7708) 所概述的步骤进行操作。

## 基于 Akamai 的 IBM CDN 解决方案包含哪些安全性？
{: faq}

通过使用分布式 Akamai 平台，您可以使用 50 多个国家/地区的数千台服务器，从而获得无与伦比的规模和弹性。Akamai 平台位于您的基础架构和最终用户之间，充当流量突然激增的第一道防线。Akamai Intelligent Platform 也是一个逆向代理服务器，只侦听和响应端口 80 和 443 上的请求，这意味着其他端口上的流量会在边缘上被丢弃，而不会转发到您的基础架构。

## Akamai CDN 会保留来自源服务器的 cookie 吗？
{: faq}

对于不可高速缓存的内容或任何未高速缓存的内容，将保留来自源的 cookie。对于边缘服务器高速缓存的内容，不会保留 cookie。

## 如何使用 IBM Cloud 控制台为其他用户授予创建或管理 CDN 的许可权？
{: faq}

帐户的主用户可以为其他用户提供创建和管理 CDN 的许可权。

在 IBM Cloud 控制台主页中，执行以下步骤以编辑许可权：
 * 选择**管理**选项卡
 * 选择**访问权 (IAM)**
 * 单击左侧窗格中的**用户**选项卡
 * 单击所需的**用户**
 * 然后，选择**经典基础架构**选项卡
 * 接下来，在**许可权**选项卡下，展开**服务**类别
 * 选择**管理 CDN 帐户**
 * 单击**保存**按钮

在原有控制台主页中，执行以下步骤以编辑许可权：
 * 选择**帐户**选项卡
 * 选择**用户 -> 用户列表**
 * 单击所需的**用户名**
 * 然后，选择**门户网站许可权**选项卡
 * 选择**服务**选项卡
 * 选择**管理 CDN 帐户**
 * 单击**编辑门户网站许可权**按钮

## 为什么“创建”按钮未在 https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai 页面上显示或该按钮处于禁用状态？
{: faq}

如果您是帐户的主用户，那么必须升级帐户后，“创建”按钮才会在此页面上显示或启用。在 IBM Cloud 控制台页面中，以帐户的主用户身份执行以下步骤：
 * 通过单击 Web 页面左上角的`三横`图标打开左侧导航窗格。
 * 选择**经典基础架构**
 * 单击**升级帐户**按钮并遵循指示信息进行操作

如果您是帐户的其中一个辅助用户，那么帐户的主用户必须授予您`添加/升级服务`许可权后，“创建”按钮才会在此页面上显示或启用。在 IBM Cloud 控制台页面中，帐户的主用户应执行以下步骤以编辑您的许可权：
 * 选择**管理**选项卡
 * 选择**访问权 (IAM)**
 * 单击左侧窗格中的**用户**选项卡
 * 单击所需的**用户**
 * 然后，选择**经典基础架构**选项卡
 * 接下来，在**许可权**选项卡下，展开**帐户**类别
 * 选择**添加/升级服务**
 * 单击**保存**按钮

## 为什么我在使用 `protectionType` `ALLOW` 配置了热链接保护之后无法通过 CDN 访问 Web 页面？
{: faq}

让我们考虑一个示例，示例中最终用户 Web 站点的域配置为您的 CDN 的域/主机名：`cdn.example.com`。如果有人尝试通过在浏览器导航栏中直接导航来访问 Web 页面，浏览器通常不会在 HTTP 请求中发送 Referer 头。例如，如果以此方式直接导航到 `https://cdn.example.com/`，那么 CDN 会认为请求中包含与指定的 `refererValues` 不匹配的项。当 CDN 通过热链接保护对相应的影响或响应求值时，它可以确定是否发生了不匹配。因此，CDN 会拒绝访问，不会“允许”。

## 我可以在 CDN 设置中使用对象存储的私有端点吗？
{: faq}

不，CDN 只能连接到公共端点上的对象存储。

## 我可以在 CDN 服务中使用 Brotli 功能吗？
{: faq}

不，我们基于 Akamai 的 CDN 服务不支持 Brotli 功能。

## 如何在不使用域的情况下创建 CDN 端点？
{: faq}

您可以在不使用域的情况下创建 CDN 端点，但仅适用于类型为**通配符 HTTPS** 的 CDN。在创建**通配符 HTTPS** 类型的 CDN 时，**CNAME** 充当 CDN 端点，**CNAME** 用于提供流量。
