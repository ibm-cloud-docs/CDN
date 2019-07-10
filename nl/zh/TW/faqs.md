---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

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
{:DomainName: data-hd-keyref="DomainName"}

# 常見問題 (FAQ)
{: #faqs}

## 何謂 Content Delivery Network (CDN)？
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Content Delivery Network (CDN) 是邊緣伺服器的集合，分佈在國家/地區或全世界各地。其 Web 內容是由邊緣伺服器所提供，而邊緣伺服器位在最接近要求內容之客戶的地理區域中。此技術可讓使用者接收內容，而且其延遲會低於透過遞送某個集中化位置中的內容所產生的延遲。它可針對您的客戶提供較佳的整體經驗。

## Content Delivery Network (CDN) 如何運作？
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

CDN 透過將 Web 內容快取到全球各地的邊緣伺服器來達到其目的。使用者要求 Web 內容時，會將內容要求遞送至地理上最接近該使用者的邊緣伺服器。透過減少內容必須行進的距離，CDN 可提供最佳化產量、最小化延遲，並提高效能。

## IBM Cloud Content Delivery Network 服務帳戶如何建立？
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

在 CDN 訂購處理程序期間會建立您的帳戶。如果您從舊式入口網站建立 CDN，當您按一下**訂購 CDN** 按鈕時，在**網路 -> CDN** 頁面下，會建立您的帳戶。如果您從 {{site.data.keyword.cloud}} 入口網站建立 CDN，當您按一下**建立**按鈕時，在**型錄 -> 網路 -> Content Delivery Network** 頁面下，會建立您的帳戶。

## CDN 處於 CNAME 狀態時我該怎麼做？
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

若為以 HTTP 和 SAN 憑證為基礎的 HTTPS CDN，請更新 DNS 記錄，讓網站指向與新 CDN 對映相關聯的 `CNAME`。若為以萬用字元憑證為基礎的 HTTPS CDN，**不需要**進行這項 CDN 更新。

**附註**：最多可能需要 15-30 分鐘，更新才會作用。請與 DNS 提供者聯繫，以取得精確的預估時間。

## 如何在 DNS 中新增 CDN 網域的 CNAME 記錄？
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

在 CDN 網域的 DNS 配置頁面中，您可以建立一個 CNAME 記錄，其「CDN 網域名稱」為「主機」，而您用來配置 CDN 的 IBMCNAME 為 CNAME。IBM CNAME 的結尾為 `cdnedge.bluemix.net`。

一般 CNAME 記錄在 DNS 配置頁面上將類似如下：

| **資源類型** | **主機** | **指向 (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
|CNAME| www.example.com | example.cdnedge.bluemix.net | 15 分鐘|


## CDN 的計費內容為何
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

我們只會針對每個 IBM Cloud Content Delivery Network 實例的頻寬用量向您收費。如果 CDN 未使用任何頻寬，則不會引發任何費用。頻寬價格取決於邊緣伺服器的地區位置。您可以在本服務的[定價文件](/docs/infrastructure/CDN?topic=CDN-pricing)中，看到各地理區域的頻寬定價。

## CDN 的計費時間為何？
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

IBM Cloud Content Delivery Network 計費是根據「{{site.data.keyword.BluSoftlayer_notm}} 帳戶」中所建立的計費期間進行。

## 如果我在 CDN 的「溢位」功能表中選取 `delete`，這會刪除我的帳戶嗎？
{: faq}

不會；它只會刪除該 CDN。您的帳戶仍然存在，而且您可以建立其他 CDN。

## 內容快取是使用推送還是取回？
{: #does-content-caching-use-push-or-pull}
{: faq}

「內容快取」是使用_原點取回_ 模型進行。「原點取回」是邊緣伺服器用來從「原點伺服器」「取回」資料的方法，而不是用來將內容手動上傳至邊緣伺服器。

## 是否有建議用於 CDN 服務配置的瀏覽器？
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

是，建議的瀏覽器是 Firefox 及 Chrome。建議您使用最新版來搭配 IBM Cloud Content Delivery Network。

## 為何要在建立 CDN 時提供路徑？
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

如果您在建立 CDN 時提供路徑，則可讓您隔離可透過 CDN 提供的檔案與特定「原點伺服器」。

## 我的 CDN 處於「錯誤狀態」。我現在該怎麼做？
{: faq}

請參閱[疑難排解](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting)或[取得協助及支援](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp)頁面，或在[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 中開立問題單。

## 如果我未提供 CDN 的 CNAME，則在哪裏可以找到它？
{: faq}

按一下您的 CDN 以在入口網站存取**概觀**頁面。您會在右上角看到包含 `CName` 資訊的**詳細資料**區段。

## 對於給定檔案路徑的「我的清除要求」正在進行中。我可以針對相同的檔案路徑提交新的要求嗎？

不行。一次只能有一個對於給定檔案路徑的作用中「清除」要求。

## IBM Cloud Content Delivery Network 服務支援網際網路通訊協定第 6 版 (IPv6) 嗎？它如何運作？
{: faq}

Akamai 的邊緣伺服器支援 IPv6（或雙重堆疊支援）。其設計是要協助只有 IPv4 原點的客戶接受來自 IPv6 用戶端的連線、在邊緣從 IPv6 轉換為 IPv4，然後繼續送往 IPv4 的原點。

**附註：**不支援使用 IPv6 位址作為原點伺服器位址來建立 IBM Cloud CDN。

## 允許 Akamai 使用的 HTTP 及 HTTPS 埠號有任何限制嗎？
{: faq}

是。針對 Akamai 供應商，只允許下列埠號：72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 及 45002。

## 存取 CDN 或原點路徑下的資料時應該使用什麼 URL？
{: faq}

CDN 對映的路徑或是原點的路徑會被視為目錄。因此，嘗試存取原點路徑的使用者應該以目錄的方式存取它（搭配斜線）。例如，如果 CDN `www.example.com`
建立時使用了包含 `/images` 目錄的路徑，則要呼叫到它的 URL 應該是 `www.example.com/images/`。

若省略斜線，例如使用 `www.example.com/images`，將導致**找不到頁面**的錯誤。

## 如何針對 IBM Cloud Object Storage (COS) 設定我的 Content Delivery Network？
{: faq}

[這裡的指導教學](/docs/tutorials?topic=solution-tutorials-static-files-cdn)是針對為 IBM Cloud Object Storage 建立 Content Delivery Network。

## 我收到通知，我的原點憑證即將到期。我現在該怎麼做？
{: faq}

請遵循來自 Akamai 的[這篇文章 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://community.akamai.com/docs/DOC-7708) 中所概述的步驟。

## IBM CDN 解決方案搭配 Akamai 包含了什麼安全等級？
{: faq}

藉由使用分散式 Akamai 平台，您會得到無與倫比的擴充和備援，並在超過 50 個國家/地區有數千台伺服器。Akamai 平台在您的基礎架構與使用者之間，它扮演了資料流量突然增強的第一道防禦。Akamai Intelligent Platform 也是反向 Proxy，它只會在埠 80 和 443 接聽並回應要求，這表示其他埠上的資料流量會在邊緣就捨棄，不會轉遞到您的基礎架構。

## Akamai CDN 會保留來自原點伺服器的 Cookie 嗎？
{: faq}

針對無法快取的內容，或未快取的任何內容，會保留來自原點的 Cookie。針對邊緣伺服器所快取的內容，則不會保留 Cookie。

## 如何使用 IBM Cloud 主控台授與其他使用者許可權來建立或管理 CDN？
{: faq}

帳戶的「管理員使用者」可以提供其他使用者許可權，來建立和管理 CDN。

從 IBM Cloud 主控台主頁面，遵循下列步驟以編輯許可權：
 * 選取**管理**標籤
 * 選取**存取 (IAM)**
 * 按一下左窗格中的**使用者**標籤
 * 按一下想要的**使用者**
 * 然後選取**標準基礎架構**標籤
 * 然後在**許可權**標籤下，展開**服務**種類
 * 選取**管理 CDN 帳戶**。
 * 按一下**儲存**按鈕

從舊主控台主頁面，遵循下列步驟以編輯許可權：
 * 選取**帳戶**標籤。
 * 選取**使用者 -> 使用者清單**。
 * 按一下想要的**使用者名稱**。
 * 然後選取**入口網站許可權**標籤。
 * 選取**服務**標籤。
 * 選取**管理 CDN 帳戶**。
 * 按一下**編輯入口網站許可權**按鈕。

## 在 https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai 頁面上，為何未顯示「建立」按鈕，或者該按鈕已停用？
{: faq}

如果您是帳戶的「管理員使用者」，則您必須升級帳戶，才能在此頁面上顯示或啟用「建立」按鈕。從 IBM Cloud 主控台頁面，以帳戶的「管理員使用者」身分遵循下列步驟：
 * 按一下網頁左上角的 `triple bar` 圖示，以開啟左導覽窗格
 * 選取**標準基礎架構**
 * 按一下**升級帳戶**按鈕，然後遵循指示

如果您是帳戶的次要「使用者」之一，則帳戶的「管理員使用者」必須給您`新增/升級服務`許可權，才能在此頁面上顯示或啟用「建立」按鈕。從 IBM Cloud 主控台頁面，帳戶的「管理員使用者」應遵循下列步驟以編輯您的許可權：
 * 選取**管理**標籤
 * 選取**存取 (IAM)**
 * 按一下左窗格中的**使用者**標籤
 * 按一下想要的**使用者**
 * 然後選取**標準基礎架構**標籤
 * 然後在**許可權**標籤下，展開**帳戶**種類
 * 選取**新增/升級服務**
 * 按一下**儲存**按鈕

## 使用 `protectionType` `ALLOW` 來配置「快速鏈結保護」之後，為何無法透過我的 CDN 呼叫到我的網頁？
{: faq}

舉例說明，假設您網站的一般使用者網域配置為您的 CDN 網域/主機名稱：`cdn.example.com`。當某人試圖直接從瀏覽器的導覽列進行導覽，以呼叫網頁時，瀏覽器一般不會在其 HTTP 要求中傳送 Referer 標頭。例如，當您以此方式直接導覽至 `https://cdn.example.com/` 時，您的 CDN 會認為該要求所包含的值不符合所指定的 `refererValues`。當 CDN 透過您的「快速鏈結保護」來評估適當的結果或回應時，會判定已發生不符合的情況。因此，您的 CDN 會拒絕存取，而非 'ALLOW'。

## 是否可以在 CDN 設定中使用物件儲存空間的專用端點？
{: faq}

否，CDN 只能連接至「公用」端點上的「物件儲存空間」。

## 是否可以在 CDN 服務中使用 Brotli 特性？
{: faq}

否，Akamai 的 CDN 服務不支援 Brotli 特性。

## 如何在未使用網域的情況下建立 CDN 端點？
{: faq}

您可以在未使用「網域」的情況下建立 CDN 端點，但僅限於**萬用字元 HTTPS** 類型的 CDN。在建立**萬用字元 HTTPS** 類型的 CDN 時，**CNAME** 用來作為 CDN 端點，並且使用 **CNAME** 來提供資料流量。
