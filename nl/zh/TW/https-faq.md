---

copyright:
  years: 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HTTPS 常見問題

## 使用「萬用字元」憑證的 HTTPS 與使用 SAN 憑證的 HTTPS 有何差異？

使用「萬用字元」憑證時，所有客戶都會使用供應商之 CDN 網路上所部署的相同憑證。必須使用 CNAME（包括 IBM 字尾 `.cdnedge.bluemix.net`）來存取服務。例如，`https://www.example-cname.cdnedge.bluemix.net`

如果是 SAN 憑證，多個客戶網域都會藉由將其網域名稱新增至 SAN 項目，來共用單一 SAN 憑證。然後，可以使用主機名稱來存取服務，例如 `https://www.example.com`

## 如何使用重新導向來完成「網域驗證」？

這視您的伺服器而定。您可以在[完成 HTTPS 的網域控制驗證](how-to-https.html#redirect-)頁面上，找到完成 Apache 及 Nginx 伺服器的「網域驗證」程序。

## 「網域驗證」需要多長的時間？

「網域驗證」通常需要 2 - 4 小時，但會根據針對驗證所選擇的方法而不同。使用 CNAME 驗證的 DV 最為快速，通常會在一個小時內完成。處理盤查之後，使用「標準」及「重新導向」方法的 DV 通常需要 ~4 小時。

## 使用 DV SAN 憑證的建立程序需要多長的時間？

從起始要求到執行中，啟用 HTTPS 的一般要求平均需要 3 - 9 小時。

## 刪除使用 DV SAN 憑證的 CDN 需要多久的時間？

刪除您的 CDN 需要從所有 Edge Server 上的憑證中移除您的網域。此程序最多需要 8 小時才能完成。

## 是否有其他與使用 DV SAN 憑證相關聯的成本？

否。相較於使用「萬用字元憑證」的 HTTP 或 HTTPS，DV SAN 憑證配置可供您免費使用。

## 使用具有「萬用字元」的 HTTPS 所建立的 CDN 可以更新為使用 DV SAN 憑證嗎？

否，目前無法將萬用字元對映變更為 SAN 憑證。

## 何謂「憑證管理中心」？

「憑證管理中心 (CA)」是發出「數位憑證」的實體。

## IBM Cloud CDN 服務使用哪一個 CA 來發出 DV SAN 憑證？

IBM Cloud CDN 服務使用「LetsEncrypt 憑證管理中心」。

## 支援哪些 SSL 憑證？

支援的 SSL 憑證為「萬用字元」憑證及「網域驗證 (DV) 主體替代名稱 (SAN)」憑證。SAN 憑證是在多位客戶之間共用。IBM Cloud CDN 不支援上傳自訂憑證。

## 我收到一封電子郵件，要求我處理「網域驗證」盤查。我現在該怎麼做？

「網域驗證」可以使用下列三種方式的其中一種來處理：CNAME、「標準」或「直接」。

如需如何處理上述任何問題的詳細資料，請參閱[完成 HTTPS 的網域控制驗證](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation)文件。

## 如果我未處理網域驗證的盤查，會發生什麼情況？

如果對映的狀態處於 DOMAIN_VALIDATION_PENDING 狀態超過 48 小時，則會取消建立對映，而且對映的狀態會是 CREATE_ERROR。在此狀態下，您可以選擇「重試」建立或刪除對映。

## 驗證網域時是否需要「萬用字元」憑證？

否，但您只能使用 CNAME 來擷取原點中的內容。`https://www.example-cname.cdnedge.bluemix.net`

## 如果我使用 SAN 憑證類型，則仍然可以使用 CNAME 來存取我的服務嗎？

否。對於 SAN 憑證，您只能使用自訂網域來存取原點中的內容。`https://www.example.com`

## 我的所有網域都會新增至一個憑證嗎？

不必然。Akamai 會處理憑證選取，以確保憑證處於最有效狀態。網域會按比例新增至不同的憑證，因此我們無法保證您的所有網域都將位於相同的憑證上。

## 當我執行 `dig`，或在 CDN 處於`要求憑證`、`網域驗證擱置`或`部署憑證`狀態時嘗試存取內容，為什麼會看到「萬用字元」？

在 DV SAN 憑證要求處理程序期間，會暫時將 CDN 的 DNS 記錄鏈鏈結至萬用字元憑證。在此處理程序完成之前，會透過此萬用字元憑證暫時提供內容。在要求處理程序完成之後，會將 DNS 記錄鏈更新為鏈結至 CDN 的 DV SAN 憑證。
