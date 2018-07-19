---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 管理 CDN

## 使用“生存时间”设置内容高速缓存时间

在您的 CDN 运行后，您可以使用生存时间 (TTL) 设置内容高速缓存。特定文件或目录路径的生存时间指出该内容应该高速缓存的时间。在您创建 CDN 映射时，即已创建 3600 秒的缺省全局 TTL。

**步骤 1：**  

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。

**步骤 2：**  

您可以使用箭头或输入新时间，来调整时间。时间值以秒为单位指定。例如，3600 秒等于 1 小时。可以选择的 `timeToLive` 的最小值为 30 秒，而最大值为 2147483647 秒。选择**保存**按钮，可以设置内容高速缓存时间。

  ![添加 ttl](images/adding-path.png)

**步骤 3：**

保存后，您可以使用溢出菜单选项，**编辑**或**删除** TTL 设置。（**注**：无法更改 TTL 的路径。如果更改映射路径，那么 TTL 路径会自动更新。）

  ![编辑或删除 ttl](images/edit-delete-ttl-setting.png)  

  * 当内容与多个规则匹配时，最新添加的配置优先。

  * 仅可以针对特定文件名或目录设置 TTL 值。不支持正则表达式，因为它们可能会产生不可预测的行为。

## 添加源路径详细信息

当您的 CDN 处于 *CNAME 配置*或*正在运行*状态时，您可以添加源路径详细信息。您可以选择从多个源服务器提供内容。例如，可以从不同于视频的服务器提供照片。源可以基于主机服务器或 Object Storage。

**注：**CDN 将为源服务器进行 URL 转换。例如，当用户打开 URL `www.example.com/example/*` 时，如果源 `xyz.example.com` 添加了路径 `/example/*`，那么 CDN 边缘服务器将会从 `xyz.example.com/*` 中检索内容。

**步骤 1：**

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。  

**步骤 2：**

选择**源**选项卡，然后选择**添加源**按钮。此步骤会打开一个新的对话窗口，可以在其中配置源。  

   ![源添加源](images/add-origins.png)

**步骤 3：**

您*必须*提供路径。如果 CDN 是使用路径创建的，那么路径*必须*以 CDN 路径作为前缀开始。
  
例如，如果 CDN 是使用路径 `/examplePath` 创建的，那么源的路径必须以前缀 `/examplePath/` 开头。您可选择提供主机头。  

   ![源添加源](images/add-origin-path.png)

**步骤 4：**

选择**服务器**或 **Object Storage**。

  * 如果您已选择**服务器**，请输入源服务器地址作为 IPv4 地址或_主机名_。建议提供主机名并提供标准域名 (FQDN)。根据 CDN 创建期间所选择的协议，还请提供 HTTP 端口和/或 HTTPS 端口。如果使用 HTTPS 端口，那么源服务器地址**必须**为_主机名_，而不能为 IP 地址。

       ![添加源服务器](images/add-origin-server-default.png)

  * 如果您已选择 **Object Storage**，请提供端点、存储区名称和 HTTPS 端口。选择性地，指定可以在 CDN 服务在使用的文件扩展名。如果未指定任何选项，则允许使用所有文件扩展名。

       ![添加源 Object Storage](images/add-origin-object-storage.png)

  * “服务器”和 Object Storage 配置的**优化**和**高速缓存键**选项是相同的。

    * 从下拉菜单中选择**优化**选项。**常规 Web 交付**是缺省选项，也可以选择**大型文件**或**视频点播**优化。**常规 Web 交付**支持 CDN 提供不超过 1.8 GB 的内容，而**大型文件**优化支持下载 1.8 GB 到 320 GB 的文件。**视频点播**可优化 CDN 的分段流格式交付。[大型文件优化](about.html#large-file-optimization)和[视频点播](about.html#video-on-demand)的功能描述提供了进一步的信息。

        ![性能配置选项](images/performance-config-options.png)

    * 从下拉菜单中选择**高速缓存键**选项。缺省选项为**全部包含**。如果选择**包含指定项**或**忽略指定项**，那么**必须**输入要包含或忽略的查询字符串，各字符串之间用空格分隔。例如，对于一个查询字符串，输入 `uuid=123456`，或者对于两个查询字符串，输入 `uuid=123456 issue=important`。可以在功能描述中了解有关[高速缓存键查询自变量](about.html#cache-key-query-args)的更多信息。

        ![“高速缓存键”选项](images/cache-key-options.png)

**注**：UI 所显示的“协议”和“端口”选项将与订购 CDN 期间所选的内容相匹配。例如，如果在订购 CDN 的过程中选择了 **HTTP 端口**，那么在“添加源”的过程中，仅会显示 **HTTP 端口**选项。

**步骤 5：**

选择**添加**按钮以添加源路径。

  **注**：为 Object Storage 源路径提供文件扩展名时，具有与源路径相同 URL 的 TTL 设置的范围将会扩展，以包含具有这些指定文件扩展名的所有文件。例如，如果您创建源路径 `/example` 并指定文件扩展名为“jpg png gif”，那么 TTL 路径 `/example` 的 TTL 值的范围将会包含 `/example` 目录及其子目录下的所有 JPG/PNG/GIF 文件。

**步骤 6：**

添加后，您可以使用溢出菜单选项，**编辑**或**删除**源。

  ![编辑或删除源](images/edit-delete-origin.png)

## 清除高速缓存内容

在您的 CDN 运行后，您可以从供应商的服务器清除高速缓存内容。

**步骤 1：**

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。

**步骤 2：**

选择**清除**选项卡。


   ![清除页面](images/purge_tab.png)

**步骤 3：**

输入标准 Unix 路径语法，以指出您要清除的文件，然后选择**清除**按钮。目前仅允许对单个文件使用清除。请参阅[规则和命名约定](rules-and-naming-conventions.html#what-are-the-rules-for-the-path-string-for-purge-)页面，以获取有关清除路径所允许语法的更多详细信息。

**步骤 4：**

清除后，会在**清除活动**下列出该活动。您可以使用溢出菜单选项，**重做清除**或**收藏**路径。

   ![清除活动](images/purge-activity.png)

   **注：**如果有超过 15 个清除，那么每 15 天会自动裁剪一次“清除活动”。

## 更新 CDN 配置详细信息

在您的 CDN 运行后，您可以更新 CDN 配置详细信息。

**步骤 1：**

在 CDN 页面上，选择 CDN，这会将您带到**概述**页面。

**步骤 2：**

选择**设置**选项卡。此时将显示 CDN 配置详细信息。

   ![“设置”选项卡](images/settings-tab.png)  
   **注**：仅当 CDN 配置为使用 HTTPS 时，您才会看到 SSL 证书。

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

## 为 CDN 配置 IBM Cloud Object Storage

要使用存储在 IBM Cloud Object Storage 中的对象，您必须为存储区中的每个对象设置“acl”（即访问控制表）属性的值，以允许“public-read”访问权。

请参阅 IBM Cloud Object Storage Developer Center 中的“工具”一节 (https://developer.ibm.com/cloudobjectstorage/)，以安装任何必要的客户机或工具。本指南假设您已安装正式的 AWS 命令行界面，其与 IBM Cloud Object Storage S3 API 兼容。

下面的示例代码显示如何使用命令行界面，为存储区中的所有对象设置“public-read”访问权。

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```
