---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

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

Content Delivery Network (CDN) 是边缘服务器的集合，这些服务器在国家/地区或世界的各个部分进行分布。其 Web 内容通过边缘服务器来提供，该服务器所在的地理区域与请求内容的客户最近。通过此项技术，用户接收内容的延迟时间会比从一个集中位置交付内容所能达到的延迟时间更短。使用该技术，可为客户提供更好的整体体验。

## Content Delivery Network (CDN) 如何工作？

CDN 通过在世界各地的边缘服务器上高速缓存 Web 内容来达成其目的。当用户请求 Web 内容时，内容请求会路由到地理位置与该用户最近的边缘服务器。通过减少内容必须传输的距离，CDN 提供最优的吞吐量、最短的等待时间和提高的性能。 

## 如何创建我的 IBM Cloud Content Delivery Network 服务帐户？

在 CDN 订购流程中，当浏览“供应商选择”菜单后单击**选择**时会创建您的帐户。

## 当 CDN 处于 CNAME_CONFIGURATION 状态时我该怎么办？

对于基于 HTTP 的 CDN，请更新 DNS 记录，以便您的 Web 站点指向与新 CDN 映射相关联的 `CNAME`。对于基于 HTTPS 的 CDN，**无需**此 CDN 更新。

**注**：可能最多需要 15-30 分钟才能使更新变为活动状态。请向您的 DNS 提供者咨询，以获取准确的时间估算。

## 需要付费的项有哪些？

您仅需要为每个 IBM Cloud Content Delivery Network 实例所使用的带宽付费。如果 CDN 未使用带宽，则不会发生收费。带宽价格会根据边缘服务器的区域位置而有所不同。可以在此服务的[入门](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd)部分中，按地理区域查看带宽定价。

## 什么时候支付？

根据在 {{site.data.keyword.BluSoftlayer_notm}} 帐户中所设立的结算周期进行 IBM Cloud Content Delivery Network 收费。

## 如何查看度量和使用情况？

您可以在**概述**页面查看度量和使用情况，通过从 **Content Delivery Network** 页面选择 CDN 可访问该页面。**注**：创建 CDN 后，可能最多需要 48 小时才能显示度量。

## 我已创建 CDN，并且有通过 CDN 的数据流量。为什么度量不显示？

创建 CDN 后，需要 48 小时才能显示度量。

## 有没有我可以查看度量的最小天数？有最大天数吗？

有可以使用基于 Akamai 的 IBM Cloud Content Delivery Network 来查看度量的最小天数和最大天数。可以收集度量的最小天数是 7 天。可以查看度量的最大天数是 90 天。对于使用 API 的人来说，建立使用 89 天作为最大值，以将时区的任何差异考虑进去。

## 当总命中数为零时，为什么命中率不为零。
命中率代表从边缘服务器高速缓存传递而非从源服务器传递内容的次数百分比。其计算方式如下：

```
((边缘命中数 - Ingress 命中数)/边缘命中数) * 100

其中：
边缘命中数定义为“从终端用户对边缘服务器的所有命中数”
Ingress 命中数定义为“源或 Ingress 命中数用于从源到 Akamai 边缘服务器的流量”
```
 
因为命中率在帐户级别而非按 CDN 计算，因此命中率对于帐户中的所有 CDN 来说都是相同的。此事实还解释了当特定 CDN 的边缘命中数为零时，为什么命中率可能不为零。

## 如果从 CDN 的溢出菜单中选择`删除`，会删除我的帐户吗？

不会，此操作仅会删除该 CDN。您的帐户仍然存在，您可以创建其他 CDN。

## 高速缓存使用推送还是拉出？

内容高速缓存使用_源提取_模型完成。源提取是一种方法，通过该方法，数据由边缘服务器从源服务器“拉出”，而不是手动将内容上传至边缘服务器。 

## 生存时间有最大值吗？有最小值吗？

生存时间的最大值为 2,147,483,647 秒，大概等于 68 年！最小值为 30 秒。

## 源和 TTL 条目的数目有限制吗？

有，组合限制为每个 CDN 75 个条目。

## 我可以拥有的 CDN 的数目有限制吗？

有，每个帐户的 CDN 数限制为 10。

## 对于 CDN 服务配置，有建议使用的浏览器吗？

有，建议使用 Firefox 和 Chrome 浏览器。建议在使用 IBM Cloud Content Delivery Network 时，采用这些浏览器的最新版本。

## 创建 CDN 时提供路径的目的是什么？

如果在创建 CDN 时提供路径，那么您可以将可通过 CDN 提供的文件与特定源服务器分开。

## 如何了解 CDN 的运作情况？
运行下面的“curl”命令，但将“origin.cdntesting.net/assets/ibm_3d.gif”替换为源上的相应文件路径： 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
如果命令的输出与下列格式匹配，则表示 CDN 如预期那样运作：
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## 我的 CDN 处于错误状态。我现在该怎么做？
请参阅[获取帮助和支持](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp)页面，或者在[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 开具凭单。

## 如果我未提供 CNAME，那么在哪里能够找到它？ 
单击您的 CDN 以访问门户网站中的**概述**页面。在右下角，您可以查看具有 `CName` 信息的**详细信息**部分。

## 可以同时处于活动状态的清除请求数目有限制吗？
有。一次只能有 5 个活动的清除请求。

## 我对给定文件路径的清除请求正在进行。我能够对相同的文件路径提交新的请求吗？
不能。给定文件路径的活动清除请求一次只能有一个。

## IBM Cloud Content Delivery Network 服务支持因特网协议 V6 (IPv6) 吗？它是如何运作的？
Akamai 的边缘服务器支持 IPv6（或双堆栈支持）。其旨在帮助仅具有 IPv4 源的客户接受来自 IPv6 客户机的连接，在边缘服务器上将 IPv6 转换为 IPv4，并转发到具有 IPv4 的源。

**注：**不支持使用 IPv6 地址作为源服务器地址来创建 IBM Cloud CDN。

## 主机名的规则是什么？
`主机名`输入字符串**必须**符合以下规则：
  * 由字母数字字符组成
  * 少于 254 个字符
  * 以有效的顶级域名结尾
  * **不能**以“cdnedge.bluemix.net”结尾（此结尾用于 CNAMES 且为保留结尾）

请参阅 RFC 1035 第 2.3.4 部分以获取更多详细信息。

## 定制 CNAME 的命名约定是什么？
`CNAME` 输入字符串必须遵循以下规则：
  * **必须**唯一（不能由其他任何 IBM Cloud CDN 使用）
  * 少于 64 个字母数字字符
  * **不**允许使用特殊字符 `! @ # $ % ^ & *`
  * **不**允许使用下划线 `_`
  * **不**允许使用句点 `.`
  * 允许使用连字符 `-`，但 CNAME 不能以连字符开头或结尾

## 存储区名称的规则是什么？
存储区名称：
  * 必须至少为 1 个字符
  * 长度不能超过 199 个字符
  * 可以包含字母（允许使用大写和小写字母）、数字和连字符
  * 格式**不能**设置为 IP 地址（例如 127.0.0.1）
  * **不能**以句点 (.) 开头
  * 允许以句点 (.) 结尾
  * 标签之间只允许使用一个句点 (.)（例如，不允许使用 my..ibmcloud.bucket）。 

**注**：虽然大写字母和句点可以通过验证，但我们建议您始终使用符合 DNS 标准的存储区名称。

## 源的路径字符串的规则是什么？
创建 CDN 时，路径是可选的。但是，如果提供了路径，那么路径**必须**符合以下规则：
  * 长度少于 1000 个字符
  * 以“/”开头

## 对于**添加源**命令，Path 字符串有要遵循的任何规则吗？
对于**添加源**，路径是必要的。此外，如果 CDN 是使用路径创建的，那么**添加源**的路径开头必须以 CDN 路径作为前缀。例如，如果 CDN 路径指定为 `/storage`，要调用路径为 `/examplePath` 的**添加源**，所提供的路径应该为 `/storage/examplePath`。

## 使用此 CDN 服务创建对象存储器源类型时，我应该提供什么格式的文件扩展名？

文件扩展名应该以逗号分隔。例如，列表“txt, jpg, xml”是有效的列表。任何特定扩展名长度不能超过 10 个字符。

## 允许供 Akamai 使用的 HTTP 和 HTTPS 端口号有任何限制吗？
有。对于 Akamai 供应商，仅允许以下端口号：
72、80-89、443、488、591、777、1080、1088、1111、1443、2080、7001、7070、7612、7777、8000-9001、9090、9901-9908、11080-11110、12900-12949、20410 和 45002。

## 应该使用哪个 URL 来访问 CDN 或源路径下的数据？ 
CDN 映射或源的路径会被视为目录。因此，尝试访问源路径的用户应该将其作为目录（具有斜杠）来访问。例如，如果 `www.example.com` 使用包括 `/images` 目录的路径创建，那么到达它的 URL 应该为 `www.example.com/images/`

省略斜杠（例如，使用 `www.example.com/images`）将会导致**找不到页面**错误。

## 对于 HTTPS，为什么我无法通过 curl 命令连接或使用主机名进行浏览？

HTTPS 目前仅通过通配符证书进行支持。作为此限制的结果，必须使用 CNAME 进行连接；尝试使用主机名进行连接会导致失败。

## 在浏览器上为支持的 CDN 协议装入 CNAME 或主机名时，应该发生哪些预期行为？

|浏览器 URL| 仅支持 HTTP 协议的 CDN| 仅支持 HTTPS 协议的 CDN| 同时支持 HTTP 和 HTTPS 协议的 CDN|
|-------|-----|-----|-----|
|http://hostname| 成功装入| 301 已永久移动| 301 已永久移动|
|https://hostname| 访问被拒绝| 重定向到 IBM Cloud Web 页面| 重定向到 IBM Cloud Web 页面| 
|http://cname| 301 已永久移动| 访问被拒绝| 成功装入| 
|https://cname| 重定向到 IBM Cloud Web 页面| 成功装入| 成功装入|

## 如何为 IBM Cloud Object Storage 设置 Content Delivery Network (COS)？
请参阅关于如何为 IBM Cloud Object Storage 创建 Content Delivery Network 的[教程](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn)。

## 为什么选择 IBM Cloud Object Storage (COS) 作为源时，我的主机名不会在浏览器上装入？

IBM Cloud CDN 配置为使用 IBM COS 作为对象存储器时，直接访问 Web 站点将不起作用。您必须在浏览器的地址栏中指定完整的请求路径（例如，`www.example.com/index.html`）。导致此行为的原因是 IBM COS 中的索引文档限制。

## 可以通过 Akamai CDN 传递的最大文件大小是多少？

对于缺省性能配置，尝试检索或传递大于 1.8 GB 的文件时，会收到 `403 访问被拒绝`响应。如果启用了“大型文件优化”，那么可以下载最大 320 GB 的文件。

## 我收到源证书即将到期的通知。我现在该怎么做？

请遵循 Akamai 中[此文章](https://community.akamai.com/docs/DOC-7708)所概述的步骤进行操作。

## 什么是字节范围请求，如何用于 Akamai CDN？

字节范围请求用于从源服务器中检索部分内容。范围 HTTP 请求头指示服务器应该返回哪部分内容。可以使用一个范围头来一次请求多个部分，然后服务器可以通过多部分响应发回这些范围。如果服务器发回这些范围，它会使用 206（部分内容）状态进行响应。

使用基于 Akamai 的 IBM Cloud CDN 来发送字节范围请求时，对于第一个请求，用户可能会收到 200（正常）响应代码，而对于所有后续请求，会收到 206 响应代码。这是因为 Akamai 边缘服务器以压缩格式向源请求内容。所以，当边缘服务器在其高速缓存中没有对象，也不具有关于对象内容长度的任何信息时，该服务器会向源请求整个对象。在这种情况下，源会向 Akamai 提供没有内容长度头的对象，并且向最终用户提供整个对象，即便这是一个字节范围请求。因此会出现 200 状态码。在后续请求中，边缘服务器在其高速缓存中具有对象，因此将提供 206 状态码。

确保提供 206 响应（对于第一个字节范围请求也是如此）的一种方法是在源服务器上禁用 `Transfer-Encoding: chunked`。

## 基于 Akamai 的 IBM CDN 解决方案包含哪些安全性？

通过使用分布式 Akamai 平台，您可以使用 130 多个国家/地区的 240,000 多台服务器，从而获得无与伦比的规模和弹性。Akamai 平台位于您的基础架构和最终用户之间，充当流量突然激增的第一道防线。Akamai Intelligent Platform 也是一个逆向代理服务器，只侦听和响应端口 80 和 443 上的请求，这意味着其他端口上的流量会在边缘上被丢弃，而不会转发到您的基础架构。
