---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

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


# CDN API 参考
{: #cdn-api-reference}

{{site.data.keyword.cloud}} 提供的 {{site.data.keyword.cloud}} 基础架构应用程序编程接口（通常称为 SLAPI）是开发接口，可让开发者和系统管理员直接与 {{site.data.keyword.cloud_notm}} 基础架构后端系统进行交互。

SLAPI 在客户门户网站中可实现许多功能：如果可以在客户门户网站中进行交互，那么该交互也可以在 SLAPI 中完成。由于您可以通过编程方式与 {{site.data.keyword.cloud_notm}} 基础架构环境的所有部分进行交互，因此在 SLAPI 中，您可以使用 API 来自动执行任务。

SLAPI 是远程过程调用 (RPC) 系统。每个调用都涉及将数据发送到 API 端点并在返回时接收结构化数据。使用 SLAPI 进行发送和接收数据的格式取决于您所选择的 API 实施。SLAPI 当前使用 SOAP、XML-RPC 或 REST 进行数据传输。

有关 SLAPI 或 {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN) 服务 API 的更多信息，请参阅 {{site.data.keyword.cloud_notm}} Development Network 中的以下资源：

* [SLAPI 概述](https://softlayer.github.io/ )
* [SLAPI 入门](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API 指南](https://softlayer.github.io/article/php/ )

----

要开始使用，以下是建议要遵循的 API 调用序列：
* `listVendors` - 提供受支持供应商的列表
* `verifyOrder` - 验证是否可以下订单
* `placeOrder` - 使用给定供应商创建 CDN 帐户。成功调用 placeOrder 后，最多可创建 10 个 CDN 映射。
* `createDomainMapping` - 创建 CDN 映射
* `verifyDomainMapping` - 将 CDN 状态更改为 _RUNNING_

在您已遵循之前的序列之后，您可以使用其他 API。

[此调用序列中的每个步骤都有可用的示例代码。](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

对于本文档中显示的大多数 API 调用，**必须**使用具有 `CDN_ACCOUNT_MANAGE` 许可权的用户的 API 用户名和 API 密钥。如果您需要启用此许可权，请咨询您帐户的主用户。（每个 IBM Cloud 客户帐户都提供一个主用户。）
{: note}

----
## 供应商的 API
{: #api-for-vendor}

### listVendors
此 API 允许用户列出受支持的 CDN 供应商。需要 `vendorName` 才能创建 CDN 帐户并开始订购 CDN。

* **必需参数**：无
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Vendor` 类型的集合

  可以在此查看供应商容器和用法示例：[供应商容器](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## 帐户的 API
{: #api-for-account}

### verifyCdnAccountExists
检查针对给定的 `vendorName`，调用 API 的用户是否存在 CDN 帐户。

* **必需参数**：`vendorName`：提供有效 CDN 提供者的名称。
* **返回值**：如果存在帐户则为 `true`，否则为 `false`

----
## 域映射的 API
{: #api-for-domain-mapping}

### createDomainMapping
使用提供的输入，此函数可创建给定供应商的域映射，并将其与用户的 {{site.data.keyword.cloud_notm}} 基础架构帐户标识相关联。必须先使用 `placeOrder` 创建 CDN 帐户，此 API 才有效（请参阅[代码示例](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)中的 `placeOrder` API 调用示例）。成功创建 CDN 后，会创建 `defaultTTL`，其值为 3600 秒。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

  [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

  以下属性是输入容器的一部分，可在创建域映射时提供（属性是可选的，除非另有说明）：
    * `vendorName`：（**必需**）提供有效 IBM Cloud CDN 提供者的名称。
    * `origin`：（**必需**）以字符串形式提供源服务器地址。
    * `originType`：（**必需**）源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：（**必需**）以字符串形式提供主机名。
    * `protocol`：（**必需**）支持的协议为 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `certificateType`：对于 HTTPS 协议是**必需**的。`SHARED_SAN_CERT` 或 `WILDCARD_CERT`
    * `path`：将从中提供高速缓存内容的路径。缺省路径为 `/*`
    * `httpPort` 和/或 `httpsPort`：（对于主机服务器为**必需**）这两个选项必须对应所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以了解允许的端口号。
    * `header`：指定源服务器使用的主机头信息
    * `respectHeader`：布尔值，如果设置为 `true`，将导致源中的 TTL 设置覆盖 CDN TTL 设置。
    * `cname`：为主机名提供别名。如果未提供，会生成别名。
    * `bucketName`：（仅对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（对于 Object Storage 为可选）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为。如果未提供 `cacheKeyQueryRule` 自变量，将缺省为“include-all”
      * `include-all`：包含所有查询自变量（**缺省值**）
      * `ignore-all`：忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，`include: query1 query2`

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合。

  **注**：该集合提供 `uniqueId` 值，需要发送该值以作为与映射和源路径相关的后续 API 调用的输入。

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
基于 `uniqueId` 删除域映射。域映射必须处于以下某个状态：_RUNNING_、_STOPPED_、_DELETED_、_ERROR_、_CNAME_CONFIGURATION_ 或 _SSL_CONFIGURATION_。

* **必需参数**：`uniqueId`：要删除的映射的 uniqueId
* **返回**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合
  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
验证 CDN 的状态并更新 CDN 映射的 `status`（如果已更改）。初始创建 CDN 映射时，其状态显示为 _CNAME_CONFIGURATION_。此时，您必须更新 DNS 记录，以便 CDN 映射将主机名指向 CNAME。如果您对如何执行更新以及更改在互联网上传播可能花费的时间有疑问，请与您的 DNS 提供商进行核实。通常来说，应该花费 15 到 30 分钟。在该时间之后，必须调用此 `verifyDomainMapping` API 来验证 CNAME 链是否完整。如果 CNAME 链完整，那么 CDN 映射状态会更改为 _RUNNING_。

可以随时调用此 API 以获取最新的 CDN 映射状态。该域映射的状态必须是 _RUNNING_ 或 _CNAME_CONFIGURATION_。

* **必需参数**：`uniqueId`：要验证的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
基于 `uniqueId` 启动 CDN 域映射。要启动，域映射必须处于 _STOPPED_ 状态。

* **必需参数**：`uniqueId`：要启动的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
基于 `uniqueId` 停止 CDN 域映射。要启动停止，域映射必须处于 _RUNNING_ 状态。

* **必需参数**：`uniqueId`：要停止的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
使用户能够更新 `uniqueId` 所识别的映射的属性。可以更改以下字段：`originHost`、`httpPort`、`httpsPort`、`respectHeader`、`header` 和 `cacheKeyQueryRule` 自变量，如果源类型为 Object Storage，那么也可以更改 `bucketName` 和 `fileExtension`。要进行更新，域映射必须处于 _RUNNING_ 状态。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此查看输入容器中的所有属性：
  [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

  以下属性是输入容器的一部分，在更新域映射时**必需**提供：
    * `vendorName`：提供此映射的 CDN 提供者的名称。
    * `path`：提供此映射的当前路径
    * `origin`：以字符串形式提供源服务器地址。
    * `originType`：源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：提供主机名。
    * `protocol`：支持的协议为 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `httpPort` 和/或 `httpsPort`：这两个选项必须对应所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以了解允许的端口号。
    * `header`：指定源服务器使用的主机头信息
    * `respectHeader`：布尔值，如果设置为 `true`，将导致源中的 TTL 设置覆盖 CDN TTL 设置。
    * `uniqueId`：创建映射后生成。
    * `cname`：提供 CNAME。如果未提供，那么在创建映射时会生成 CNAME。
    * `bucketName`：（仅对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（仅对于 Object Storage 为**必需**）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：只能更新在 2017 年 11 月 16 日_之后_创建的 CDN 映射的高速缓存键行为规则。以下选项可用于配置高速缓存键行为：
      * `include-all`：包含所有查询自变量（**缺省值**）
      * `ignore-all`：忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，`include: query1 query2`
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
返回当前客户的所有域映射的集合。

* **必需参数**：无
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
基于 CDN 的 `uniqueId` 返回具有单个域对象的集合。

* **必需参数**：`uniqueId`：要返回的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的单对象集合

  [查看映射容器](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## 源的 API
{: #apis-for-origin}

### createOriginPath
为现有 CDN 和特定客户创建源路径。源路径可以基于主机服务器或 Object Storage。要创建源路径，域映射必须处于 _RUNNING_ 或 _CNAME_CONFIGURATION_ 状态。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

  [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

  以下属性是输入容器的一部分，可在创建源路径时提供（属性是可选的，除非另有说明）：
    * `vendorName`：（**必需**）提供有效 IBM Cloud CDN 提供者的名称。
    * `origin`：（**必需**）以字符串形式提供源服务器地址。
    * `originType`：（**必需**）源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：（**必需**）以字符串形式提供主机名。
    * `protocol`：（**必需**）支持的协议为 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `path`：将从中提供高速缓存内容的路径。必须以映射路径开头。例如，如果映射路径为 `/test`，那么源路径可能为 `/test/media`
    * `httpPort` 和/或 `httpsPort`：（**必需**）这两个选项必须对应所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以了解允许的端口号。
    * `header`：指定源服务器使用的主机头信息
    * `uniqueId`：（**必需**）创建映射后生成。
    * `cname`：为主机名提供别名。如果未提供唯一 CNAME，会在创建映射时生成 CNAME。
    * `bucketName`：（对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（对于 Object Storage 为可选）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为：
      * `include-all`：包含所有查询自变量（**缺省值**）
      * `ignore-all`：忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，`include: query1 query2`

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 类型的集合

  [查看源路径容器](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
为现有映射和特定客户更新现有源路径。无法使用此 API 来更改源类型。可以更改以下属性：`path`、`origin`、`httpPort`、`httpsPort`、`header` 和 `cacheKeyQueryRule` 自变量。要进行更新，域映射必须处于 _RUNNING_ 或 _CNAME_CONFIGURATION_ 状态。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

  [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

  以下属性是输入容器的一部分，可在更新源路径时提供（属性是可选的，除非另有说明）：
    * `oldPath`：（**必需**）要更改的当前路径
    * `origin`：（如果是进行更新，就是**必需**的）以字符串形式提供源服务器地址。
    * `originType`：（**必需**）源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `path`：（**必需**）要添加的新路径。此路径相对于映射路径。
    * `httpPort` 和/或 `httpsPort`：（如果是进行更新，对于主机服务器为**必需**）这两个选项必须对应所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以了解允许的端口号。
    * `uniqueId`：（**必需**）此源所属的映射的 uniqueId
    * `bucketName`：（仅对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（仅对于 Object Storage 为**必需**）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：（如果是进行更新，就是**必需**的）只能更新在 2017 年 11 月 16 日_之后_创建的源路径的高速缓存键行为规则。以下选项可用于配置高速缓存键行为：
      * `include-all`：包含所有查询自变量（**缺省值**）
      * `ignore-all`：忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，`include: query1 query2`

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 类型的集合

  [查看源路径容器](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
为现有 CDN 和特定客户删除现有源路径。要进行删除，域映射必须处于 _RUNNING_ 或 _CNAME_CONFIGURATION_ 状态。

* **必需参数**：
  * `uniqueId`：此源路径所属的映射的 uniqueId
  * `path`：要删除的路径

* **返回值**：如果成功删除就返回状态消息；否则抛出异常。

----
### listOriginPath
列出基于 `uniqueId` 的现有映射的源路径。

* **必需参数**：
  * `uniqueId`：提供要列出其源路径的映射的 uniqueId。
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 类型的对象的集合

  [查看源路径容器](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## 清除的 API
{: #api-for-purge}

### 清除的容器类
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
创建清除记录并将其插入到数据库。

* **参数**：
  * `uniqueId`：将创建清除的映射的 uniqueId
  * `path`：要创建的清除的路径

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 类型的集合

----
### getPurgeHistoryPerMapping
基于 `uniqueId` 和 `saved` 状态，返回 CDN 的清除历史记录。（缺省情况下，`saved` 的值会设置为 _UNSAVED_。）

* **参数**：
  * `uniqueId`：将检索其清除历史记录的映射的 uniqueId
  * `saved`：返回状态曾经为 _SAVED_ 或 _UNSAVED_ 的清除

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 类型的集合

----
### saveOrUnsavePurgePath
更新清除路径条目的状态，以指示是否应该保存该清除路径。如果保存清除路径，请创建新的 `saved` 清除。如果路径为 `unsaved`，请删除已保存的清除记录。

* **参数**：
  * `uniqueId`：清除所属的映射的 uniqueId
  * `path`：要保存或取消保存的清除的路径
  * `saved`：_SAVED_ 或 _UNSAVED_

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 类型的集合

----
## 生存时间的 API
{: #api-for-time-to-live}

### TimeToLive 类变量：  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
创建新 `TimeToLive` 对象并将其插入到数据库。

 * **参数**：`string` `uniqueId`、`string` `path`、`int` `ttl`
 * **返回值**：`SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` 类型的对象
___
### updateTtl
更新现有 `TimeToLive` 对象。如果 _oldTtl_ 和 _newTtl_ 输入相等，则会及早退出。

 * **参数**：`string` `uniqueId`、`string` `oldPath`、`string` `newPath`、`int` `oldTtl`、`int` `newTtl`
 * **返回值**：`SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` 类型的对象
___
### deleteTtl
从数据库删除现有的 `TimeToLive` 对象。

 * **参数**：`string` `uniqueId`、`string` `pathName`
 * **返回值**：具有删除状态的字符串
___
### listTtl
基于 CDN 的 `uniqueId`，列出现有 `TimeToLive` 对象。

 * **参数**：`string` `uniqueId`
 * **返回值**：`SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` 类型的对象的数组

 ----
## 度量值 API
{: #api-for-metrics}

[查看度量值容器](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
返回给定时间段内预先确定的统计信息的总数，以向客户帐户直接显示（无图形）。

 * **参数**：
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingUsageMetrics
返回预先确定的统计信息的总数，以针对给定映射直接显示。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingHitsMetrics
返回给定时间范围内特定频率下每个域映射的命中总数。频率可以是“day”、“week”和“month”，其中每个间隔一个图形的绘制点。返回数据根据 `startDate`、`endDate` 和 `frequency` 进行排序。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingHitsByTypeMetrics
返回给定时间范围内特定频率下的命中总数。频率可以是“day”、“week”和“month”，其中每个间隔一个图形的绘制点。返回数据必须根据 `startDate`、`endDate` 和 `frequency` 进行排序。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingBandwidthMetrics
返回单个 CDN 的边缘命中数。对于每个供应商来说，区域可能不同。按映射。

 * **参数**：  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingBandwidthByRegionMetrics
返回给定时间段内预先确定的统计信息的总数，以针对给定映射直接显示（无图形）。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合

----
## 地理访问控制的 API
{: #api-for-geographical-access-control}

### createGeoblocking
创建新的地理访问控制规则，并返回新创建的规则。

  * **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

    [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下属性是输入容器的一部分，在创建新的地理访问控制规则时是**必需**的：
    * `uniqueId`：要为其分配规则的映射的 uniqueId
    * `accessType`：指定规则将允许 (`ALLOW`) 还是拒绝 (`DENY`) 流至给定区域的流量
    * `regionType`：应用地理访问控制规则的区域类型 - `CONTINENT` 或 `COUNTRY_OR_REGION`
    * `regions`：列出将应用 `accessType` 的位置的数组

      请参阅 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) 页面，以查看可能区域的列表。

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 的对象

    [查看 Geo-blocking 类](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
更新现有域映射的现有地理访问控制规则，并返回更新后的规则。

  * **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

    [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下属性是输入容器的一部分，可在更新地理访问控制规则时提供（参数是可选的，除非另有说明）：
    * `uniqueId`：（**必需**）要更新的规则所属的映射的 uniqueId
    * `accessType`：指定规则将允许 (`ALLOW`) 还是拒绝 (`DENY`) 流至给定区域的流量
    * `regionType`：应用规则的区域类型 - `CONTINENT` 或 `COUNTRY_OR_REGION`
    * `regions`：列出将应用 `accessType` 的位置的数组

      请参阅 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) 页面，以查看可能区域的列表。

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 的对象

    [查看 Geo-blocking 类](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
从现有域映射中除去现有地理访问控制规则。

  * **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

    [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下属性是输入容器的一部分，在删除地理访问控制规则时是**必需**的：
    * `uniqueId`：提供要删除的规则所属的映射的 uniqueId。

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 的对象

    [查看 Geo-blocking 类](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
从数据库中检索映射的地理访问控制行为。

  * **参数**：
    * `uniqueId`：规则所属的映射的 uniqueId。

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 的对象

    [查看 Geo-blocking 类](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
返回允许创建地理访问控制规则的类型和区域的列表。

  * **参数**：
    * `uniqueId`：域映射的 uniqueId

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type` 的对象

    [查看 Geo-blocking 类](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## 热链接保护的 API
{: #api-for-hotlink-protection}

### createHotlinkProtection
创建新的热链接保护，并返回新创建的行为。

  * **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

    [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下属性是输入容器的一部分，在创建新的热链接保护时是**必需**的：
    * `uniqueId`：要为其分配行为的映射的 uniqueId
    * `protectionType`：指定在 Web 页面针对具有与指定 refererValues 中某项匹配的 Referer 头值的内容发出请求时，是允许（“ALLOW”）还是拒绝（“DENY”）对内容的访问权
    * `refererValues`：`protectionType` 行为将对其生效的 Referer URL 匹配项的单空格分隔列表

      请参阅 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) 页面，以查看有效热链接保护值的列表。

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 的对象

    [查看热链接保护类](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
更新现有域映射的现有热链接保护行为，并返回更新后的行为。

  * **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

    [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下属性是输入容器的一部分，在更新现有热链接保护时是**必需**的：
    * `uniqueId`：现有行为所属的映射的 uniqueId
    * `protectionType`：指定在 Web 页面针对具有与指定 refererValues 中某项匹配的 Referer 头值的内容发出请求时，是允许（“ALLOW”）还是拒绝（“DENY”）对内容的访问权 
    * `refererValues`：`protectionType` 行为将对其生效的 Referer URL 匹配项的单空格分隔列表

      请参阅 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) 页面，以查看有效热链接保护值的列表。

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 的对象

    [查看热链接保护类](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
从现有域映射中除去现有热链接保护行为。

  * **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此处查看输入容器中的所有属性：

    [查看输入容器](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下属性是输入容器的一部分，在创建新的热链接保护时是**必需**的：
    * `uniqueId`：要从中除去行为的映射的 uniqueId

  * **返回**：null

----
### getHotlinkProtection
检索映射的当前热链接保护行为。

  * **参数**：
    * `uniqueId`：行为所属的映射的 uniqueId

  * **返回**：类型为 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 的对象

    [查看热链接保护类](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
