---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 故障诊断
{: #troubleshooting}

在本文档中，您将找到多种方法来对 {{site.data.keyword.cloud}} CDN 进行故障诊断。如果您需要联系支持人员，请务必提供您的 CDN 引用号。

## 如何知道 CDN 在正常运行？
{: #how-do-I-know-my-cdn-is-working}

运行以下 `curl` 命令，并将 `http://your.cdn.domain/uri` 替换为您的 CDN 上的相应文件路径：

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

如果 `curl` 命令的输出类似于以下示例格式，说明 CDN 在如预期正常运行：

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```
{: screen}

## 我收到了 503 错误。为什么？
{: #i-received-a-503-error-why}

对于 503 错误，我们看到的最常见原因是 SSL 证书链中的证书存在问题。

您看到的错误可能是：`503 服务不可用`。  

除了 503 错误，您可能还会看到类似于以下内容的消息：`处理请求时发生错误。引用号 30.3598c0ba.1521745157.87201fff`（实际引用号可能会有所不同）。在本例中，错误字符串中的引用号解释为 SSL 握手失败。

要更正此问题，请确保源服务器的 SSL 证书符合以下条件：
  * 证书**必须**是由 Akamai 信任的认证机构颁发的。您可以在[此链接 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates) 中查看 Akamai 可信证书列表
  * 证书**必须**与 CDN 上配置的*主机头*相匹配
  * 证书**不能**为自签名证书
  * 证书**不能**到期

如果使用上述条件验证了源的证书链，但仍然遇到相同的错误，请参阅[获取帮助和支持](/docs/infrastructure/CDN?topic=CDN-gettinghelp)页面。请记下参考错误字符串，并在与我们进行的任何通信中包含此字符串。

## 源为 IBM Cloud Object Storage (COS) 时，我的主机名不会在浏览器上装入。
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

{{site.data.keyword.cloud_notm}} CDN 配置为使用 COS 作为对象存储时，无法直接访问该 Web 站点。您必须在浏览器的地址栏中指定完整的请求路径（例如，`www.example.com/index.html`）。造成此行为的原因是 IBM COS 中的索引文档限制。

## 我无法通过 `curl` 命令或浏览器使用包含 HTTPS 的主机名进行连接。
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

如果 CDN 是使用包含通配符证书的 HTTPS 创建的，那么必须使用 CNAME 来建立连接；例如，`https://www.exampleCname.cdnedge.bluemix.net`。这包括在 2018 年 6 月 18 日之前使用 HTTPS 创建的**所有** CDN。尝试使用主机名进行连接将导致错误。

## 在浏览器上针对支持的协议装入 CNAME 或主机名时，预期的行为是什么？
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

下表显示了从 Web 浏览器装入**主机名**或 **CNAME** 时，受支持协议的预期行为。

<table>
<caption caption-side=“top”>预期行为表</caption>
<thead>
<tr>
<th rowspan=2 scope="col">浏览器 URL</th>
<th rowspan=2 scope="col">CDN（仅使用 HTTP 协议）</th>
<th colspan=2 scope="col">CDN（仅使用 HTTPS 协议）</th>
<th colspan=2 scope="col">CDN（同时使用 HTTP 和 HTTPS 协议）</th>
</tr>
<tr>
<th scope="col"> 通配符</th>
<th scope="col"> 共享 SAN</th>
<th scope="col"> 通配符</th>
<th scope="col"> 共享 SAN</th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> 成功装入</td>
<td> 301 已永久移走</td>
<td> 成功装入</td>
<td> 301 已永久移走</td>
<td> 成功装入</td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> 访问被拒绝</td>
<td> 重定向到 IBM Cloud Web 页面</td>
<td> 成功装入</td>
<td> 重定向到 IBM Cloud Web 页面</td>
<td> 成功装入</td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 已永久移走</td>
		<td> 成功装入</td>
		<td> 301 已永久移走</td>
		<td> 成功装入</td>
		<td> 301 已永久移走</td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> 重定向到 IBM Cloud Web 页面</td>
		<td> 成功装入</td>
		<td> 301 已永久移走</td>
		<td> 成功装入</td>
		<td> 重定向到 IBM Cloud Web 页面</td>
</tr>
</tbody>
</table>

**常见错误消息：**

`301 已永久移走`消息很可能指示您在尝试使用主机名通过 `HTTPS` 或 `HTTP_AND_HTTPS` 协议访问 CDN。由于 HTTPS 通配符证书的限制，您**必须**使用 CNAME 来访问 CDN。

使用**仅限** HTTP 的协议时，如果尝试使用 CNAME 来访问 CDN，那么将收到 `301 已永久移走`消息。在这种情况下，您_只能_使用主机名访问 CDN。

尝试使用不正确的协议来访问 CDN 时，会看到`访问被拒绝`消息。确保对于使用 HTTP 协议创建的 CDN，使用的是 `http`，对于使用 HTTPS 协议创建的 CDN，使用的是 `https`。

**可能的重定向错误：**

URL 重定向到 {{site.data.keyword.cloud_notm}} CDN Web 页面的行为在 URL 对于协议不正确时最常见。如果 CDN 是使用 HTTPS 或 HTTPS_AND_HTTPS 协议创建的，那么必须使用 CNAME 来访问 CDN。例如：对于 HTTPS 映射，使用 `https://examplecname.cdnedge.bluemix.net`，或者对于 HTTP_AND_HTTPS 映射，使用 `http://examplecname.cdnedge.bluemix.net` 或 `https://examplecname.cdnedge.bluemix.net`。

在本例中，URL 会重定向到 {{site.data.keyword.cloud_notm}} CDN Web 页面，因为对于 CDN 的协议，协议和域都不正确。对于_仅_使用 HTTP 协议创建的 CDN，_只能_通过主机名进行访问。例如，`http://example.com`。
