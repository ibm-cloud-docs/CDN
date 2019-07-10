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

# 關於 HTTPS
{: #about-https}

{{site.data.keyword.cloud}} 提供兩種使用 HTTPS 來保護 CDN 的方式：「萬用字元憑證」及「網域驗證 (DV) SAN 憑證」。在配置 CDN 時選取 **HTTPS 埠**，即可配置這兩個 HTTPS 選項。預設「HTTPS 埠」為 443，或者您可以選擇不同的埠號來遞送 HTTPS 資料流量。您可以在[常見問題](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)中，找到容許的埠號清單。

若要決定針對 HTTPS 使用**萬用字元憑證**還是 **SAN 憑證**，請回答這個問題：您想要處理來自 CDN CNAME 還是 CDN 網域名稱的 HTTPS 資料流量？如果您想要處理來自 CNAME 的 HTTPS 資料流量，請選取**萬用字元憑證**。如果您想要處理來自 CDN 網域名稱的 HTTPS 資料流量，請選取 **SAN 憑證**。

## 萬用字元憑證支援
{: #wildcard-certificate-support}

**附註**：
目前不支援具有「萬用字元憑證」的新 CDN 對映。

「萬用字元」憑證是將 Web 內容安全地遞送給一般使用者的最簡單方式。完整 CDN CNAME（包括「萬用字元」憑證字尾）**必須**用來作為服務進入點（例如，`https://example.cdnedge.bluemix.net`），才能使用「萬用字元」憑證。

IBM Cloud CDN 使用「萬用字元」憑證 `*.cdnedge.bluemix.net`。CNAME（不論是為您建立還是由您提供，且結尾為 `*.cdnedge.bluemix.net` 字尾）會新增至 CDN 邊緣伺服器上所維護的萬用字元憑證。因此，CNAME 會變成一般使用者針對 CDN 使用 HTTPS 的唯一方式。

![HTTP 及萬用字元的圖表](images/state-diagram-wildcard.png)

## 主體替代名稱 (SAN) 憑證支援
{: #san-certificate-suport}

「主體替代名稱 (SAN) 憑證」是容許透過單一憑證來保護多個網域或主機名稱的數位 SSL 憑證。

使用 SAN 憑證進行 HTTPS 時，您的主要「CDN 主機名稱」會新增至由「憑證管理中心」發出的憑證。這可讓使用者透過主機名稱（而非 CNAME）安全地存取服務；例如，`https://www.example.com`。

使用 HTTPS SAN 憑證下 CDN 訂單時，會完成要求憑證以及建立「網域控制驗證 (DCV)」的處理程序。DCV 是「憑證管理中心」用來建立您有權存取及控制網域的處理程序。需要您採取動作，才能完成此步驟。建立控制權之後，會將憑證部署至全球的 CDN 邊緣伺服器。順利部署憑證之後，即會自動處理憑證的更新。您可以在[特性說明](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support)中，找到此特性的相關資訊。在[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation)頁面上，會更詳細地說明「網域控制驗證」方法。

一旦 CDN 達到 RUNNING 狀態，您便必須在 DNS 中保留 CDN 主機名稱 CNAME 記錄。如果移除了 CNAME 記錄，則可能在 3 天內從 SAN 憑證移除 CDN 主機名稱。如果發生此情況，便不會再用該 CDN 主機名稱處理 HTTPS 資料流量。
{:note}

![具有 SAN 憑證之 HTTPS 的圖表](images/state-diagram-san.png)
