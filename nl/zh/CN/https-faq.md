---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-05"

keywords: faq, https, wildcard, certificate, san certificate, domain validation, redirect, domains, challenge

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# HTTPS 常见问题
{: #faq-for-https}

## 对于我的 CDN，使用通配符证书的 HTTPS 和使用 SAN 证书的 HTTPS 有何不同？
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

使用通配符证书时，所有客户都使用在供应商 CDN 网络上部署的同一证书。必须使用 CNAME（包括 IBM 后缀 `.cdnedge.bluemix.net`）来访问服务。例如，`https://www.example-cname.cdnedge.bluemix.net`

对于 SAN 证书，多个客户域通过将其域名添加到 SAN 条目来共享单个 SAN 证书。然后，可以使用主机名来访问服务，例如 `https://www.example.com`

## 如何完成通过重定向进行的域验证？
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

这取决于您的服务器。完成 Apache 和 Nginx 服务器的域验证的过程可在[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect)页面上找到。

## 域验证需要多长时间？
{: #how-long-does-domain-validation-take}
{:faq}

域验证通常需要 2 到 4 小时，但是具体因选择用于验证的方法而异。使用 CNAME 的 DV 验证速度最快，通常可在一小时内完成。处理质询后，使用“标准”和“重定向”方法的 DV 通常需要约 4 小时。

## 使用 DV SAN 证书为 CDN 创建并启用 HTTPS 需要多长时间？
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

启用 HTTPS 的正常请求平均需要 3 到 9 小时（从初始请求到运行）。

## 删除使用 DV SAN 证书的 CDN 需要多长时间？
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

删除 CDN 需要从所有边缘服务器上的证书中除去域。此过程可能最长需要 8 小时才能完成。

## 是否有与将 DV SAN 证书用于 CDN 关联的其他任何成本？
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

没有。相对于使用通配符证书的 HTTP 或 HTTPS，提供 DV SAN 证书配置不会额外收费。

## 通过使用通配符的 HTTPS 创建的 CDN 可以更新为使用 DV SAN 证书吗？
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

不能，无法将通配符映射更改为 SAN 证书。

## 什么是认证中心？
{: #what-is-a-certificate-authority}
{:faq}

认证中心 (CA) 是签发数字证书的实体。

## {{site.data.keyword.cloud}} CDN 服务使用哪个 CA 签发 DV SAN 证书？
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

IBM Cloud CDN 服务使用 LetsEncrypt 认证中心。

## IBM Cloud CDN 支持哪些 SSL 证书？
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

支持的 SSL 证书为通配符证书和域验证 (DV) 主题备用名称 (SAN) 证书。SAN 证书在多个客户之间共享。IBM Cloud CDN 不支持上传定制证书。

## 我收到一封电子邮件，要求我处理与 CDN 相关的域验证质询。我现在该怎么做？
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

可以通过以下三种方法之一来处理域验证：CNAME、标准或重定向。

有关如何处理其中任一种方法的详细信息，请参阅[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation)文档。

## 如果不处理 CDN 的域验证质询，会发生什么情况？
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

如果映射处于 DOMAIN_VALIDATION_PENDING 状态超过 48 小时，将取消映射创建，并且映射的状态将为 CREATE_ERROR。在此状态下，可以选择“重试创建”或删除映射。

## 通配符证书需要验证 CDN 域吗？
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

不需要，但您只能使用 CNAME 从源中检索内容。`https://www.example-cname.cdnedge.bluemix.net`

## 我收到一封电子邮件，表明我的域名未指向 IBM CDN CNAME。我现在该怎么做？
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

此电子邮件表示您的 CDN 未被使用。要使用 CDN 并激活证书中的域，您必须在 DNS 提供者系统中设置列出的 CNAME DNS 记录。
如果您在 7 天内完成此操作，那么将复原 CDN 的 HTTP 和 HTTPS 流量，并且 CDN 将进入 RUNNING 状态。如果 CDN 在 7 天后仍未使用，我们必须为您的 CDN 域永久禁用 HTTPS，以防止未使用的域阻止将新的 CDN 域请求添加到共享 SAN 证书。通过为您的域添加 CNAME 记录，以后仍可以复原通过 CDN 进行的 HTTP 流量访问。
有关如何处理此情况的详细信息，请参阅[完成 HTTPS 的域控制验证](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname)文档。

## 如果我的 CDN 使用的是 SAN 证书类型，那么仍可以使用 CNAME 来访问服务吗？
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

不能。对于 SAN 证书，您只能使用定制域从源访问内容。

## 我的所有 IBM Cloud CDN 域都会添加到一个证书中吗？
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

不一定。证书选择由 Akamai 处理，以便确保证书处于最高效状态。域将按均匀比例添加到不同证书中，因此我们无法保证您的所有域都位于同一证书上。

## 当我执行 `dig` 时，或者当我在 CDN 处于`正在请求证书`、`域验证暂挂`或`正在部署证书`状态的情况下尝试访问内容时，为什么会看到“通配符”？
{: #why-do-i-see-wildcard}
{:faq}

在请求 DV SAN 证书过程中，CDN 的 DNS 记录链将暂时链接到通配符证书。在此过程完成之前，将通过此通配符证书来暂时提供内容。请求过程完成后，DNS 记录链将更新为链接到 CDN 的 DV SAN 证书。
