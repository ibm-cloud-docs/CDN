---

copyright:
  years: 2018
lastupdated: "2018-06-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 使 CDN 处于正在运行状态

了解如何通过遵循这些准则使 CDN 处于 RUNNING 状态。此文档还介绍了如何启动和停止 CDN。

## 进入正在运行状态

在您创建 CDN 后，它会显示在 CDN 仪表板上。在此处，您将看到 CDN 的名称、源、提供者和状态。  

 ![映射列表屏幕快照](images/mapping_list_cname.png)


如果订购的是支持使用通配符证书的 HTTP 或 HTTPS 的 CDN，那么可以继续执行步骤 1。

如果是使用 HTTPS DV SAN 证书创建的 CDN，那么可能需要执行其他步骤来验证域，这些步骤可在[完成 HTTPS 的域控制验证](how-to-https.html#completing-domain-control-validation-for-https)页面上找到。

**步骤 1：**

在您订购 CDN 后，您需要使用您的 DNS 提供者配置 **CNAME**。大部分 DNS 提供者都能够在设置和更改 CNAME 方面给予您指导。

   * 在此期间，您的 CDN 状态将显示为 **CNAME 配置**。请向您的 DNS 提供者咨询，以了解更改何时处于活动中。

   ![CNAME 配置](images/cname-config.png)  

**步骤 2：**

在您使用 DNS 提供者配置 CNAME 后，您可以随时检查状态，方法是从 CDN 状态右侧的溢出菜单中选择**获取状态**。

  ![CNAME 获取状态](images/cname-getstatus.png)  

**步骤 3：**

CNAME 链接完成时，选择**获取状态**会使状态更改为 *RUNNING*，并且 CDN 已就绪可供使用。

恭喜您！现在，您的 CDN 正在运行。在此，[管理 CDN](how-to.html#manage-your-CDN) 页面具有有关配置选项（例如，[生存时间](how-to.html#setting-content-caching-time-using-time-to-live-)、[清除高速缓存的内容](how-to.html#purging-cached-content)和[添加源路径详细信息](how-to.html#adding-origin-path-details)）的其他信息。

## 启动 CDN

仅当 CDN 处于“已停止”状态时，才可以启动它。  

**步骤 1：**

单击溢出菜单（在 CDN 行右侧显示为三个点）中的“启动 CDN”。

  ![溢出菜单](images/start_cdn.png)

**步骤 2：**

此时将显示一个较大的对话窗口，要求确认您要启动该服务。选择**确认**以继续。

**步骤 3：**

如果操作成功，那么将在屏幕的右上角显示一个对话框，让您了解已经成功，同时还会显示时间。

**步骤 4：**

此步骤会使状态更改为“CNAME 配置”

**步骤 5：**

从溢出菜单单击“获取状态”。此步骤会使状态更改为“正在运行”。现在，您的 CDN 在正常运行。

## 停止 CDN

仅当 CDN 处于“正在运行”状态时，才可以停止它。

**步骤 1：**

从溢出菜单（CDN 状态右侧的三个垂直的点）![溢出菜单](images/stop_cdn.png) 中单击“停止 CDN”。

**步骤 2：**

此时将显示一个较大的对话窗口，要求确认您要停止该服务。选择**确认**以继续。

**步骤 3：**

大约 5 到 15 秒后，状态应该更改为“已停止”

## 删除 CDN

要删除 CDN，请执行以下步骤：

**注**：从溢出菜单中选择`删除`仅会删除 CDN；不会删除您的帐户。

**步骤 1：**

从溢出菜单单击“删除”。

 ![删除 CDN 溢出菜单](images/delete_cdn.png)

**步骤 2：**

此时将显示一个较大的对话窗口，要求确认您要确认。单击**删除**以继续。

**注**：如果通过使用 DV SAN 证书的 HTTPS 配置了 CDN，那么完成删除过程可能需要最长 5 小时。

  ![删除并带有警告](images/delete-with-warning.png)

**步骤 3：**

完成步骤 1 和 2 后，CDN 的状态将为 `Deleting`。删除过程完成后，再次单击溢出菜单中的“获取状态”，然后从 CDN 列表中除去相应行。如果删除过程尚未完成，那么此操作将不起作用。
