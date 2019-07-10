---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# 映射容器
{: #mapping-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 集合包含映射 API 利用的属性。每一个映射 API 都会返回此类型的集合。

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` **类**：

* `vendorName`：有效 {{site.data.keyword.cloud}} CDN 提供者的名称。
* `uniqueId`：每个映射唯一的系统生成的 10 位数标识。在创建映射时生成。
* `domain`：主 CDN 名称。也称为 `hostname`。
* `protocol`：用于设置服务的协议。可以为 HTTP、HTTPS 或两者的组合 (HTTP_AND_HTTPS)。
* `originType`：源主机的类型，当前为“HOST_SERVER”或“OBJECT_STORAGE”。
* `originHost`：源服务器地址（源服务器的主机名或 IPv4 地址）、从中访存内容的端点或存储内容的存储区的名称。必须少于 511 个字符。
* `httpPort`：用于 HTTP 协议的端口号。Akamai 对 HTTP 和 HTTPS 端口的端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以了解允许的端口号。
* `httpsPort`：用于 HTTPS 协议的端口号。Akamai 对 HTTP 和 HTTPS 端口的端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以了解允许的端口号。
* `certificateType`：映射使用的证书类型。可以是 `WILDCARD_CERT` 或 `SHARED_SAN_CERT`。对于 HTTP 映射，值将为 0。
* `cname`：作为主机名别名的规范名称记录。可由用户提供，或由系统生成。如果用户提供，应该少于 64 个字母数字字符，并且必须唯一。如果未提供，将在创建映射时生成该名称。
* `respectHeaders`：布尔值，如果设置为“true”，将导致源中的 TTL 设置覆盖 CDN TTL 设置。
* `header`：指定源服务器使用的主机头信息。
* `status`：映射的状态。状态可以为 CNAME_CONFIGURATION、SSL_CONFIGURATION、RUNNING、STOPPED、DELETED 或 ERROR。
* `bucketName`：S3 Object Storage 的存储区名称。
* `fileExtension`：允许进行高速缓存的文件扩展名。
* `path`：S3 Object Storage 的路径。通常位于服务的对象存储 URL 或 API 部分。
* `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为：
  * `include-all`：包含所有查询自变量（**缺省值**）
  * `ignore-all`：忽略所有查询自变量
  * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，“ignore: query1 query2”
  * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，“include: query1 query2”

特别要注意的是 `uniqueId`，此项在创建映射时生成，并用作后续 API 调用的参数。

例如，对 `listDomainMappingByUniqueid` 的以下调用  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

会返回类似于下面的对象：

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
            [cname] => api-testing.cdnedge.bluemix.net
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
            [performanceConfiguration] => General web delivery
            [protocol] => HTTP
            [respectHeaders] => 1
            [serveStale] => 1
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

调用 `stopDomainMapping` 和 `startDomainMapping` 将返回同一个映射对象，但 `status` 不同。

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
