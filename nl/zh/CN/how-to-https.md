---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 通过 DV SAN 完成 HTTPS 的域控制验证
{: #completing-domain-control-validation-for-https-with-dv-san}

下图概述了 CDN 从创建之时起一直到进入正在运行状态之间将进入的各种状态。

  ![SAN 状态图](images/state-diagram-san.png)

## 域控制验证的初始步骤
{: #initial-steps-to-domain-control-validation}

**步骤 1：**

使用 DV SAN 证书订购 CDN 后，证书请求过程即开始。在此过程中，{{site.data.keyword.cloud}} CDN 会向 Akamai 请求证书。证书变为可用后，Akamai 会向认证中心 (CA) 发出请求。

  * 在此期间，CDN 状态将显示为**正在请求证书**。

    ![“正在请求证书”状态](images/requesting-cert.png)

**步骤 2：**

CA 收到请求后，将发出域验证质询。

  * 发生此情况时，CDN 的状态将更改为**需要域验证**。

    ![需要域验证](images/domain-validation-needed.png)

**步骤 3：**

单击需要验证的 CDN 的名称。此时将打开“概述”页面，在其中可以查看 CDN 的总体状态。在页面顶部会显示一个警报，提醒您需要进行域验证。选择**查看域验证**按钮以打开一个窗口，其中显示完成验证过程所需的质询信息。

   ![需要域验证](images/view-domain-validation.png)

**步骤 4：**完成了有关如何处理域验证质询的部分中的其中一个验证步骤后，CDN 将移至**正在部署证书**状态。在此期间，Akamai 会将已验证的证书分发到其边缘服务器。部署证书可能需要 2 到 4 小时。

  * 此过程完成后，所有域（无论使用的是哪种验证方法）都会移至 **CNAME 配置**状态。

有关 CNAME 配置完成情况以及监视 CDN 的其他信息，可以在[进入正在运行状态](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running)页面上找到。


## 域控制验证
{: #domain-control-validation}

要将您的 CDN 域名添加到 SAN 证书，您必须证明您对域具有管理控制权。此证明过程称为处理域控制验证 (DCV)。您必须在 48 小时内处理 DCV。如果未在 48 小时内处理 DCV，您的请求将到期，您必须重新开始订购过程。以下各部分描述了三种不同的 DCV 处理方法。


### CNAME
{: #cname}

**仅当** CDN **不**提供实时流量时，才建议使用此方法。如果域正在提供实时流量，那么建议您使用“标准”或“重定向”方法来验证域。

要使用此方法，您需要将 CDN 域的 CNAME 记录添加到 DNS 配置中。要使用的 CNAME 值是您在创建 CDN 时使用的 CNAME。此值应该以 `cdnedge.bluemix.net` 域结尾。您无需执行其他任何操作。DCV 将自动从此点开始执行。验证可能需要 2 到 4 小时。部署证书后，您的 CDN 将直接进入 RUNNING 状态。

大部分 DNS 提供者都能够在设置和更改 CNAME 方面给予您指导。下面是典型 CNAME 记录的示例：

|**资源类型**|**主机**|**指向 (CNAME)**|**TTL**|
|------------------|---------|-------------|----------------|
|CNAME|www.example.com|example.cdnedge.bluemix.net|15 分钟|


---
### 标准
{: #standard}

如果选择“标准”方法来进行域验证，那么“域验证”窗口将显示**质询 URL** 和**质询响应**。要完成域验证过程，请将提供的**质询响应**添加到源服务器。添加后，CA 可以使用在**质询 URL** 中指定的 URL 从源服务器中检索**质询响应**。正确配置源服务器后，域验证可能需要 2 到 4 小时。

   ![域验证质询 - 标准](images/domain-validation-standard.png)

要通过“标准”方法成功完成域验证，必须以特定方式配置源服务器。下面概述了 _Apache(TM)_ 和 _Nginx(TM)_ 服务器的示例过程。

**示例情境**
* 源服务器：`www.example.com`
* CDN 域：`cdn.example.com`
* 质询 URL：`http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* 质询响应：`examplechallenge`

#### Apache 配置
{: #apache-configuration}

  * **步骤 1：**登录到运行 Apache2 服务器的机器。

  * **步骤 2：**在 Web 站点内容所在目录中的 `.well-known/acme-challenge/` 下，创建用于质询响应的质询响应文件。Apache2 Web 站点内容的缺省位置为 `/var/www/html/`。对于此示例，质询响应将放置在 `/var/www/html/.well-known/acme-challenge/` 目录中。

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **步骤 3：**如果需要，打开 Apache2 服务器配置文件。`/etc/apache2/apache2.conf` 和 `/etc/apache2/sites-enabled/` 是配置文件的缺省位置。

  * **步骤 4：**如果需要，将 CDN 域作为附加 **ServerAlias** 添加到源的虚拟主机。

  * **步骤 5：**如果必须修改 Apache2 服务器配置，请使用以下命令重新启动 Apache2 服务器，此方法的停机时间最短：

      ```
      apachectl -k graceful
      ```

  * **步骤 6：**在 DNS 中创建 CDN 域和源服务器的 IP 地址之间的一个 A 记录。

#### Nginx 配置
{: #nginx-configuration}

  * **步骤 1：**登录到运行 Nginx 服务器的机器。

  * **步骤 2：**在 Web 站点内容所在目录中的 `.well-known/acme-challenge/` 下，创建用于质询响应的质询响应文件。Nginx Web 站点内容的缺省位置为 `/usr/share/nginx/html/`。对于此示例，质询响应将放置在 `/usr/share/nginx/html/.well-known/acme-challenge/` 目录中。
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **步骤 3：**如果需要，打开 Nginx 服务器配置文件。`/etc/nginx/nginx.conf` 和 `/etc/nginx/conf.d/` 是配置文件的缺省位置。

  * **步骤 4：**如果需要，将 CDN 域作为附加 **server_name** 添加到源的服务器块。

  * **步骤 5：**如果必须修改 Nginx 服务器配置，请使用以下命令重新启动 Nginx 服务器，此方法的停机时间最短：

      ```
      nginx -s reload
      ```

  * **步骤 6：**在 DNS 中创建 CDN 域和源服务器的 IP 地址之间的一个 A 记录。

#### 验证用于处理域验证的此“标准”方法是否已准备就绪，可供 CA 使用
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* 要通过 `curl` 验证此方法是否有效，请对质询 URL 执行以下命令。
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* 要通过浏览器验证此方法是否有效，请尝试在浏览器中访问质询 URL。

无论哪种情况，您都应该能够检索到存储在源服务器上的“域验证质询”文件对象的副本。

#### 用于标准方法的清除操作
{: #clean-up-for-the-standard-method}

在 CDN 达到**正在部署证书**状态后：
1. 除去 `examplechallenge-fileobject` 文件。（可选）
1. 如果需要，请从服务器配置中除去添加的 ServerAlias (Apache2) 或 server_name (Nginx)。（可选）
1. 除去 CDN 域和源服务器 IP 之间的 A 记录。

---
### 重定向
{: #redirect}

单击**重定向**选项卡将显示通过重定向来处理域验证所需的所有信息。这些信息允许 CA 通过源服务器从 Akamai 中检索**质询响应**的副本。正确配置服务器后，域验证可能需要 2 到 4 小时。

   ![域验证质询 - 重定向](images/domain-validation-redirect.png)

要通过“重定向”方法成功完成域验证，您可能需要以特定方式配置 Web 服务器。下面各部分概述了 Apache 和 Nginx 服务器的示例过程。

**示例情境**
* 源服务器：`www.example.com`
* CDN 域：`cdn.example.com`
* 质询 URL：`http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* 重定向 URL：`http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Apache 重定向配置
{: #apache-redirect-configuration}

  * **步骤 1：**登录到运行 Apache2 服务器的机器。

  * **步骤 2：**打开 Apache2 服务器配置文件。`/etc/apache2/apache2.conf` 和 `/etc/apache2/sites-enabled/` 是配置文件的缺省位置。

  * **步骤 3：**在配置文件中的相应位置添加 redirect 语句。如果需要，将 CDN 域作为附加 **ServerAlias** 添加到源的虚拟主机。

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **步骤 4：**使用以下命令重新启动 Apache2 服务器，此方法的停机时间最短：

    ```
    apachectl -k graceful
    ```

  * **步骤 5：**在 DNS 中创建 CDN 域和源服务器的 IP 地址之间的一个 A 记录。

#### Nginx 重定向配置
{: #nginx-redirect-confguration}

  * **步骤 1：**登录到运行 Nginx 服务器的机器。

  * **步骤 2：**打开 Nginx 服务器配置文件。`/etc/nginx/nginx.conf` 和 `/etc/nginx/conf.d/` 是配置文件的缺省位置。

  * **步骤 3：**对于此步骤，有两个同样有效的方法。

    * 选项 1：（建议）添加包含 `return` 伪指令的 `location` 块，以在相应的 `server` 块中执行重定向。如果需要，将 CDN 域作为附加 **server_name** 添加到源的服务器块。

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives
      # ...
    }
    ```

   * 选项 2：在 `server` 块中添加 `rewrite` 伪指令。如果需要，将 CDN 域作为附加 **server_name** 添加到源的服务器块。

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives
      # ...
    }
    ```

  * **步骤 4：**使用以下命令重新启动 Nginx 服务器，此方法的停机时间最短：

    ```
    nginx -s reload
    ```

  * **步骤 5：**在 DNS 中创建 CDN 域和源服务器的 IP 地址之间的一个 A 记录。

#### 验证是否正在执行重定向
{: #verify-that-the-redirect-is-occurring}

完成以下步骤可_仅_将特定质询 URL 的流量重定向到重定向 URL。可以通过 `curl` 或浏览器来验证重定向是否正常工作。

* 要通过 `curl` 验证重定向是否有效，请对质询 URL 执行以下命令。

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* 要通过浏览器验证重定向是否有效，请尝试在浏览器中访问质询 URL。

无论哪种情况，您都应该能够在 Akamai 中原始请求重定向到的 `dcv.akamai.com` 域上检索到“域验证质询”文件对象的副本。

#### 用于重定向方法的清除操作
{: #clean-up-for-the-redirect-method}

在 CDN 达到**正在部署证书**状态后：
1. 从配置文件中除去 redirect 语句或块。（可选）
1. 如果需要，请从服务器配置中除去添加的 ServerAlias (Apache2) 或 server_name (Nginx)。（可选）
1. 除去 CDN 域和源服务器 IP 之间的 A 记录。
