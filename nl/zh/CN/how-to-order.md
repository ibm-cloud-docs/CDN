---

copyright:
  years: 2017,2018
lastupdated: "2018-06-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 订购 CDN

以下您将了解如何订购 Content Delivery Network (CDN)。可以从[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/) 或 [Bluemix 门户网站](https://www.ibm.com/cloud-computing/bluemix/) 订购 CDN。

## 从控件门户网站：

**步骤 1：**

要开始进行，请使用唯一凭证，登录到[客户门户网站 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/)。

**步骤 2：**

从显示画面顶部的导航栏中，选择**网络 -> CDN**。

   ![“网络”菜单选项](images/network-cdn.png)

**步骤 3：**

在 **Content Delivery Network** 页面上，选择右上角的**订购 CDN** 按钮。

   ![选择“订购 CDN”](images/order-cdn-button.png)

## 从 IBM Cloud 门户网站：

**步骤 1：**

登录到 [IBM Cloud 门户网站](https://www.ibm.com/cloud-computing/bluemix/)

**步骤 2：**

单击 [IBM Cloud 目录](https://console.bluemix.net/catalog/)。从左侧的导航栏中，选择**网络**。

   ![Bluemix CDN 导航](images/bluemix_navigation.png)

**步骤 3：**

单击 **CDN 磁贴**，这会将您带到“供应商选择”屏幕。

   ![Bluemix CDN 图标](images/bluemix_tile.png)


**步骤 4：**

从**选择 CDN 提供者**屏幕，在 CDN 提供者选项之间进行选择。单击**选择**按钮以确定所选选项，然后单击屏幕右下方的**下一步**，以启动供应流程。  
       ![选择 CDN 提供者](images/Vendor_Select_And_Provision.png)

**步骤 5：**

填写**配置名称**字段：  

  * 指定**主机名**（**必需**），该值将作为 CDN 的主标识（例如，_example.testingcdn.net_）。  
  * （可选）您可以提供定制 **CNAME**（如 _myfirstcdn.cdnedge.bluemix.net_）。如果未提供 CNAME，系统会为您创建一个。<此处要包含验证信息>  

       ![配置名称](images/configure-hostname-cname.png)  

    **注**：使用不适当的 CNAME 可能会导致服务终止。

**步骤 6：**

填写**配置源**字段：要配置此字段，您必须选择**服务器**或 **Object Storage** 选项。  

   * 指定**主机头**（可选）。如果未提供，将缺省为**主机名**。请参阅[主机头支持](about.html#host-header-support-)的功能描述，以获取有关主机头的更多信息。  

   * 指定**路径**（可选）。“路径”应该相对于**主机名**。

      ![配置源](images/configure-origin.png)  

  * **服务器选项**：如果您选择**服务器**选项，请输入应该从中高速缓存数据的源服务器的主机名或 IP 地址。
      * 如果您选择此选项，则您必须指定**源服务器地址**（源服务器的主机名或 IPv4 地址）。如果选择 **HTTPS 端口**，那么**源服务器地址**必须为主机名，而不能为 IP 地址。
      * 您还可以提供 **HTTP 端口**和/或 **HTTPS 端口**。这些字段指出可以使用哪些协议和端口号来联系源服务器。对于非缺省端口号，请参阅[常见问题解答](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai)，以获取允许的端口号列表。
      * **SSL 证书**：_仅当_选择了 HTTPS 端口时，才会显示此选项。\* HTTPS 和 SSL 证书配置的其他信息遵循“Object Storage 选项”描述。

	     ![配置源服务器](images/configure-origin-server.png)

  * **Object Storage 选项**：如果您选择 **Object Storage** 选项，那么您必须提供以下信息：
      * 从中访存对象的**端点**、
      * 存储内容的**存储区**，以及
      * **HTTPS 端口**。
      * **SSL 证书**：_仅当_选择了 HTTPS 端口时，才会显示此选项。\* HTTPS 和 SSL 证书配置的其他信息遵循“Object Storage 选项”描述。
      * 您还可以指定可以在 CDN 服务中使用的文件扩展名，以逗号分隔。（如果未指定任何文件扩展名，那么允许所有文件扩展名。）
      * 您必须为**存储区**中的每个**对象**，将**访问控制表** (ACL) 设置为“public-read”。

      	  ![配置 Object Storage](images/configure-origin-cos.png)

  * **SSL 证书**：如果为服务器或 Object Storage 选择 **HTTPS 端口**，那么可以选择**通配符**或 **DV SAN 证书**作为 **SSL 证书**选项。这两项都提供了通过 HTTPS 实现的增强安全性。
    * 仅在使用 **CNAME** 时，**通配符证书**才允许传输 HTTPS 流量，并且您不需要执行进一步操作
    * **DV SAN 证书**允许通过域传输的 HTTPS 流量，但需要执行其他步骤进行验证。

        ![配置 HTTPS](images/configure-https.png)


**步骤 7：**

配置**其他选项**字段：此部分包含**遵守头**字段的配置选项。

   * 当**遵守头**选项处于**开启**状态时，源在头中定义的 TTL 设置将覆盖缺省 CDN TTL。缺省情况下，**遵守头**会设置为**开启**，但是您必须配置此字段。  

        ![其他选项](images/other-options.png)

**步骤 8：**

选择右下角的**创建**按钮，可以创建 CDN。
