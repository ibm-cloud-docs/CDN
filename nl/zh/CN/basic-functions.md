---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: running status, additional steps, stop cdn, learn, configure cname, delete cdn, start cdn

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:warning: .warning}
{:download: .download}

# 使 CDN 处于正在运行状态
{: #getting-your-cdn-to-running-status}

了解如何通过遵循这些准则使 CDN 处于 RUNNING 状态。此文档还介绍了如何启动和停止 CDN。

## 进入正在运行状态
{: #get-to-running}

在您创建 CDN 后，它会显示在 CDN 仪表板上。在此处，您将看到 CDN 的名称、源、提供者和状态。  

 ![映射列表屏幕快照](images/mapping-list.png)


如果订购的是支持使用通配符证书的 HTTP 或 HTTPS 的 CDN，那么可以继续执行步骤 1。

如果是使用 HTTPS DV SAN 证书创建的 CDN，那么可能需要执行其他步骤来验证域，这些步骤可在[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https)页面上找到。

**步骤 1：**

在您订购 CDN 后，您需要使用您的 DNS 提供者配置 **CNAME**。大部分 DNS 提供者都能够在设置和更改 CNAME 方面给予您指导。

   * 在此期间，您的 CDN 状态将显示为 **CNAME 配置**。请向您的 DNS 提供者咨询，以了解更改何时处于活动中。

   ![CNAME 配置](images/cname-config.png)  

**步骤 2：**

在您使用 DNS 提供者配置 CNAME 后，您可以随时检查状态，方法是从 CDN 状态右侧的溢出菜单中选择**获取状态**。

  ![CNAME 获取状态](images/cname-getstatus.png)  

**步骤 3：**

CNAME 链接完成时，选择**获取状态**会使状态更改为 *RUNNING*，并且 CDN 已就绪可供使用。

恭喜您！现在，您的 CDN 正在运行。在此，[管理 CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#manage-your-cdn) 页面具有有关配置选项（例如，[生存时间](docs/infrastructure/CDN?topic=CDN-manage-your-cdn#setting-content-caching-time-using-time-to-live-)、[清除高速缓存的内容](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#purging-cached-content)和[添加源路径详细信息](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#adding-origin-path-details)）的其他信息。

## 启动 CDN
{: #starting-cdn}

启动 CDN 会通知 DNS 将来自源的流量定向到 Akamai 边缘服务器。启动映射后，DNS 高速缓存仍然可以将流量定向到源，因此在启动映射后，域可能不会立即看到该功能。更新所需的时间取决于 DNS 高速缓存刷新频率，并根据 DNS 提供者而变化。

仅当 CDN 处于`已停止`状态时，才可以启动 CDN。
{: note}

**步骤 1：**

单击溢出菜单（在 CDN 行右侧显示为三个点）中的**启动 CDN**。

  ![溢出菜单](images/start_cdn.png)

**步骤 2：**

此时将显示一个较大的对话窗口，要求确认您要启动该服务。选择**确认**以继续。

**步骤 3：**

如果操作成功，那么将在屏幕的右上角显示一个对话框，让您了解已经成功，同时还会显示时间。

**步骤 4：**

此步骤会使状态更改为 `CNAME 配置`

**步骤 5：**

单击溢出菜单中的**获取状态**。此步骤会使状态更改为`正在运行`。现在，您的 CDN 在正常运行。

## 停止 CDN
{: #stopping-a-cdn}

STOP CDN 功能适用于不超过 7 天的维护时段。7 天后，必须启动 CDN，否则将禁用 CDN，并且不会将流向 CDN CNAME 的流量重定向到源服务器。
{: important}

停止映射后，DNS 查找将切换到源。流量会跳过 CDN 边缘服务器，并且直接从源中访存内容。停止映射后，可能会有很短的一段时间无法访问内容。这是因为 DNS 高速缓存可能仍在将流量定向到 Akamai 边缘服务器。但是，在此期间，Akamai 边缘服务器会拒绝该域的流量。此时间段的持续时间取决于 DNS 高速缓存刷新频率，并根据 DNS 提供者而变化。

仅当 CDN 处于`正在运行`状态时，才可以将其停止。
{: note}

对于配置为使用 HTTPS SAN 证书的 CDN，建议**不要**停止 CDN，因为将 CDN 移回`正在运行`状态后，HTTPS 流量可能不起作用。
{: important}

目前**不**允许停止通配符域的 CDN。
{: important}

**步骤 1：**

从溢出菜单（CDN 状态右侧的三个垂直的点）![溢出菜单](images/stop_cdn.png) 中单击“停止 CDN”。

**步骤 2：**

此时将显示一个较大的对话窗口，要求确认您要停止该服务。选择**确认**以继续。

**步骤 3：**

大约 5 到 15 秒后，状态应该更改为“已停止”

## 删除 CDN
{: #deleting-a-cdn}

要删除 CDN，请执行以下步骤：

从溢出菜单中选择`删除`仅会删除 CDN；不会删除您的帐户。
{: note}

**步骤 1：**

从溢出菜单单击“删除”。

 ![删除 CDN 溢出菜单](images/delete_cdn.png)

**步骤 2：**

此时将显示一个较大的对话窗口，要求确认您要确认。单击**删除**以继续。

如果通过 DV SAN 证书使用 HTTPS 配置了 CDN，那么完成删除过程可能需要最长 5 小时。
{: note}

  ![删除并带有警告](images/delete-with-warning.png)

**步骤 3：**

完成步骤 1 和 2 后，CDN 的状态将为 `Deleting`。删除过程完成后，再次单击溢出菜单中的“获取状态”，然后从 CDN 列表中除去相应行。如果删除过程尚未完成，那么此操作将不起作用。
