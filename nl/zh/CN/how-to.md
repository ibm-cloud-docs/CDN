---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

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

# 管理 CDN
{: #manage-your-cdn}

本文档介绍了管理 CDN 的常见任务。

## 使用“生存时间”设置内容高速缓存时间
{: #setting-content-caching-time-using-time-to-live}

在您的 CDN 运行后，您可以使用生存时间 (TTL) 设置内容高速缓存。特定文件或目录路径的生存时间指出该内容应该高速缓存的时间。在您创建 CDN 映射时，即已创建 3600 秒（1 小时）的缺省全局 TTL。

**步骤 1：**  

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。

**步骤 2：**  

您可以使用箭头或输入新时间，来调整时间。时间值以秒为单位指定。例如，3600 秒等于 1 小时。可以选择的 `timeToLive` 的最小值为 0 秒，而最大值为 2147483647 秒（约为 24855 天）。选择**保存**按钮，可以设置内容高速缓存时间。

  ![添加 ttl](images/adding-path.png)

**步骤 3：**

保存后，您可以使用溢出菜单选项，**编辑**或**删除** TTL 设置。（**注**：无法更改 TTL 的路径。如果更改映射路径，那么 TTL 路径会自动更新。）

  ![编辑或删除 ttl](images/edit-delete-ttl-setting.png)  

  * 当内容与多个规则匹配时，最新添加的配置优先。

  * 仅可以针对特定文件名或目录设置 TTL 值。不支持正则表达式，因为它们可能会产生不可预测的行为。

## 添加源路径详细信息
{: #adding-origin-path-details}

当您的 CDN 处于 *CNAME 配置*或*正在运行*状态时，您可以添加源路径详细信息。您可以选择从多个源服务器提供内容。例如，可以从不同于视频的服务器提供照片。源可以基于主机服务器或 Object Storage。

CDN 将为源服务器执行 URL 变换。例如，当用户打开 URL `www.example.com/example/*` 时，如果源 `xyz.example.com` 添加了路径 `/example/*`，那么 CDN 边缘服务器将会从 `xyz.example.com/*` 中检索内容。
{: note}

**步骤 1：**

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。  

**步骤 2：**

选择**源**选项卡，然后选择**添加源**按钮。此步骤会打开一个新的对话窗口，可以在其中配置源。  

   ![源添加源](images/add-origins.png)

**步骤 3：**

您*必须*提供路径。您可选择提供主机头。  

   ![源添加源](images/add-origin-path.png)

**步骤 4：**

选择**服务器**或 **Object Storage**。

  * 如果您已选择**服务器**，请输入源服务器地址作为 IPv4 地址或_主机名_。建议提供主机名并提供标准域名 (FQDN)。根据 CDN 创建期间所选择的协议，还请提供 HTTP 端口和/或 HTTPS 端口。如果使用 HTTPS 端口，那么源服务器地址**必须**为_主机名_，而不能为 IP 地址。

       ![添加源服务器](images/add-origin-server-default.png)

  * 如果您已选择 **Object Storage**，请提供端点、存储区名称和 HTTPS 端口。选择性地，指定可以在 CDN 服务在使用的文件扩展名。如果未指定任何选项，则允许使用所有文件扩展名。

       ![添加源 Object Storage](images/add-origin-object-storage.png)

  * “服务器”和 Object Storage 配置的**优化**和**高速缓存键**选项是相同的。

    * 从下拉菜单中选择**优化**选项。**常规 Web 交付**是缺省选项，也可以选择**大型文件**或**视频点播**优化。**常规 Web 交付**支持 CDN 提供不超过 1.8 GB 的内容，而**大型文件**优化支持下载 1.8 GB 到 320 GB 的文件。**视频点播**可优化 CDN 的分段流格式交付。[大型文件优化](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization)和[视频点播](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand)的功能描述提供了进一步的信息。

        ![性能配置选项](images/performance-config-options.png)

    * 从下拉菜单中选择**高速缓存键**选项。缺省选项为**全部包含**。如果选择**包含指定项**或**忽略指定项**，那么**必须**输入要包含或忽略的查询字符串，各字符串之间用空格分隔。例如，对于一个查询字符串，输入 `uuid=123456`，或者对于两个查询字符串，输入 `uuid=123456 issue=important`。可以在功能描述中了解有关[高速缓存键查询自变量](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args)的更多信息。

        ![“高速缓存键”选项](images/cache-key-options.png)

UI 所显示的“协议”和“端口”选项将与订购 CDN 时所选的内容相匹配。例如，如果在订购 CDN 的过程中选择了 **HTTP 端口**，那么在“添加源”的过程中，仅会显示 **HTTP 端口**选项。
{: note}

**步骤 5：**

选择**添加**按钮以添加源路径。

为 Object Storage 源路径提供文件扩展名时，其 URL 与源路径相同的 TTL 设置的范围限定为包含具有这些指定文件扩展名的所有文件。例如，如果您创建源路径 `/example` 并指定文件扩展名为“jpg png gif”，那么 TTL 路径 `/example` 的 TTL 值的范围将会包含 `/example` 目录及其子目录下的所有 JPG/PNG/GIF 文件。
{: note}

**步骤 6：**

添加后，您可以使用溢出菜单选项，**编辑**或**删除**源。

  ![编辑或删除源](images/edit-delete-origin.png)

## 清除高速缓存内容
{: #purging-cached-content}

在您的 CDN 运行后，您可以从供应商的服务器清除高速缓存内容。

**步骤 1：**

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。

**步骤 2：**

选择**清除**选项卡。


   ![清除页面](images/purge_tab.png)

**步骤 3：**

输入标准 Unix 路径语法，以指示您要清除的文件，然后选择**清除**按钮。仅允许一次清除一个文件。请参阅[规则和命名约定](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-)页面，以获取有关清除路径所允许语法的更多详细信息。

**步骤 4：**

清除后，会在**清除活动**下列出该活动。您可以使用溢出菜单选项，**重做清除**或**收藏**路径。

   ![清除活动](images/purge-activity.png)

如果有超过 15 个清除，那么每 15 天会自动裁剪一次“清除活动”。
{: note}

## 更新 CDN 配置详细信息
{: #updating-cdn-configuration-details}

在您的 CDN 运行后，您可以更新 CDN 配置详细信息。

**步骤 1：**

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。

**步骤 2：**

选择**设置**选项卡。此时将显示 CDN 配置详细信息。

   ![“设置”选项卡](images/settings-tab.png)  

仅当 CDN 配置为使用 HTTPS 时，您才会看到 SSL 证书。
{: note}

对于**服务器**，可以更改以下字段：
  * 主机头
  * 源服务器地址
  * HTTP/HTTPS 端口
  * 提供旧内容
  * 遵守头
  * 优化选项
  * 高速缓存查询    

对于 **Object Storage**，可以更改以下字段：
  * 主机头
  * 端点
  * 存储区名称
  * HTTPS 端口
  * 允许的文件扩展名
  * 提供旧内容
  * 遵守头
  * 优化选项
  * 高速缓存查询

**步骤 3：**

如果必要，请更新**源**或**其他选项**详细信息，然后单击右下角的**保存**按钮，以更新 CDN 配置详细信息。

   ![“保存”按钮](images/save-button.png)
