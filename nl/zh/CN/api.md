---

copyright:
  years: 2017
lastupdated: "2018-01-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}



# CDN API 参考

IBM Cloud 提供的 {{site.data.keyword.BluSoftlayer_notm}} 应用程序编程接口（通常称为 SLAPI）是开发接口，可让开发者和系统管理员直接与 {{site.data.keyword.BluSoftlayer_notm}} 后端系统进行交互。

SLAPI 在客户门户网站中可实现许多功能：如果可以在客户门户网站中进行交互，那么该交互也可以在 SLAPI 中完成。由于您可以通过编程方式，与 {{site.data.keyword.BluSoftlayer_notm}} 环境的所有部分进行交互，因此在 SLAPI 中，您可以使用 API 来自动化任务。

SLAPI 是远程过程调用 (RPC) 系统。每个调用都涉及将数据发送到 API 端点并在返回时接收结构化数据。使用 SLAPI 进行发送和接收数据的格式取决于您所选择的 API 实施。SLAPI 当前使用 SOAP、XML-RPC 或 REST 进行数据传输。

有关 SLAPI 或 IBM Cloud Content Delivery Network (CDN) 服务 API 的更多信息，请参阅 IBM Cloud Development Network 中的以下资源：

* [SLAPI 概述](https://sldn.softlayer.com/article/softlayer-api-overview )
* [SLAPI 入门](http://sldn.softlayer.com/article/getting-started )
* [SoftLayer_Product_Package API](http://sldn.softlayer.com/reference/services/SoftLayer_Product_Package )
* [PHP Soap API 指南](https://sldn.softlayer.com/article/PHP )

----

要开始使用，以下是建议要遵循的 API 调用序列：
* `listVendors` - 提供受支持供应商的列表
* `verifyOrder` - 验证是否可以下订单
* `placeOrder`  - 使用给定供应商创建 CDN 帐户。成功调用 placeOrder 后，最多可创建 10 个 CDN 映射。
* `createDomainMapping` - 创建 CDN 映射
* `verifyDomainMapping` - 将 CDN 状态更改为 _RUNNING_

在您已遵循之前的序列之后，您可以使用其他 API。

[此调用序列中的每个步骤都有可用的示例代码。](cdn-example-code.html#code-examples-using-the-cdn-api)

**注**：对于本文档中显示的大多数 API 调用，**必须**使用具有 `CDN_ACCOUNT_MANAGE` 许可权的用户的 API 用户名和 API 密钥。如果您需要启用此许可权，请咨询您帐户的主用户。（每个 IBM Cloud 客户帐户都提供有一个主用户。）

----
## 供应商的 API
### listVendors
此 API 允许用户列出受支持的 CDN 供应商。需要 `vendorName` 才能创建 CDN 帐户并开始订购 CDN。

* **必需参数**：无
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Vendor` 类型的集合

  可以在此查看供应商容器和用法示例：[供应商容器](vendor-container.html)

----
## 帐户的 API
### verifyCdnAccountExists
检查针对给定的 `vendorName`，调用 API 的用户是否存在 CDN 帐户。

* **必需参数**：`vendorName`：提供有效 CDN 提供者的名称。
* **返回值**：如果存在帐户则为 `true`，否则为 `false`

----
## 域映射的 API
### createDomainMapping
使用提供的输入，此函数可创建给定供应商的域映射，并将其与用户的 {{site.data.keyword.BluSoftlayer_notm}} 帐户标识相关联。必须先使用 `placeOrder` 创建 CDN 帐户，此 API 才有效（请参阅[代码示例](cdn-example-code.html)中的 `placeOrder` API 调用）。成功创建 CDN 后，会创建 `defaultTTL`，其值为 3600 秒。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此查看输入容器中的所有属性：
  [查看输入容器](input-container.html)

  以下属性是输入容器的一部分，可在创建域映射时提供（属性是可选的，除非另有说明）：
    * `vendorName`：（**必需**）提供有效 IBM Cloud CDN 提供者的名称。
    * `origin`：（**必需**）以字符串形式提供源服务器地址。
    * `originType`：（**必需**）源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：（**必需**）以字符串形式提供主机名。
    * `protocol`：（**必需**）支持的协议为 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `path`：将从中提供高速缓存内容的路径。缺省路径为 /\*
    * `httpPort` 和/或 `httpsPort`：（对于主机服务器为**必需**）这两个选项必须对应于所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，并且_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，并且_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号。
    * `header`：指定源服务器使用的主机头信息
    * `respectHeader`：布尔值，如果设置为 `true`，将导致源中的 TTL 设置覆盖 CDN TTL 设置。
    * `cname`：为主机名提供别名。如果未提供，会生成别名。
    * `bucketName`：（仅对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（对于 Object Storage 为可选）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为。如果未提供 `cacheKeyQueryRule` 自变量，将缺省为“include-all”
      * `include-all` - 包含所有查询自变量（**缺省值**）
      * `ignore-all` - 忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量` - 忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量` - 包含这些特定查询自变量。例如，`include: query1 query2`

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合。

  **注**：该集合提供 `uniqueId` 值，其需要作为与映射和源路径相关的后续 API 调用的输入发送。

  [查看映射容器](mapping-container.html)

----
### deleteDomainMapping
基于 `uniqueId` 删除域映射。域映射必须处于以下其中一个状态：_RUNNING_、_STOPPED_、_DELETED_、_ERROR_、_CNAME_CONFIGURATION_ 或 _SSL_CONFIGURATION_。

* **必需参数**：`uniqueId`：要删除的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合
  [查看映射容器](mapping-container.html)。

----
### verifyDomainMapping
验证 CDN 的状态并更新 CDN 映射的 `status`（如果已更改）。初始创建 CDN 映射时，其状态显示为 _CNAME_CONFIGURATION_。此时，您必须更新 DNS 记录，以便 CDN 映射将主机名指向 CNAME。如果您对如何执行更新以及更改在互联网上传播可能花费的时间有疑问，请与您的 DNS 提供商进行核实。通常来说，应该花费 15 到 30 分钟。在该时间之后，必须调用此 `verifyDomainMapping` API 来验证是否完成 CNAME 链。如果 CNAME 链已完成，那么 CDN 映射状态会更改为 _RUNNING_。

可以随时调用此 API 以获取最新的 CDN 映射状态。
域映射必须是以下其中一个状态：_RUNNING_ 或 _CNAME_CONFIGURATION_。

* **必需参数**：`uniqueId`：要验证的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](mapping-container.html)

----
### startDomainMapping
基于 `uniqueId` 启动 CDN 域映射。要启动，域映射必须处于 _STOPPED_ 状态。

* **必需参数**：`uniqueId`：要启动的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](mapping-container.html)

----
### stopDomainMapping
基于 `uniqueId` 停止 CDN 域映射。要启动停止，域映射必须处于 _RUNNING_ 状态。

* **必需参数**：`uniqueId`：要停止的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合

  [查看映射容器](mapping-container.html)

----
### updateDomainMapping
使用户能够更新 `uniqueId` 所识别的映射的属性。可能会更改以下字段：`originHost`、`httpPort`、`httpsPort`、`respectHeader`、`header` 和 `cacheKeyQueryRule` 自变量，如果源类型为对象存储器，那么还会更改 `bucketName` 和 `fileExtension`。要进行更新，域映射必须处于 _RUNNING_ 状态。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此查看输入容器中的所有属性：
  [查看输入容器](input-container.html)

  以下属性是输入容器的一部分，在更新域映射时**必需**提供：
    * `vendorName`：为此映射提供 CDN 提供者的名称。
    * `path`：提供此映射的当前路径
    * `origin`：以字符串形式提供源服务器地址。
    * `originType`：源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：提供主机名。
    * `protocol`：支持的协议为 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `httpPort` 和/或 `httpsPort`：这两个选项必须对应于所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，并且_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，并且_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号。
    * `header`：指定源服务器使用的主机头信息
    * `respectHeader`：布尔值，如果设置为 `true`，将导致源中的 TTL 设置覆盖 CDN TTL 设置。
    * `uniqueId`：创建映射后生成。
    * `cname`：提供 CNAME。如果未提供，那么在创建映射时会生成 CNAME。
    * `bucketName`：（仅对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（仅对于 Object Storage 为**必需**）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：只能更新在 2017 年 11 月 16 日_之后_创建的 CDN 映射的高速缓存键行为规则。以下选项可用于配置高速缓存键行为：
      * `include-all` - 包含所有查询自变量（**缺省值**）
      * `ignore-all` - 忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量` - 忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量` - 包含这些特定查询自变量。例如，`include: query1 query2`
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合
  [查看映射容器](mapping-container.html)。

----
### listDomainMappings
返回当前客户的所有域映射的集合。

* **必需参数**：无
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的集合
  [查看映射容器](mapping-container.html)。

----
### listDomainMappingByUniqueId
基于 CDN 的 `uniqueId` 返回具有单个域对象的集合。

* **必需参数**：`uniqueId`：要返回的映射的 uniqueId
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 类型的单个对象集合
  [查看映射容器](mapping-container.html)。

----
## 源的 API
### createOriginPath
为现有 CDN 和特定客户创建源路径。源路径可以基于主机服务器或 Object Storage。要创建源路径，域映射必须处于 _RUNNING_ 或 _CNAME_CONFIGURATION_ 状态。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此查看输入容器中的所有属性：
  [查看输入容器](input-container.html)

  以下属性是输入容器的一部分，可在创建源路径时提供（属性是可选的，除非另有说明）：
    * `vendorName`：（**必需**）提供有效 IBM Cloud CDN 提供者的名称。
    * `origin`：（**必需**）以字符串形式提供源服务器地址。
    * `originType`：（**必需**）源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：（**必需**）以字符串形式提供主机名。
    * `protocol`：（**必需**）支持的协议为 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `path`：将从中提供高速缓存内容的路径。必须以映射路径开头。例如，如果映射路径为 `/test`，那么源路径可能为 `/test/media`
    * `httpPort` 和/或 `httpsPort`：（**必需**）这两个选项必须对应于所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，并且_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，并且_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号。
    * `header`：指定源服务器使用的主机头信息
    * `uniqueId`：（**必需**）创建映射后生成。
    * `cname`：为主机名提供别名。如果未提供唯一 CNAME，会在创建映射时生成 CNAME。
    * `bucketName`：（对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（对于 Object Storage 为可选）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为：
      * `include-all` - 包含所有查询自变量（**缺省值**）
      * `ignore-all` - 忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量` - 忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量` - 包含这些特定查询自变量。例如，`include: query1 query2`

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 类型的集合

  [查看源路径容器](path-container.html)

----
### updateOriginPath
为现有映射和特定客户更新现有源路径。无法使用此 API 来更改源类型。可以更改以下属性：`path`、`origin`、`httpPort`、`httpsPort`、`header` 和 `cacheKeyQueryRule` 自变量。要进行更新，域映射必须处于 _RUNNING_ 或 _CNAME_CONFIGURATION_ 状态。

* **参数**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 类型的集合。可以在此查看输入容器中的所有属性：
  [查看输入容器](input-container.html)

  以下属性是输入容器的一部分，可在更新源路径时提供（属性是可选的，除非另有说明）：
    * `oldPath`：（**必需**）要更改的当前路径
    * `origin`：（如果是进行更新，则为**必需**）以字符串形式提供源服务器地址。
    * `originType`：（**必需**）源类型可以为 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `path`：（**必需**）要添加的新路径。此路径相对于映射路径。
    * `httpPort` 和/或 `httpsPort`：（如果是进行更新，对于主机服务器为**必需**）这两个选项必须对应于所需协议。如果协议为 `HTTP`，那么必须设置 `httpPort`，并且_不能_设置 `httpsPort`。同样，如果协议为 `HTTPS`，那么必须设置 `httpsPort`，并且_不能_设置 `httpPort`。如果协议为 `HTTP_AND_HTTPS`，那么_必须__同时_设置 `httpPort` 和 `httpsPort`。Akamai 对端口号有特定限制。请参阅[常见问题及解答](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号。
    * `uniqueId`：（**必需**）此源所属的映射的 uniqueId
    * `bucketName`：（仅对于 Object Storage 为**必需**）S3 Object Storage 的存储区名称。
    * `fileExtension`：（仅对于 Object Storage 为**必需**）允许进行高速缓存的文件扩展名。
    * `cacheKeyQueryRule`：（如果是进行更新，则为**必需**）只能更新在 2017 年 11 月 16 日_之后_创建的源路径的高速缓存键行为规则。以下选项可用于配置高速缓存键行为：
      * `include-all` - 包含所有查询自变量（**缺省值**）
      * `ignore-all` - 忽略所有查询自变量
      * `ignore: 空格分隔的查询自变量` - 忽略这些特定查询自变量。例如，`ignore: query1 query2`
      * `include: 空格分隔的查询自变量` - 包含这些特定查询自变量。例如，`include: query1 query2`

* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 类型的集合

  [查看源路径容器](path-container.html)

----
### deleteOriginPath
为现有 CDN 和特定客户删除现有源路径。要进行删除，域映射必须处于 _RUNNING_ 或 _CNAME_CONFIGURATION_ 状态。

* **必需参数**：
  * `uniqueId`：此源路径所属的映射的 uniqueId
  * `path`：要删除的路径

* **返回值**：如果成功删除则返回状态消息；否则抛出异常。

----
### listOriginPath
列出基于 `uniqueId` 的现有映射的源路径。

* **必需参数**：
  * `uniqueId`：提供要列出其源路径的映射的 uniqueid。
* **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 类型的对象的集合

  [查看源路径容器](path-container.html)

----
## 清除的 API
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
  * `saved`返回状态为 _SAVED_ 或 _UNSAVED_ 的清除

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
## 度量的 API  
### 度量的容器类  
```  
class SoftLayer_Container_Network_CdnMarketplace_Metrics  
{  
    /**
     * @var string
     */
    public $type;

    /**
     * @var string[]
     */
    public $names;

    /**
     * @var string[]
     */   
     public $totals;

    /**
     * @var string[]
     */
    public $percentage;

    /**
     * @var string[]
     */
    public $time;

    /**
     * @var string[]
     */
    public $xaxis;

    /**
     * @var string[]
     */
    public $yaxis1;

    /**
     * @var string[]
     */
    public $yaxis2;

    /**
     * @var string[]
     */
    public $yaxis3;

    /**
     * @var string[]
     */
    public $yaxis4;

    /**
     * @var string[]
     */
    public $yaxis5;

    /**
     * @var string[]
     */
    public $yaxis6;

    /**
     * @var string[]
     */
    public $yaxis7;

    /**
     * @var string[]
     */
    public $yaxis8;

    /**
     * @var string[]
     */
    public $yaxis9;

    /**
     * @var string[]
     */
    public $yaxis10;

    /**
     * @var string[]
     */
    public $yaxis11;

    /**
     * @var string[]
     */
    public $yaxis12;

    /**
     * @var string[]
     */
    public $yaxis13;

    /**
     * @var string[]
     */
    public $yaxis14;

    /**
     * @var string[]
     */
    public $yaxis15;

    /**
     * @var string[]
     */
    public $yaxis16;

    /**
     * @var string[]
     */
    public $yaxis17;

    /**
     * @var string[]
     */
    public $yaxis18;

    /**
     * @var string[]
     */
    public $yaxis19;

    /**
     * @var string[]
     */
    public $yaxis20;
}  
```  
### getCustomerUsageMetrics
返回给定时间段内预先确定的统计信息的总数，以向客户帐户直接显示（无图形）。

 * **参数**：`string` `vendorName`、`int` `startDate`、`int` `endDate`、`string` `frequency`

 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingUsageMetrics
返回预先确定的统计信息的总数，以针对给定映射直接显示。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingHitsMetrics
返回给定时间范围内特定频率下每个域映射的命中总数。频率可以是“day”、“week”和“month”，其中每个间隔一个图形的绘制点。返回数据根据 `startDate`、`endDate` 和 `frequency` 进行排序。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingHitsByTypeMetrics
返回给定时间范围内特定频率下的命中总数。频率可以是“day”、“week”和“month”，其中每个间隔一个图形的绘制点。返回数据必须根据 `startDate`、`endDate` 和 `frequency` 进行排序。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingBandwidthMetrics
返回单个 CDN 的边缘命中数。对于每个供应商来说，区域可能不同。按映射。

 * **参数**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
___
### getMappingBandwidthByRegionMetrics
返回给定时间段内预先确定的统计信息的总数，以针对给定映射直接显示（无图形）。缺省情况下，`frequency` 的值为“aggregate”。

 * **参数**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **返回值**：`SoftLayer_Container_Network_CdnMarketplace_Metrics` 类型的对象的集合
