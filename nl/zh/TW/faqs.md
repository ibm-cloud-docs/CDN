---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 常見問題 (FAQ)

## 何謂 Content Delivery Network (CDN)？

Content Delivery Network (CDN) 是 Edge Server 的集合，分佈在國家/地區或全世界各地。其 Web 內容是由 Edge Server 所提供，而 Edge Server 位在最接近要求內容之客戶的地理區域中。此技術可讓使用者接收內容，而且其延遲會低於透過遞送某個集中化位置中的內容所產生的延遲。它可針對您的客戶提供較佳的整體經驗。

## Content Delivery Network (CDN) 如何運作？

CDN 透過將 Web 內容快取到全球各地的 Edge Server 來達到其目的。使用者要求 Web 內容時，會將內容要求遞送至地理上最接近該使用者的 Edge Server。透過減少內容必須行進的距離，CDN 可提供最佳化產量、最小化延遲，並提高效能。 

## IBM Cloud Content Delivery Network 服務帳戶如何建立？

如果您在瀏覽「供應商選項」功能表之後按一下**選取**，則會在 CDN 訂購處理程序期間建立您的帳戶。

## CDN 處於 CNAME_CONFIGURATION 狀態時我該怎麼做？

若為以 HTTP 為基礎的 CDN，請更新 DNS 記錄，讓網站指向與新 CDN 對映相關聯的 `CNAME`。若為以 HTTPS 為基礎的 CDN，**不需要**進行這項 CDN 更新。

**附註**：最多可能需要 15-30 分鐘，更新才會作用。請與 DNS 提供者聯繫，以取得精確的預估時間。

## 向我收費的內容為何？

我們只會針對每個 IBM Cloud Content Delivery Network 實例的頻寬用量向您收費。如果 CDN 未使用任何頻寬，則不會引發任何費用。頻寬價格取決於 Edge Server 的地區位置。您可以在本服務的[開始使用](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd)區段看到各地理區域的頻寬定價。

## 何時向我收費？

IBM Cloud Content Delivery Network 計費是根據「{{site.data.keyword.BluSoftlayer_notm}} 帳戶」中所建立的計費期間進行。

## 如何檢視度量值及使用情形？

您可以在**概觀**頁面上檢視度量值及使用情形，從 **Content Delivery Networks** 頁面中選取 CDN 即可到達此頁面。**附註**：在您建立 CDN 之後，最多可能需要 48 個小時，才會顯示度量值。

## 我建立了 CDN 而且有資料流量通過 CDN。為什麼我的度量值未出現？

建立 CDN 之後，需要 48 小時度量值才會出現。

## 我最少可以檢視度量值幾天？有上限嗎？

您可以使用 IBM Cloud Content Delivery Network 與 Akamai 檢視的度量值有天數的上限和下限。最少可以收集度量值 7 天。最多可以檢視度量值 90 天。如果是使用 API，則建議使用 89 天作為上限，以說明任何時區差異。

## 為什麼當總命中數為零時，命中率不是零？
命中率代表從 Edge Server 快取遞送內容，而不是從原點伺服器遞送內容的時間百分比。計算方式如下：

```
((Edge hits - Ingress hits)/Edge hits) * 100

其中：
Edge hits 定義為「從一般使用者對 Edge Server 的所有命中」
Ingress hits 定義為「原點或進入的命中是針對從您的原點到 Akamai Edge Server 的資料流量」
```
 
因為命中率的計算是在帳戶層次，而不是針對 CDN，所以在您帳戶中的所有 CDN 命中率都會相同。這一點也解釋了為何當特定 CDN 的邊緣命中數為零時，命中率可能不是零。

## 如果我在 CDN 的「溢位」功能表中選取 `delete`，這會刪除我的帳戶嗎？

不會；它只會刪除該 CDN。您的帳戶仍然存在，而且您可以建立其他 CDN。

## 快取是使用推送還是取回？

「內容快取」是使用_原點取回_ 模型進行。「原點取回」是 Edge Server 用來從「原點伺服器」「取回」資料的方法，而不是用來將內容手動上傳至 Edge Server。 

## 「存活時間」有最大值嗎？最小值？

「存活時間」的最大值是 2,147,483,647 秒，約略等於 68 年！最小值是 30 秒。

## 「原點」及 TTL 項目數是否有所限制？

是的，合併限制為每個 CDN 有 75 個項目。

## 我可以擁有的 CDN 數目是否有所限制？

是的，限制是每個帳戶有 10 個 CDN。

## 是否有建議用於 CDN 服務配置的瀏覽器？

是，建議的瀏覽器是 Firefox 及 Chrome。建議您使用最新版來搭配 IBM Cloud Content Delivery Network。

## 為何要在建立 CDN 時提供路徑？

如果您在建立 CDN 時提供路徑，則可讓您隔離可透過 CDN 提供的檔案與特定「原點伺服器」。

## 如何知道 CDN 正常運作？
請將 "origin.cdntesting.net/assets/ibm_3d.gif" 取代為您原點伺服器上的個別檔案路徑，以執行下列 'curl' 指令： 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
如果指令的輸出符合下列格式，則 CDN 會如預期運作：
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## 我的 CDN 處於「錯誤狀態」。我現在該怎麼做？
請參閱[取得說明及支援](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp)頁面，或在[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 中開立問題單。

## 如果我未提供 CNAME，則在哪裏可以找到它？ 
按一下您的 CDN 以在入口網站存取**概觀**頁面。您會在右上角看到包含 `CName` 資訊的**詳細資料**區段。

## 同時可作用的「清除」要求數目是否有所限制？
是。一次只能有 5 個作用中清除要求。

## 對於給定檔案路徑的「我的清除要求」正在進行中。我可以針對相同的檔案路徑提交新的要求嗎？
不行。一次只能有一個對於給定檔案路徑的作用中「清除」要求。

## IBM Cloud Content Delivery Network 服務支援網際網路通訊協定第 6 版 (IPv6) 嗎？它如何運作？
Akamai 的邊緣伺服器支援 IPv6（或雙重堆疊支援）。其設計是要協助只有 IPv4 原點的客戶接受來自 IPv6 用戶端的連線、在邊緣從 IPv6 轉換為 IPv4，然後繼續送往 IPv4 的原點。

**附註：**不支援使用 IPv6 位址作為原點伺服器位址來建立 IBM Cloud CDN。

## 主機名稱的規則為何？
`Hostname` 輸入字串**必須**：
  * 由英數字元組成
  * 少於 254 個字元
  * 以有效的頂層網域名稱為結尾
  * **不得**以 'cdnedge.bluemix.net` 為結尾（該結尾用於 CNAMES 且已保留）

如需詳細資料，請參閱 RFC 1035 第 2.3.4 節。

## 自訂 CNAME 命名慣例為何？
`CNAME` 輸入字串必須遵循下列規則：
  * **必須**是唯一的（它不能由任何其他 IBM Cloud CDN 使用中）
  * 少於 64 個英數字元
  * **不允許**特殊字元 `! @ # $ % ^ & *`
  * **不允許**底線 `_`
  * **不允許**句點 `.`
  * 允許橫線 `-`，但 CNAME 不得以橫線為開頭或結尾

## 儲存區名稱的規則為何？
儲存區名稱：
  * 必須至少有 1 個字元
  * 長度不得超過 199 個字元
  * 可以包含字母（大寫和小寫字母都允許）、數字和連字號
  * **不得**格式化為 IP 位址（例如 127.0.0.1）
  * **不得**以句點 (.) 為開頭
  * 可以以句點 (.) 為結尾
  * 在標籤之間只允許一個句點 (.)（例如，不允許 my..ibmcloud.bucket）。 

**附註**：雖然大寫字母和句點可以通過驗證，我們仍建議您一律使用遵循 DNS 的儲存區名稱。

## 原點路徑字串的規則為何？
建立 CDN 時路徑是選用性的。不過，如果提供，路徑**必須**：
  * 長度少於 1000 個字元
  * 以 '/' 為開頭

## 對於**新增原點**指令，路徑字串有任何規則必須遵循嗎？
對於**新增原點**，路徑是必要項目。此外，如果 CDN 建立時有路徑，則**新增原點**的路徑開頭必須以 CDN 路徑作為字首。例如，如果 CDN 路徑指定為 `/storage`，則若要呼叫**新增原點**並使用稱為 `/examplePath` 的路徑，所提供的路徑將為 `/storage/examplePath`'。

## 使用此 CDN 服務建立 Object Storage 原點類型時我應該提供什麼格式的副檔名？

副檔名應該以逗點區隔。例如，清單 "txt, jpg, xml" 便是有效的清單。任何特定副檔名長度不得超過 10 個字元。

## 允許 Akamai 使用的 HTTP 及 HTTPS 埠號有任何限制嗎？
是。針對 Akamai 供應商，只允許下列埠號：72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 及 45002。

## 存取 CDN 或原點路徑下的資料時應該使用什麼 URL？ 
CDN 對映的路徑或是原點的路徑會被視為目錄。因此，嘗試存取原點路徑的使用者應該以目錄的方式存取它（搭配斜線）。例如，如果 CDN `www.example.com`
建立時使用了包含 `/images` 目錄的路徑，則要呼叫到它的 URL 應該是 `www.example.com/images/`。

若省略斜線，例如使用 `www.example.com/images`，將導致**找不到頁面**的錯誤。

## 對於 HTTPS，為什麼我無法透過 curl 指令或瀏覽器使用主機名稱來連接？

目前只透過萬用字元憑證支援 HTTPS。由於此項限制，必須使用 CNAME 建立連線；嘗試使用主機名稱來連接將導致失敗。

## 在您的瀏覽器上針對支援的通訊協定載入 CNAME 或主機名稱時，預期的行為為何？

|瀏覽器 URL| CDN 僅搭配 HTTP 通訊協定| CDN 僅搭配 HTTPS 通訊協定| CDN 同時搭配 HTTP 和 HTTPS 通訊協定|
|-------|-----|-----|-----|
|http://hostname| 成功載入| 301 永久地移動| 301 永久地移動|
|https://hostname | 拒絕存取| 重新導向至 IBM Cloud 網頁| 重新導向至 IBM Cloud 網頁| 
|http://cname| 301 永久地移動| 拒絕存取| 成功載入| 
|https://cname| 重新導向至 IBM Cloud 網頁| 成功載入| 成功載入|

## 如何針對 IBM Cloud Object Storage (COS) 設定我的 Content Delivery Network？
[這裡提供](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn)為 IBM Cloud Object Storage 建立 Content Delivery Network 的指導教學。

## 選擇 IBM Cloud Object Storage (COS) 作為原點時，我的主機名稱為何無法在瀏覽器上載入？

當您的 IBM Cloud CDN 配置為使用 IBM COS 作為物件儲存空間時，將無法直接存取網站。您必須在瀏覽器的位址列指定完整要求路徑（例如 `www.example.com/index.html`）。此行為是由 IBM COS 中的索引文件限制所導致。

## 可以透過 Akamai CDN 遞送的最大檔案大小為何？

試圖擷取或遞送大於 1.8GB 的檔案時，會針對預設效能配置收到 `403 Access Forbidden` 的回應。如果已啟用「大型檔案最佳化」，檔案下載最大可達到 320GB。

## 我收到通知，我的原點憑證即將到期。我現在該怎麼做？

請遵循來自 Akamai 的[這篇文章](https://community.akamai.com/docs/DOC-7708)所概述的步驟。

## 何謂位元組範圍要求，它與 Akamai CDN 如何搭配運作？

位元組範圍要求用來從原點伺服器擷取局部內容。範圍 HTTP 要求標頭指出伺服器應該傳回內容的哪個部分。一個範圍標頭可以一次要求數個部分，伺服器可以用多部分回應來傳回這些範圍。如果伺服器傳回範圍，它會回應 206（局部內容）狀態。

使用 IBM Cloud CDN 與 Akamai 傳送位元組範圍要求時，使用者可能針對第一個要求會收到 200 (OK) 回應碼，而後續所有要求則都收到 206 回應碼。這是因為 Akamai Edge Server 會以壓縮格式向原點要求內容。因此，當 Edge Server 在快取中沒有某個物件，也沒有關於物件內容長度的任何資訊時，它會向原點前進，且會要求整個物件。在此情況下，原點會提供沒有內容長度標頭的物件給 Akamai，使用者則會得到整個物件，即使是位元組範圍要求。因此是 200 狀態碼。在後續的要求中，Edge Server 的快取中已經有物件，將會提供 206 狀態碼。

有一種確保 206 回應的方法（即使是第一個位元組範圍要求），就是在您的原點伺服器上停用 `Transfer-Encoding: chunked`。

## IBM CDN 解決方案搭配 Akamai 包含了什麼安全等級？

藉由使用分散式 Akamai 平台，您會得到無與倫比的擴充和備援，並在超過 130 個國家/地區有超過 240,000 台伺服器。Akamai 平台在您的基礎架構與使用者之間，它扮演了資料流量突然增強的第一道防禦。Akamai Intelligent Platform 也是反向 Proxy，它只會在埠 80 和 443 接聽並回應要求，這表示其他埠上的資料流量會在邊緣就捨棄，不會轉遞到您的基礎架構。
