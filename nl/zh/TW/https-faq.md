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

# HTTPS 常見問題
{: #faq-for-https}

## 對於我的 CDN，使用「萬用字元」憑證的 HTTPS 與使用 SAN 憑證的 HTTPS 有何差異？
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

使用「萬用字元」憑證時，所有客戶都會使用供應商之 CDN 網路上所部署的相同憑證。必須使用 CNAME（包括 IBM 字尾 `.cdnedge.bluemix.net`）來存取服務。例如，`https://www.example-cname.cdnedge.bluemix.net`

如果是 SAN 憑證，多個客戶網域都會藉由將其網域名稱新增至 SAN 項目，來共用單一 SAN 憑證。然後，可以使用主機名稱來存取服務，例如 `https://www.example.com`

## 如何使用重新導向來完成「網域驗證」？
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

這視您的伺服器而定。您可以在[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect)頁面上，找到完成 Apache 及 Nginx 伺服器的「網域驗證」程序。

## 「網域驗證」需要多長的時間？
{: #how-long-does-domain-validation-take}
{:faq}

「網域驗證」通常需要 2 至 4 小時，但會根據針對驗證所選擇的方法而不同。使用 CNAME 驗證的 DV 最為快速，通常會在一個小時內完成。處理盤查之後，使用「標準」及「重新導向」方法的 DV 通常需要 ~4 小時。

## 對於使用 DV SAN 憑證的 CDN，建立並啟用 HTTPS 需要多長的時間？
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

從起始要求到執行中，啟用 HTTPS 的一般要求平均需要 3 - 9 小時。

## 刪除使用 DV SAN 憑證的 CDN 需要多長的時間？
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

刪除您的 CDN 需要從所有邊緣伺服器上的憑證中移除您的網域。此程序最多需要 8 小時才能完成。

## 如果我的 CDN 使用 DV SAN 憑證，是否有任何其他相關成本？
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

否。相較於使用「萬用字元憑證」的 HTTP 或 HTTPS，DV SAN 憑證配置可供您免費使用。

## 使用具有「萬用字元」的 HTTPS 所建立的 CDN 可以更新為使用 DV SAN 憑證嗎？
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

否，無法將萬用字元對映變更為 SAN 憑證。

## 何謂「憑證管理中心」？
{: #what-is-a-certificate-authority}
{:faq}

「憑證管理中心 (CA)」是發出「數位憑證」的實體。

## {{site.data.keyword.cloud}} CDN 服務使用哪一個 CA 來發出 DV SAN 憑證？
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

IBM Cloud CDN 服務使用「LetsEncrypt 憑證管理中心」。

## IBM Cloud CDN 支援哪些 SSL 憑證？
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

支援的 SSL 憑證為「萬用字元」憑證及「網域驗證 (DV) 主體替代名稱 (SAN)」憑證。SAN 憑證是在多位客戶之間共用。IBM Cloud CDN 不支援上傳自訂憑證。

## 我收到一封電子郵件，要求我處理與我的 CDN 相關的「網域驗證」盤查。我現在該怎麼做？
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

「網域驗證」可以使用下列三種方式的其中一種來處理：CNAME、「標準」或「直接」。

如需如何處理上述任何問題的詳細資料，請參閱[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation)文件。

## 如果我未處理我的 CDN 網域驗證盤查，會發生什麼情況？
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

如果對映的狀態處於 DOMAIN_VALIDATION_PENDING 狀態超過 48 小時，則會取消建立對映，而且對映的狀態會是 CREATE_ERROR。在此狀態下，您可以選擇「重試」建立或刪除對映。

## 驗證我的 CDN 網域時是否需要「萬用字元」憑證？
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

否，但您只能使用 CNAME 來擷取原點中的內容。`https://www.example-cname.cdnedge.bluemix.net`

## 我收到的電子郵件指出我的網域未指向 IBM CDN CNAME。我現在該怎麼做？
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

此電子郵件表示您的 CDN 未被使用。若要使用 CDN 並使用憑證讓網域作用，您必須在 DNS 提供者系統中設定所列出的 CNAME DNS 記錄。
如果您在 7 天內完成此動作，則會還原 CDN 的 HTTP 和 HTTPS 資料流量，而且 CDN 會移至 RUNNING 狀態。如果在 7 天後仍未使用 CDN，則必須永久停用 CDN 網域的 HTTPS，以防止未使用的網域封鎖將新的 CDN 網域要求新增至「共用 SAN」憑證。藉由新增網域的 CNAME 記錄，稍後仍然可以還原透過 CDN 的 HTTP 資料流量存取。
如需如何處理此狀況的詳細資料，請參閱[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname)文件。

## 如果我的 CDN 使用 SAN 憑證類型，我仍然可以使用 CNAME 來存取我的服務嗎？
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

否。對於 SAN 憑證，您只能使用自訂網域來存取原點中的內容。

## 我的所有 IBM Cloud CDN 網域都會新增至一個憑證嗎？
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

不必然。Akamai 會處理憑證選取，以確保憑證處於最有效狀態。網域會按比例新增至不同的憑證，因此我們無法保證您的所有網域都將位於相同的憑證上。

## 當我執行 `dig`，或在 CDN 處於`要求憑證`、`網域驗證擱置`或`部署憑證`狀態時嘗試存取內容，為什麼會看到「萬用字元」？
{: #why-do-i-see-wildcard}
{:faq}

在 DV SAN 憑證要求處理程序期間，會暫時將 CDN 的 DNS 記錄鏈鏈結至萬用字元憑證。在此處理程序完成之前，會透過此萬用字元憑證暫時提供內容。在要求處理程序完成之後，會將 DNS 記錄鏈更新為鏈結至 CDN 的 DV SAN 憑證。
