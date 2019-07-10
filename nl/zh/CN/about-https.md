---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# 关于 HTTPS
{: #about-https}

{{site.data.keyword.cloud}} 提供了两种方法来通过 HTTPS 保护 CDN - 通配符证书和域验证 (DV) SAN 证书。配置 CDN 时，通过选择 **HTTPS 端口**可以配置这两个 HTTPS 选项。缺省端口为 443，或者可以选择其他端口号来路由 HTTPS 流量。允许使用的端口号的列表可以在[常见问题](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)中找到。

要决定对于 HTTPS 是使用**通配符证书**还是 **SAN 证书**，请回答以下问题：是要通过 CDN CNAME 还是通过 CDN 域名提供 HTTPS 流量？如果要通过 CNAME 提供 HTTPS 流量，请选择**通配符证书**。如果要通过 CDN 域名提供 HTTPS 流量，请选择 **SAN 证书**。

## 通配符证书支持
{: #wildcard-certificate-support}

**注**：目前不支持使用通配符证书的新 CDN 映射。

通配符证书是将 Web 内容安全地交付给最终用户的最简单方法。**必须**将完整 CDN CNAME（包括通配符证书后缀）用作服务入口点（例如，`https://example.cdnedge.bluemix.net`），才能使用通配符证书。

IBM Cloud CDN 使用的通配符证书为 `*.cdnedge.bluemix.net`。CNAME（无论是为您创建的还是由您提供的，并以后缀 `*.cdnedge.bluemix.net` 结尾）会添加到 CDN 边缘服务器上维护的通配符证书中。因此，CNAME 成为最终用户要将 HTTPS 用于 CDN 的唯一方法。

![HTTP 和通配符的图](images/state-diagram-wildcard.png)

## 主题备用名称 (SAN) 证书支持
{: #san-certificate-suport}

主题备用名称 (SAN) 证书是一种数字 SSL 证书，允许多个域或主机名通过单个证书进行保护。

通过将 SAN 证书用于 HTTPS，主 CDN 主机名会添加到已由认证中心签发的证书中。这允许用户能够安全地通过主机名（而不是 CNAME）访问服务；例如，`https://www.example.com`。

使用 HTTPS SAN 证书订购 CDN 时，将经历请求证书和创建域控制验证 (DCV) 的过程。DCV 是认证中心用于确定您是否有权访问和控制域的过程。要完成此步骤，您必须执行相应操作。建立控制后，证书会部署到全球各地的 CDN 边缘服务器。成功部署证书后，系统会自动处理证书更新。有关此功能的更多信息可在[功能描述](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support)中找到。在[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation)页面上更详细地说明了域控制验证方法。

CDN 达到 RUNNING 状态后，必须在 DNS 中保留 CDN 主机名 CNAME 记录。如果除去了该 CNAME 记录，那么可能会在 3 天内从 SAN 证书中除去该 CDN 主机名。如果发生这种情况，那么不会再通过该 CDN 主机名来提供 HTTPS 流量。
{:note}

![使用 SAN 证书的 HTTPS 的图](images/state-diagram-san.png)
