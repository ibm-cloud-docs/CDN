---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 输入容器
{: #input-container}

输入容器是映射和（源）路径类所利用的集合。输入容器为这两个类提供了一组一致的输入属性。

* `vendorName`：有效 {{site.data.keyword.cloud}} CDN 提供者的名称。
* `oldPath`：由 updateOriginPath() 使用。此属性存储当前或“旧”路径的名称。

下面是映射和（源）路径类的公共属性：
* `originType`：源主机的类型，当前为“HOST_SERVER”或“OBJECT_STORAGE”。
* `origin`：源服务器地址（源服务器的主机名或 IPv4 地址）、从中访存内容的端点或存储内容的存储区的名称。必须少于 511 个字符。
* `httpPort`：用于 HTTP 协议的端口号。Akamai 对 HTTP 和 HTTPS 端口的端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号列表。
* `httpsPort`：用于 HTTPS 协议的端口号。Akamai 对 HTTP 和 HTTPS 端口的端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号列表。
* `status`：映射或路径的状态。状态可以为 CNAME_CONFIGURATION、SSL_CONFIGURATION、RUNNING、STOPPED、DELETED 或 ERROR。
* `path`：将从中提供高速缓存内容的路径。缺省路径为 /\*。由 `updateOriginPath` API 使用时，此属性是指要添加的新路径。
* `performanceConfiguration`：映射的性能配置的规范。
* `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为：
  * `include-all`：包含所有查询自变量
  * `ignore-all`：忽略所有查询自变量
  * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，`ignore: query1 query2`
  * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，`include: query1 query2`
* `geoblockingRule`

下面是特定于映射类的属性：

* `uniqueId`：每个映射唯一的系统生成的 10 位数标识。在创建映射时生成。
* `domain`：主 CDN 名称。也称为主机名。
* `protocol`：用于设置服务的协议。可以为 HTTP、HTTPS 或两者的组合 (HTTP_AND_HTTPS)。
* `cname`：作为主机名别名的规范名称记录。可由用户提供，或由系统生成。如果用户提供，应该少于 64 个字母数字字符，并且必须唯一。如果未提供，将在创建映射时生成该名称。
* `certificateType`：映射使用的证书类型。可以是 `WILDCARD_CERT` 或 `SHARED_SAN_CERT`。对于 HTTP 映射，值将为 0。
* `respectHeaders`：布尔值，如果设置为“true”，将导致源中的 TTL 设置覆盖 CDN TTL 设置。
* `header`：指定源使用的主机头信息。

以下属性与 Cloud Object Storage (COS) 相关：  
* `bucketName`：S3 Object Storage 的存储区的唯一名称。  
* `fileExtension`：允许的文件扩展名。

下列属性与配置热链接保护相关：
* `hotlinkProtection`：请参阅[热链接保护类](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)以了解更多详细信息。
