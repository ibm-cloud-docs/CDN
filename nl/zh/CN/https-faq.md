---

copyright:
  years: 2018
lastupdated: "2018-11-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{: faq: data-hd-content-type=‘faq’}

# HTTPS 常见问题 

## 对于我的 CDN，使用通配符证书的 HTTPS 和使用 SAN 证书的 HTTPS 有何不同？
{: faq}

使用通配符证书时，所有客户都使用在供应商 CDN 网络上部署的同一证书。必须使用 CNAME（包括 IBM 后缀 `.cdnedge.bluemix.net`）来访问服务。例如，`https://www.example-cname.cdnedge.bluemix.net`

对于 SAN 证书，多个客户域通过将其域名添加到 SAN 条目来共享单个 SAN 证书。然后，可以使用主机名来访问服务，例如 `https://www.example.com`

## 如何完成通过重定向进行的域验证？
{: faq}

这取决于您的服务器。完成 Apache 和 Nginx 服务器的域验证的过程可在[完成 HTTPS 的域控制验证](how-to-https.html#redirect)页面上找到。

## 域验证需要多长时间？
{: faq}

域验证通常需要 2 到 4 小时，但是会根据选择用于验证的方法而变化。使用 CNAME 的 DV 验证速度最快，通常可在一小时内完成。处理质询后，使用“标准”和“重定向”方法的 DV 通常需要约 4 小时。

## 使用 DV SAN 证书为我的 CDN 创建并启用 HTTPS 需要多长时间？
{: faq}

启用 HTTPS 的正常请求平均需要 3 到 9 小时（从初始请求到运行）。

## 删除使用 DV SAN 证书的 CDN 需要多长时间？
{: faq}

删除 CDN 需要从所有边缘服务器上的证书中除去域。此过程可能最长需要 8 小时才能完成。

## 是否有与将 DV SAN 证书用于 CDN 关联的其他任何成本？
{: faq}

没有。相对于使用通配符证书的 HTTP 或 HTTPS，提供 DV SAN 证书配置不会额外收费。

## 通过使用通配符的 HTTPS 创建的 CDN 可以更新为使用 DV SAN 证书吗？
{: faq}

不能，目前无法将通配符映射更改为 SAN 证书。

## 什么是认证中心？
{: faq}

认证中心 (CA) 是签发数字证书的实体。

## IBM Cloud CDN 服务使用哪个 CA 签发 DV SAN 证书？
{: faq}

IBM Cloud CDN 服务使用 LetsEncrypt 认证中心。

## IBM Cloud CDN 支持哪些 SSL 证书？
{: faq}

支持的 SSL 证书为通配符证书和域验证 (DV) 主题备用名称 (SAN) 证书。SAN 证书在多个客户之间共享。IBM Cloud CDN 不支持上传定制证书。

## 我收到一封电子邮件，要求我处理与 CDN 相关的域验证质询。我现在该怎么做？
{: faq}

可以通过以下三种方法之一来处理域验证：CNAME、标准或重定向。

有关如何处理其中任一种方法的详细信息，请参阅[完成 HTTPS 的域控制验证](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation)文档。

## 如果不处理 CDN 的域验证质询，会发生什么情况？
{: faq}

如果映射处于 DOMAIN_VALIDATION_PENDING 状态超过 48 小时，将取消映射创建，并且映射的状态将为 CREATE_ERROR。在此状态下，可以选择“重试创建”或删除映射。

## 通配符证书需要验证 CDN 域吗？
{: faq}

不需要，但您只能使用 CNAME 从源中检索内容。`https://www.example-cname.cdnedge.bluemix.net`

## 如果我的 CDN 使用的是 SAN 证书类型，那么仍可以使用 CNAME 来访问服务吗？
{: faq}

不能。对于 SAN 证书，您只能使用定制域从源访问内容。`https://www.example.com`

## 我的所有 IBM Cloud CDN 域都会添加到一个证书中吗？
{: faq}

不一定。证书选择由 Akamai 处理，以便确保证书处于最高效状态。域将按均匀比例添加到不同证书中，因此我们无法保证您的所有域都位于同一证书上。

## 当我执行 `dig` 时，或者当我在 CDN 处于`正在请求证书`、`域验证暂挂`或`正在部署证书`状态的情况下尝试访问内容时，为什么会看到“通配符”？
{: faq}

在请求 DV SAN 证书过程中，CDN 的 DNS 记录链将暂时链接到通配符证书。在此过程完成之前，将通过此通配符证书来暂时提供内容。请求过程完成后，DNS 记录链将更新为链接到 CDN 的 DV SAN 证书。
