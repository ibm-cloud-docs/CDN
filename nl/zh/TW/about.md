---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 關於 Content Delivery Network (CDN)

Content Delivery Network (CDN) 是 Edge Server 的集合，分佈在國家/地區或全世界各地。Web 內容是由 Edge Server 所提供，而 Edge Server 位在最接近要求內容之客戶的地理區域中。此技術可讓您的使用者更及時地收到內容，它也為您的客戶提供更好的整體經驗。

## CDN 如何運作？

CDN 透過將 Web 內容快取到全球各地的 Edge Server 來達到其目的。客戶要求 Web 內容時，會將內容要求遞送至地理上最接近該客戶的 Edge Server。透過減少內容必須行進的距離，CDN 可提供最佳化產量、最小化延遲，並提高效能。使用 IBM Cloud Content Delivery Network 搭配 Akamai，內容提供者可以從全球各地實現有效率的要求內容遞送，並且只需要最少的配置。

IBM Cloud Content Delivery Network 服務的主要特性包括：
  * 主伺服器原點支援
  * Object Storage 原點支援
  * 相異路徑之多原點支援
  * 以路徑為基礎的 CDN 對映
  * 清除快取內容
  * 存活時間 (TTL)
  * 具有圖形視圖的度量值
  * 萬用字元憑證的 HTTPS 支援
  * 主機標頭支援
  * 提供陳舊內容
  * 「忽略快取金鑰中的查詢引數」特性
  * 內容壓縮
  * 大型檔案最佳化
  * 隨選視訊

## 特性說明

### 主伺服器原點支援

IBM Cloud Content Delivery Network (CDN) 可以配置為從「主伺服器原點」提供內容，方法是提供原點主機名稱、通訊協定、埠號，以及選擇性的內容提供路徑。預設路徑是 '/\*'。通訊協定可以是 HTTP 及/或 HTTPS。Akamai 只支援特定埠號。如需支援的埠號/範圍，請參閱[常見問題](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。目前使用萬用字元憑證來支援 HTTPS，這表示只能透過以 `.cdnedge.bluemix.net` 結束的 CNAME 來存取服務。如需相關資訊，請參閱 [HTTPS 與萬用字元憑證](about.html#https-with-wildcard-certificate-)特性說明。

### Object Storage 原點支援

IBM Cloud CDN 可以配置為從 Object Storage 端點提供內容，方法是提供_端點_、_儲存區名稱_、_通訊協定_ 和_埠_。可以選擇性地指定副檔名清單，以便只允許快取具有那些副檔名的檔案。儲存區中的所有物件都需要設定匿名讀取或公用讀取權。這份關於[如何使用 CDN 設定 IBM Bluemix Object Storage](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) 的指導教學提供了更詳細的資訊。

### 相異路徑之多原點支援

在特定情況中，您可能會想要從不同的原點伺服器遞送特定內容。例如，您可能想要從不同的原點伺服器提供照片或視訊。IBM Cloud CDN 提供選項來設定具有多條路徑的多個原點伺服器。這樣能允許資料儲存的方式和位置具有彈性。為原點伺服器提供的路徑應該對於 CDN 而言是唯一的。原點伺服器本身不需要是唯一的。

### 以路徑為基礎的 CDN 對映

您的 IBM Cloud CDN 服務可以限制為原點伺服器上的特定目錄路徑，方法是在建立 CDN 時提供路徑。使用者只能夠存取該目錄路徑中的內容。例如，如果建立了 CDN www.example.com 且具有路徑 '/videos'，將只能透過 `www.example.com/videos/` 來存取它。

### 清除快取內容

IBM Cloud CDN 提供方便快速地移除（或「清除」）Edge Server 上之快取內容的功能。在目前，只允許針對檔案路徑進行清除。Akamai 的清除實作目前會在大約 5 秒內清除檔案。使用者介面也提供檢視清除歷程以及將特定清除路徑標記為最愛的功能。

### 存活時間 (TTL)

存活時間是指 Edge Server 將快取特定檔案或目錄路徑之內容的時間量（以秒為單位）。一開始建立 CDN 時，會為路徑 ('/\*') 建立廣域的存活時間 (TTL)，預設時間為 3600 秒。TTL 的最小值是 30 秒，最大值則是 2147483647 秒。針對每個項目，TTL 路徑應該為 CDN 獨有。如果多條路徑都符合給定的內容，最近配置的路徑相符項會適用於該內容。例如，假設有兩個 TTL，`/example/file` 先建立，存活時間值為 3000 秒，而 `/example/*` 較晚建立，值為 4000 秒。雖然 `/example/file` 較為明確，但 `/example/*` 最近才建立，因此 `/example/file` 的 TTL 將為 4000 秒。建立之後，可以編輯 TTL 項目的路徑及/或時間。也可以予以刪除。

### 具有圖形視圖的度量值

個別 CDN 的度量值提供於該 CDN 對映的客戶入口網站「概觀」標籤。有兩種度量值會從 CDN 的用量計算而來：將某時段度量值顯示為圖形的度量值，以及顯示為總計值的度量值。

對於將一段時間的變化顯示為圖形的度量值，您可以看到三個折線圖和一個圓餅圖。三個折線圖是：**頻寬**、**依對映的命中數**，以及**依類型的命中數**。它們會顯示在您指定時間範圍的過程中，每日活動的情形。**頻寬**和**依對映的命中數**的圖形是單一折線圖，而**依類型的命中數**的分解會針對所提供的每一種命中類型顯示一條折線。圓餅圖會以百分比為基準，顯示 CDN 對映頻寬的地區分解。

總計值所顯示的度量值，包括**頻寬用量** (GB)、對 CDN Edge Server 的**命中數總計**，以及**命中率**。命中率指出內容由 Edge Server 而非透過其原點提供的次數百分比。命中率目前顯示為所有 CDN 對映的函數，而不只是正在檢視的 CDN 對映。

依預設，總計值和圖形都會預設為顯示過去 30 天的度量值，但您可以透過[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 來變更這點。兩個種類都能夠顯示 7、15、30、60 或 90 天期間的度量值。

### 萬用字元憑證的 HTTPS 支援

萬用字元憑證是將 Web 內容安全地遞送給一般使用者的最經濟方式。此特性的啟用方法是在配置 CDN 或原點路徑時選取 `HTTPS 埠`。使用萬用字元憑證針對 HTTPS 啟用 CDN 對映之後，Edge server 會透過 HTTPS 聯絡原點伺服器。依預設，如果將原點伺服器指定為主機名稱，則 Edge Server 會使用原點主機名稱作為「伺服器名稱指示 (SNI)」標頭，以進行與原點伺服器的「傳輸層安全 (TLS)」協議。不過，如果將原點伺服器指定為 IP 位址，則將使用 CDN 的主機名稱作為 SNI 標頭。必須由受到認可的「憑證管理中心 (CA)」來簽署原點憑證。

使用萬用字元憑證時，使用者**只能**使用 CNAME 來存取服務。例如，如果網域是 `example.com`，而 CNAME 是 `example.cdnedge.bluemix.net`，則您的 CDN 服務**只能** 透過 `https://example.cdnedge.bluemix.net` 存取。請參閱[常見問題](faqs.html#what-should-be-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-cdn-protocols-)，以瞭解使用 HTTPS 搭配萬用字元憑證的含意。

作為業界的最佳作法，Akamai 只會信任主要憑證而不信任中繼憑證，因為受信任的中繼憑證集會經常變更。因為安全的緣故，Akamai 只在 Akamai 授信儲存庫中包含主要憑證管理中心。因此，原點在與 Edge Server 進行 SSL 信號交換時，必須傳送整個憑證鏈，包括葉憑證和所有中繼憑證（不包括主要憑證）。您可以[在這裡](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)找到 Akamai 信任憑證。

### 主機標頭支援

Edge Server 在與原點主機通訊時，會使用主機標頭。此特性提供 Web 服務在原點主機上的配置方式彈性。如果未提供主機標頭輸入，服務會使用原點伺服器主機名稱作為預設 HTTP 主機標頭（如果原點伺服器指定為主機名稱而不是 IP 位址的話）。如果主機標頭未提供為輸入，並且原點伺服器已提供為 IP 位址，則 CDN 主機名稱（也稱為 CDN 網域名稱）會用來作為預設 HTTP 主機標頭。

### 遵循標頭

「遵循標頭」選項容許原點的 HTTP 標頭配置置換 CDN 配置。此特性依預設會開啟，但可以將它關閉。

### 提供陳舊內容

當 CDN Edge Server 收到使用者要求，而所要求的內容並未快取，則 Edge Server 會聯繫原點主機以提取內容。然後該內容便會予以快取，時間是依為內容指定的存活時間 (TTL) 期間。如果 TTL 過期之後收到使用者要求，Edge Server 會聯繫原點主機以提取內容。如果因故無法聯繫原點伺服器（例如，原點主機關閉，或是有網路問題），Edge Server 會提供過期（陳舊）內容給該要求。此特性由 Akamai 支援，且**無法**關閉。

### 「忽略快取金鑰中的查詢引數」特性

Akamai Edge Server 會以所謂的「快取儲存庫」來快取內容。若要使用來自「快取儲存庫」的內容，Edge Server 會使用「快取金鑰」。通常，會根據使用者 URL 的一部分來產生快取金鑰。在部分情況下，URL 會包含因個別使用者而異的函數引數，但遞送的內容相同。依預設，Akamai 會使用查詢函數的引數來產生快取金鑰，從而為每位使用者產生唯一的快取金鑰。此方法不是最佳的方法，因為它會使 Edge Server 與原點伺服器聯絡以取得已快取的內容，但是使用不同的快取金鑰。**忽略快取金鑰中的查詢引數**特性允許您指定在產生快取金鑰時是否應該忽略查詢引數。此特性適用於 CDN 對映配置的任何 `create` 或 `update`，以及原點路徑的任何 `create` 或 `update`。

**附註：**在建立 CDN 對映之後，可以從「設定」標籤配置「快取金鑰查詢引數」。對於原點路徑，可以在原點路徑的 `create` 或 `update` 期間進行配置。

### 內容壓縮

依預設，已針對下列內容類型在 Akamai CDN 中啟用內容壓縮：
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Edge Server 處理壓縮時，內容必須至少 10KB。在部分情況下，會由原點伺服器處理壓縮，而在其他情況下，要壓縮的檔案大小則無限制。如果原點伺服器已經壓縮內容，便不會再次壓縮。若要啟用內容壓縮，要求標頭必須定義 `Accept-Encoding: gzip`。

### 大型檔案最佳化

大型檔案最佳化容許 CDN 網路針對大於 10MB 的內容遞送進行最佳化。啟用此選項可提升大型檔案的效能，並縮短延遲和下載時間。若無此特性，CDN 無法提供大小超過 1.8GB 的檔案。此特性容許大於 1.8GB 的檔案下載，最大可達 320GB。

「大型檔案最佳化」若要運作，**必須**在原點伺服器上啟用位元組範圍要求。Akamai CDN 採用稱為「局部物件快取」的技術來進行此項最佳化。要求大型檔案時，Edge Server 會檢查檔案是否符合大小需求。這表示大於 10MB 的檔案將以片段向原點伺服器要求。片段到達 Edge Server 之後，便會快取並立即提供給使用者。Edge Server 平行預先提取下一個片段，因此可縮短延遲。這個處理程序會持續進行，直到擷取整個檔案或是連線終止。  

啟用此特性時，對於提供小於 10MB 的內容會有輕微的相關聯效能成本。因此，只建議針對提供大型檔案使用此特性。一般的使用案例會是在 CDN 配置中建立新的原點路徑，然後為該路徑啟用「大型檔案最佳化」。

### 隨選視訊

**隨選視訊**效能最佳化可在各種網路類型提供高品質的串流。藉由利用分散式網路的能力動態地分散負載，IBM Cloud CDN 與 Akamai 讓您能快速地針對大型對象而擴充，不論您是否為他們規劃。

**隨選視訊**已針對例如 HLS、DASH、HDS 和 HSS 等分段串流格式的分散而最佳化。目前**不支援**即時視訊串流。在「設定」標籤上，從**進行最佳化**下的下拉功能表選取選項，或是在建立新原點路徑時，都可以啟用**隨選視訊**特性。您應該只在最佳化視訊檔案的遞送時才啟用此特性。
