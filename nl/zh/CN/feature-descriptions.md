---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 功能描述
{: #feature-descriptions}

此页面重点介绍了基于 Akamai 技术的 {{site.data.keyword.cloud}} CDN 随附的许多功能。

## 主机服务器源支持
{: #host-server-origin-support}

通过提供源主机名、协议、端口号以及可选的提供内容的路径，IBM Cloud Content Delivery Network (CDN) 可以配置为从主机服务器源提供内容。缺省路径为 `/*`。协议可以为 HTTP 和/或 HTTPS。Akamai 仅支持特定端口号。请参阅[常见问题及解答](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取支持的端口号/范围。

## Object Storage 源支持
{: #object-storage-file-support}

通过提供端点、存储区名称、协议和端口，IBM Cloud CDN 可以配置为从 Object Storage 端点提供内容。（可选）可以指定文件扩展名列表，以仅允许对具有这些扩展名的文件进行高速缓存。存储区中的所有对象都需要设置有匿名读访问权或公共读访问权。

有关[如何使用 CDN 设置 IBM Cloud Object Storage](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) 的此教程提供了更详细的信息。

## 支持具有非重复路径的多个源
{: #support-for-multiple-origins-with-distinct-paths}

在某些情况下，您可能想要从不同的源服务器传递某些内容。例如，您可能想从不同的源服务器提供特定的照片或视频。IBM Cloud CDN 提供的选项支持设置使用多个路径的多个源服务器。此选项支持灵活使用数据存储方式和存储位置。为源服务器提供的路径对于 CDN 应该唯一。源服务器本身并不需要唯一。

## 基于路径的 CDN 映射
{: #path-based-cdn-mappings}

通过在创建 CDN 时提供路径，可以将 IBM Cloud CDN 服务限制于源服务器上的特定目录路径。最终用户只能访问该目录路径中的内容。例如，如果使用 `/videos` 路径创建 CDN `www.example.com`，那么**只能**通过 `www.example.com/videos/*` 对其进行访问。

## 清除高速缓存的内容
{: #purge-cached-content}

IBM Cloud CDN 提供了方便快捷地从边缘服务器中除去或“清除”高速缓存内容的功能。目前清除只能用于文件路径。目前，使用 Akamai 实施的清除会在大约 5 秒内清除文件。该 UI 还提供了查看清除历史记录以及将特定清除路径标记为收藏夹的功能。

## 生存时间 (TTL)
{: #time-to-live-ttl}

生存时间指示边缘服务器将保留该特定文件或目录路径的高速缓存内容的时间量（以秒为单位）。首次创建 CDN 时，将为路径 `/\*` 创建全局生存时间 (TTL)，缺省时间为 3600 秒。TTL 的最小值为 0 秒，最大值为 2147483647 秒。对于每个条目，TTL 路径对于 CDN 都应该唯一。如果多个路径与一个给定内容相匹配，那么将向该内容应用最近配置的路径匹配项。例如，假设有两个 TTL，先创建了 `/example/file`，生存时间值为 3000 秒，随后创建了 `/example/*`，生存时间值为 4000 秒。虽然 `/example/file` 更具体，但因为 `/example/*` 是最近创建的，所以 `/example/file` 的 TTL 为 4000 秒。创建 TTL 条目后，可以针对路径和/或时间对其进行编辑。也可以将其删除。

## 以图形视图显示度量值
{: #metrics-with-graphical-views}

在客户门户网站的“概述”选项卡上，会为每个 CDN 映射单独提供 CDN 度量值。根据 CDN 的使用情况计算两种类型的度量值：一种以图形显示一段时间内的度量值；一种显示为汇总值。

对于以图形显示一段时间内变化的度量值，可以看到三个折线图和一个饼图。三个折线图是：**带宽**、**命中数（按映射）**和**命中数（按类型）**。折线图显示在指定时间范围内每天的活动。**带宽**和**命中数（按映射）**是单折线图，而**命中数（按类型）**细分为针对提供的每种命中类型显示一条折线。饼图以百分比形式显示按区域细分的 CDN 映射带宽。

汇总值显示的度量值包括**带宽用量** (GB)、对 CDN 边缘服务器的**命中总数**以及**命中率**。命中率指示由边缘服务器（而_不是_通过其源）传递内容的次数百分比。目前，命中率显示为所有 CDN 映射的函数，而不仅仅是正在查看的映射。

缺省情况下，汇总数字和图形缺省为显示最近 30 天的度量值，但您可以通过[客户门户网站 ![外部链接图标 ](../../icons/launch-glyph.svg "外部链接图标 ")](https://control.softlayer.com/) 更改此值。两个类别都能够显示 7 天、15 天、30 天、60 天或 90 天时间段的度量值。

## 主机头支持
{: #host-header-support}

边缘服务器与源主机通信时会使用**主机头**。此功能为源主机上的 Web 服务配置方式提供了灵活性。它特定支持一个用例，该用例中的客户机在一个源主机上配置了多个 Web 服务器。如果未提供主机头输入，那么在将源服务器指定为主机名（而不是 IP 地址）时，服务会将源服务器主机名作为缺省 HTTP 主机头。如果主机头没有作为输入提供，并且源服务器作为 IP 地址提供，那么 CDN 主机名（也称为 CDN 域名）会用作缺省 HTTP 主机头。

## HTTPS 协议支持
{: #https-protocol-support}

CDN 可配置为使用 HTTPS 协议安全地向最终用户提供内容。此配置要求必须将 SSL 证书作为 CDN 配置的一部分进行设置。有两种类型的 SSL 证书选项可用于 HTTPS：[通配符证书](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support)和[域验证 (DV) 主题替代名称 (SAN) 证书](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport)。此类型在本文档中也称为 _SAN 证书_。

对于 HTTPS CDN 来说，要使用的 SSL 证书类型是一个重要的考虑事项。通配符证书配置设置速度较快，但缺点是只能通过 CNAME 访问 CDN。SAN 证书设置过程需要 4 到 8 小时才能完成，但它能将 CDN 用于 CDN 域（即，主机名）。在配置期间，SAN 证书还需要额外的[**域控制验证**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san)步骤。使用其中任一证书都没有关联的成本。请参阅[故障诊断文档](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-)，以了解选择给定证书类型的影响。

源主机还必须具有自己的 SSL 证书以用于 CDN 主机名，并且必须由认可的认证中心 (CA) 签署。

作为行业最佳实践，Akamai 只信任根证书，而不信任中间证书，因为信任的中间证书集会频繁更改。您可以在 [Web 上的此位置](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)找到 Akamai 信任的证书。

## 遵守头
{: #respect-headers}

**遵守头**选项支持源中的 HTTP 头配置覆盖相应的 CDN 配置。缺省情况下，此功能已开启，但可以将其关闭。

## 提供旧内容
{: #serve-stale-content}

如果 CDN 边缘服务器收到用户请求，但并没有高速缓存请求的内容，那么边缘服务器会联系源主机以访存该内容。然后会按为该内容指定的生存时间 (TTL) 段来高速缓存该内容。如果在 TTL 到期后收到用户请求，边缘服务器会联系源主机以访存该内容。如果出于某种原因（例如，源主机停止运行或发生网络问题）而无法访问源服务器，那么边缘服务器会向请求提供到期（旧）内容。此功能由 Akamai 支持，并且**不能**关闭。

## 高速缓存键优化
{: #cache-key-optimization}

Akamai 边缘服务器在**高速缓存存储**上高速缓存内容。为了使用**高速缓存存储**中的内容，边缘服务器要使用**高速缓存键**。通常，**高速缓存键**是基于最终用户的 URL 的一部分生成的。在某些情况下，URL 包含的查询函数自变量对于各个用户不同，但传递的内容是相同的。缺省情况下，Akamai 会使用查询函数的自变量来生成高速缓存键，因此会为每个用户生成唯一的高速缓存键。这并不是最佳方法，因为它会导致边缘服务器联系源服务器以使用其他高速缓存键来获取已高速缓存的内容。利用**高速缓存键优化**功能，您可以指定在生成高速缓存键时要包括或忽略哪些查询参数。此功能适用于 CDN 映射配置的 `create` 或 `update`，也适用于源路径的 `create` 或 `update`。

**高速缓存键优化**的值可以在创建 CDN 映射后在**设置**选项卡中配置。对于源路径，可以在执行源路径的 `create` 或 `update` 操作期间进行配置。
{: note}

## 内容压缩
{: #content-compression}

缺省情况下，在 Akamai CDN 中针对以下内容类型启用了**内容压缩**：
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

压缩由边缘服务器进行处理时，内容必须至少为 10 KB。在某些情况下，压缩由源服务器负责处理，在这种情况下，要压缩的文件大小没有限制。如果内容已经由源服务器进行压缩，那么不会再次对其进行压缩。要启用内容压缩，请求头必须定义 `Accept-Encoding: gzip`。

## 大型文件优化
{: #large-file-optimization}

通过大型文件优化，CDN 网络可以优化大于 10 MB 的内容传递。此支持可提高大型文件的性能，并减少等待时间和下载时间。如果不使用此功能，CDN 传递的文件大小不能超过 1.8 GB。此功能支持下载大于 1.8 GB 但最多不超过 320 GB 的文件。

要使大型文件优化生效，**必须**在源服务器上启用字节范围请求。Akamai CDN 为实现此优化所采用的技术称为_部分对象高速缓存_。请求大型文件时，边缘服务器会检查文件是否符合大小要求。这意味着向源服务器请求的大于 10 MB 的文件将以区块形式提供。区块到达边缘服务器后，会对其进行高速缓存并立即提供给用户。与此同时，边缘服务器将预取下一个区块，从而减少等待时间。此过程会一直持续，直到检索完整个文件或连接终止。

启用此功能后，在提供小于 10 MB 的内容时性能会略有下降。因此，建议此功能仅用于提供大型文件。一个典型的用例是在 CDN 配置中创建新的源路径，并为该路径启用大型文件优化。

## 视频点播
{: #video-on-demand}

**视频点播**性能优化能够跨多种网络类型提供高质量流。因为基于 Akamai 的 IBM Cloud CDN 具有预配置的高速缓存控制设置和分布式网络，所以无论您是否制定过针对大规模受众进行缩放的计划，都可以快速完成缩放。

**视频点播**针对 HLS、DASH、HDS 和 HSS 等分段流格式的分发进行了优化。目前，**不**支持实时视频流。您可以通过从“设置”选项卡上**优化对象**下的下拉菜单中选择**视频点播**选项来启用此功能，或者在创建新的源路径期间启用此功能。仅当优化视频文件的传递时才应启用此功能。

## 地理访问控制
{: #geographical-access-control}

地理访问控制是一种基于规则的行为，允许您根据用户的地理位置，为一组用户设置 `access-type` 参数。有两种类型的行为可用：**Allow** 和 **Deny**。

通过 access-type `Allow`，您可以根据区域类型，具体允许流量流至所选区域。允许特定区域的流量会隐式阻止其他所有区域的流量。例如，您可以选择 `Allow` 以允许流量流至所选大洲，例如欧洲和大洋洲，这将阻止其他所有大洲的访问。

另一方面，`Deny` 行为会阻止指定组访问您的服务，但允许其他所有未指定区域的访问。例如，如果将欧洲和大洋洲的地理访问控制的 access-type 设置为 `Deny`，那么这些大洲的用户将**无法**使用您的服务，而所有其他大洲的用户都可以访问您的服务。

此功能可在 CDN 配置的**设置**页面中进行访问。

## 热链接保护
{: #hotlink-protection}

热链接保护是基于规则的行为，让您可以控制是否允许特定 Web 站点访问 CDN 中的内容。如果从 Web 页面上的链接发出 HTTP 请求，并且该链接指向远程资产，那么浏览器中通常包含 `Referer` 头。一个 Web 站点用于从其他 Web 站点访问资产的链接称为_热链接_。有两种类型的行为可用：**ALLOW** 和 **DENY**。

如果 `protectionType` 设置为 `ALLOW`：
* 如果在发送给 CDN 的请求中的 `Referer` 头值与指定的 `refererValues` 之一匹配，那么 CDN **会**提供请求的内容。
* 否则，CDN **不会**提供内容。

如果 `protectionType` 设置为 `DENY`：
* 如果在发送给 CDN 的请求中的 `Referer` 头值与指定的 `refererValues` 之一匹配，那么 CDN **不会**提供请求的内容。
* 否则，CDN 会提供内容。
