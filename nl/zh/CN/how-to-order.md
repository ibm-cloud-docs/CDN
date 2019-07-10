---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# 订购 CDN
{: #order-a-cdn}

以下您将了解如何订购 Content Delivery Network (CDN)。您可以在 [{{site.data.keyword.cloud}} 控制台](https://cloud.ibm.com/login)中订购 CDN。

## 准备订购
{: #preparation-for-ordering}

下面说明了如何导航到相应的 CDN 页面来下订单。

**步骤 1**

* 在 [IBM Cloud 控制台](https://cloud.ibm.com/login)中登录到您的帐户

**步骤 2**

单击 [IBM Cloud 目录](https://cloud.ibm.com/catalog/)。从左侧的导航栏中，选择**网络**。

   ![IBM Cloud CDN 导航](images/bluemix_navigation.png)

**步骤 3**

单击 **CDN 磁贴**。

   ![IBM Cloud CDN 图标](images/bluemix_tile.png)


## 订购新的 CDN
{: #order-a-new-cdn}

进入订购页面后，请遵循以下步骤，其中说明了如何继续创建和配置 CDN。

### 步骤 1：创建 CDN 帐户
{: #create-your-cdn-account}

如果您还没有 CDN 帐户，请单击右下角的**创建**，这将创建 CDN 帐户，并将您重定向到“CDN 配置”屏幕。

   ![CDN 概述](images/content-delivery.png)

### 步骤 2：命名 CDN
{: #step-2-name-your-cdn}

填写**配置名称**字段：  

  * 指定**主机名**（**必需**），该值将作为 CDN 的主标识（例如，`example.testingcdn.net`）。  
  * （可选）您可以提供定制 **CNAME**（如 `myfirstcdn.cdnedge.bluemix.net`）。如果未提供 CNAME，系统会为您创建一个。后缀 `cdnedge.bluemix.net` 会自动附加到 CNAME。使用不适当的 CNAME 可能会导致服务终止。

       ![配置名称](images/configure-hostname-cname.png)  

在供应新的 CDN 后，您**必须**使用您的 DNS 提供者来配置 CNAME。
{: note}
### 步骤 3：配置源
{: #step-3-configure-your-origin}

填写**配置源**字段：要配置此字段，您必须选择**服务器**或 **Object Storage** 选项。  

  * **步骤 3，选项 1：服务器选项**

     ![配置源](images/configure-origin-server.png)

      * 必须指定**源服务器地址**（源服务器的主机名或 IPv4 地址）。如果还选择了 **HTTPS 端口**，那么**源服务器地址**必须为主机名，而不能为 IP 地址。

      * 指定**主机头**（可选）。如果未提供，将缺省为**主机名**。请参阅[主机头支持](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support)的功能描述，以获取有关主机头的更多信息。  

      * 提供源上可以在其中检索内容的**路径**（可选）。请查看[基于路径的 CDN 映射](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings)的功能描述，以了解此时添加路径的含义。

      * 您还可以提供 **HTTP 端口**和/或 **HTTPS 端口**。这些字段指出可以使用哪些协议和端口号来联系源服务器。对于非缺省端口号，请参阅[常见问题解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号列表。

      * **SSL 证书**：_仅当_选择了 HTTPS 端口时，才会显示此选项。如果为服务器或 Object Storage 选择 **HTTPS 端口**，那么可以选择**通配符**或 **DV SAN 证书**作为“SSL 证书”选项。这两项都提供了通过 HTTPS 实现的增强安全性。
        * 仅在使用 **CNAME** 时，**通配符证书**才允许传输 HTTPS 流量，并且您不需要执行进一步操作
        * **DV SAN 证书**允许通过域传输的 HTTPS 流量，但需要执行其他步骤进行验证。请参阅[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https)页面，以了解选择此选项所涉及的必需步骤和时间约束。

	     ![配置源服务器](images/ssl-cert-options.png)

  * **步骤 3，选项 2：Object Storage 选项**

    ![配置 Object Storage](images/configure-origin-object-storage.png)

      * **必须**指定从中访存对象的**端点**。

      * 指定**主机头**（可选）。如果未提供，将缺省为**主机名**。请参阅[主机头支持](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support)的功能描述，以获取有关主机头的更多信息。  

      * 提供源上可以在其中检索内容的**路径**（可选）。请查看[基于路径的 CDN 映射](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings)的功能描述，以了解在此添加路径的含义。

      * **必须**提供存储内容的**存储区**的名称。

      * 您还可以提供 **HTTP 端口**和/或 **HTTPS 端口**。这些字段指出可以使用哪些协议和端口号来联系源服务器。对于非缺省端口号，请参阅[常见问题解答](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)，以获取允许的端口号列表。

      * **SSL 证书**：_仅当_选择了 HTTPS 端口时，才会显示此选项。如果为服务器或 Object Storage 选择 **HTTPS 端口**，那么可以选择**通配符**或 **DV SAN 证书**作为“SSL 证书”选项。这两项都提供了通过 HTTPS 实现的增强安全性。
        * 仅在使用 **CNAME** 时，**通配符证书**才允许传输 HTTPS 流量，并且您不需要执行进一步操作
        * **DV SAN 证书**允许通过域传输 HTTPS 流量，但需要执行其他步骤进行验证。请参阅[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https)页面，以了解选择此选项所涉及的必需步骤和时间约束。

        ![配置 HTTPS](images/ssl-cert-options.png)

您必须通过 Cloud Object Storage 提供者，为存储区中的每个对象，将**访问控制表** (ACL) 设置为“public-read”。
{: note}
      
### 步骤 4
{: #step-4}

* 您必须选择正下方的**我已阅读主服务协议并同意其中的条款**（位于**创建**按钮上方）。

* 然后选择右下角的**创建**按钮以创建 CDN。
