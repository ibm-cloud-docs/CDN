---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# 特性說明
{: #feature-descriptions}

此頁面會強調顯示採用 Akamai 技術之 {{site.data.keyword.cloud}} CDN 所含的許多特性。

## 主伺服器原點支援
{: #host-server-origin-support}

IBM Cloud Content Delivery Network (CDN) 可以配置為從「主伺服器原點」提供內容，方法是提供原點主機名稱、通訊協定、埠號，以及選擇性的內容提供路徑。預設路徑是 `/*`。通訊協定可以是 HTTP 及/或 HTTPS。Akamai 只支援特定埠號。如需支援的埠號/範圍，請參閱[常見問題](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。

## Object Storage 原點支援
{: #object-storage-file-support}

IBM Cloud CDN 可以配置為從 Object Storage 端點提供內容，方法是提供端點、「儲存區」名稱、通訊協定和埠。可以選擇性地指定副檔名清單，以便只允許快取具有那些副檔名的檔案。儲存區中的所有物件都需要設定匿名讀取或公用讀取權。

這份關於[如何使用 CDN 設定 IBM Cloud Object Storage](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) 的指導教學提供更詳細的資訊。

## 相異路徑之多原點支援
{: #support-for-multiple-origins-with-distinct-paths}

在特定情況中，您可能會想要從不同的原點伺服器遞送特定內容。例如，您可能想要從不同的原點伺服器提供照片或視訊。IBM Cloud CDN 提供選項來設定具有多條路徑的多個原點伺服器。這樣能允許資料儲存的方式和位置具有彈性。為原點伺服器提供的路徑應該對於 CDN 而言是唯一的。原點伺服器本身不需要是唯一的。

## 以路徑為基礎的 CDN 對映
{: #path-based-cdn-mappings}

您的 IBM Cloud CDN 服務可以限制為原點伺服器上的特定目錄路徑，方法是在建立 CDN 時提供路徑。使用者只能夠存取該目錄路徑中的內容。例如，如果建立的 CDN `www.example.com` 具有路徑 `/videos`，則**只能**透過 `www.example.com/videos/*` 來存取它。

## 清除快取內容
{: #purge-cached-content}

IBM Cloud CDN 提供方便快速地移除（或「清除」）邊緣伺服器上之快取內容的功能。在目前，只允許針對檔案路徑進行清除。Akamai 的清除實作目前會在大約 5 秒內清除檔案。使用者介面也提供檢視清除歷程以及將特定清除路徑標記為最愛的功能。

## 存活時間 (TTL)
{: #time-to-live-ttl}

「存活時間」是指邊緣伺服器將保留針對該特定檔案或目錄路徑所快取之內容的時間量（以秒為單位）。一開始建立 CDN 時，會為路徑 `/\*` 建立廣域的「存活時間 (TTL)」，預設時間為 3600 秒。TTL 的最小值是 0 秒，最大值則是 2147483647 秒。針對每個項目，TTL 路徑應該為 CDN 獨有。如果多條路徑都符合給定的內容，最近配置的路徑相符項會適用於該內容。例如，假設有兩個 TTL，`/example/file` 先建立，存活時間值為 3000 秒，而 `/example/*` 較晚建立，值為 4000 秒。雖然 `/example/file` 較為明確，但 `/example/*` 最近才建立，因此 `/example/file` 的 TTL 將為 4000 秒。建立之後，可以編輯 TTL 項目的路徑及/或時間。也可以予以刪除。

## 具有圖形視圖的度量值
{: #metrics-with-graphical-views}

個別 CDN 的度量值提供於該 CDN 對映的客戶入口網站「概觀」標籤。有兩種度量值會從 CDN 的用量計算而來：將某時段度量值顯示為圖形的度量值，以及顯示為總計值的度量值。

對於將一段時間的變化顯示為圖形的度量值，您可以看到三個折線圖和一個圓餅圖。三個折線圖是：**頻寬**、**依對映的命中數**，以及**依類型的命中數**。它們會顯示在您指定時間範圍的過程中，每日活動的情形。**頻寬**和**依對映的命中數**的圖形是單一折線圖，而**依類型的命中數**的分解會針對所提供的每一種命中類型顯示一條折線。圓餅圖會以百分比為基準，顯示 CDN 對映頻寬的地區分解。

總計值所顯示的度量值，包括**頻寬用量** (GB)、對 CDN 邊緣伺服器的**命中數總計**，以及**命中率**。命中率指出內容由邊緣伺服器而_非_ 透過其原點提供的次數百分比。命中率目前顯示為所有 CDN 對映的函數，而不只是正在檢視的 CDN 對映。

依預設，總計值和圖形都會預設為顯示過去 30 天的度量值，但您可以透過[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 來變更這點。兩個種類都能夠顯示 7、15、30、60 或 90 天期間的度量值。

## 主機標頭支援
{: #host-header-support}

邊緣伺服器在與「原點」主機通訊時，會使用**主機標頭**。此特性提供 Web 服務在原點主機上的配置方式彈性。特別是可以讓用戶端在相同的「原點主機」上配置多個 Web 伺服器。如果未提供主機標頭輸入，服務會使用原點伺服器主機名稱作為預設 HTTP 主機標頭（如果原點伺服器指定為主機名稱而不是 IP 位址的話）。如果主機標頭未提供為輸入，並且原點伺服器已提供為 IP 位址，則 CDN 主機名稱（也稱為 CDN 網域名稱）會用來作為預設 HTTP 主機標頭。

## HTTPS 通訊協定支援
{: #https-protocol-support}

CDN 可以配置為使用 HTTPS 通訊協定，將內容安全地提供給一般使用者。此配置需要 SSL 憑證必須設定為 CDN 配置的一部分。有兩種類型的 SSL 憑證選項適用於 HTTPS：[萬用字元憑證](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support)及[網域驗證 (DV) 主體替代名稱 (SAN) 憑證](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport)。在本文件中，此類型也稱為 _SAN 憑證_。

要使用的「SSL 憑證」類型是 HTTPS CDN 的重要考量。萬用字元憑證配置設定十分快速，但缺點是只有透過 CNAME 才能存取 CDN。SAN 憑證處理程序需要 4 到 8 小時才能完成，但可以搭配使用 CDN 與「CDN 網域」（即「主機名稱」）。在配置期間，「SAN 憑證」也需要額外的[**網域控制驗證**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san)步驟。使用上述任一憑證沒有任何相關聯的成本。請參閱[疑難排解文件](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-)，以瞭解選取給定「憑證」類型的連帶作用。

「原點主機」也必須將其專屬 SSL 憑證用於 CDN 主機名稱，而且必須由受到認可的「憑證管理中心 (CA)」進行簽署。

作為業界的最佳作法，Akamai 只會信任主要憑證而不信任中繼憑證，因為受信任的中繼憑證集會經常變更。您可以[在 Web 的這個位置](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)找到 Akamai 信任憑證。

## 遵循標頭
{: #respect-headers}

**遵循標頭**選項容許「原點」中的「HTTP 標頭」配置置換 CDN 配置。此特性依預設會開啟，但可以將它關閉。

## 提供陳舊內容
{: #serve-stale-content}

當 CDN 邊緣伺服器收到使用者要求，而所要求的內容並未快取，則邊緣伺服器會聯繫原點主機以提取內容。然後該內容便會予以快取，時間是依為內容指定的存活時間 (TTL) 期間。如果 TTL 過期之後收到使用者要求，邊緣伺服器會聯繫原點主機以提取內容。如果因故無法聯繫原點伺服器（例如，原點主機關閉，或是有網路問題），邊緣伺服器會提供過期（陳舊）內容給該要求。此特性由 Akamai 支援，且**無法**關閉。

## 快取金鑰最佳化
{: #cache-key-optimization}

Akamai 邊緣伺服器會快取**快取儲存庫**上的內容。若要使用來自**快取儲存庫**的內容，邊緣伺服器會使用**快取金鑰**。通常，會根據一般使用者 URL 的一部分來產生**快取金鑰**。在部分情況下，URL 會包含因個別使用者而異的函數引數，但遞送的內容相同。依預設，Akamai 會使用查詢函數的引數來產生快取金鑰，從而為每位使用者產生唯一的快取金鑰。此方法不是最佳的方法，因為它會使邊緣伺服器與原點伺服器聯絡以取得已快取的內容，但是使用不同的快取金鑰。**快取金鑰最佳化**特性容許您指定在產生「快取金鑰」時，要包括或忽略哪些查詢引數。此特性適用於 CDN 對映配置的任何 `create` 或 `update`，以及原點路徑的任何 `create` 或 `update`。

在建立 CDN 對映之後，可以從**設定**標籤來配置**快取金鑰最佳化**的值。對於原點路徑，則可以在原點路徑的 `create` 或 `update` 作業期間進行配置。
{: note}

## 內容壓縮
{: #content-compression}

依預設，已針對下列內容類型在 Akamai CDN 中啟用**內容壓縮**：
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

邊緣伺服器處理壓縮時，內容必須至少 10KB。在部分情況下，會由原點伺服器處理壓縮，而在其他情況下，要壓縮的檔案大小則無限制。如果原點伺服器已經壓縮內容，便不會再次壓縮。若要啟用內容壓縮，要求標頭必須定義 `Accept-Encoding: gzip`。

## 大型檔案最佳化
{: #large-file-optimization}

大型檔案最佳化容許 CDN 網路針對大於 10MB 的內容遞送進行最佳化。啟用此選項可提升大型檔案的效能，並縮短延遲和下載時間。若無此特性，CDN 無法提供大小超過 1.8GB 的檔案。此特性容許大於 1.8GB 的檔案下載，最大可達 320GB。

若要讓「大型檔案最佳化」運作，**必須**在「原點伺服器」上啟用位元組範圍要求。Akamai CDN 採用稱為_局部物件快取_ 的技術來進行此最佳化。要求大型檔案時，邊緣伺服器會檢查檔案是否符合大小需求。這表示大於 10MB 的檔案將以片段向「原點伺服器」要求。片段到達邊緣伺服器之後，便會快取並立即提供給使用者。邊緣伺服器平行預先提取下一個片段，因此可縮短延遲。這個處理程序會持續進行，直到擷取整個檔案或是連線終止。

啟用此特性時，對於提供小於 10MB 的內容會有輕微的相關聯效能成本。因此，只建議針對提供大型檔案使用此特性。一般的使用案例會是在 CDN 配置中建立新的原點路徑，然後為該路徑啟用「大型檔案最佳化」。

## 隨選視訊
{: #video-on-demand}

**隨選視訊**效能最佳化可在各種網路類型提供高品質的串流。利用預先配置的快取控制設定以及分散式網路的能力，以動態方式分散負載，IBM Cloud CDN 與 Akamai 便可讓您快速地針對大量觀眾進行擴充，不論您是否已為他們規劃。

**隨選視訊**已針對例如 HLS、DASH、HDS 和 HSS 等分段串流格式的分散而最佳化。目前**不支援**即時視訊串流。在「設定」標籤上，從**進行最佳化**下的下拉功能表選取選項，或是在建立新原點路徑時，都可以啟用**隨選視訊**特性。您應該只在最佳化視訊檔案的遞送時才啟用此特性。

## 地理存取控制
{: #geographical-access-control}

「地理存取控制」是以規則為基礎的行為，它讓您為一群使用者根據他們的地理位置設定 `access-type` 參數。有兩種類型的行為可用：**Allow** 及 **Deny**。

access-type `Allow` 讓您明確地容許資料流量根據地區類型，送到選取的地區。特定地區的容許資料流量，會隱含地封鎖所有其他地區的資料流量。例如，您可能選擇 `Allow` 對所選取大陸的資料流量，例如歐洲和大洋洲，這樣會封鎖所有其他大陸的存取。

在另一方面，`Deny` 行為會封鎖指定群組的服務存取，但容許所有其他未指定地區的存取。例如，如果您將「地理存取控制」access-type 設定為針對歐洲和大洋洲的內容加以 `Deny`，則那些大陸上的使用者將**無法**使用您的服務，而所有其他大陸上的使用者將能夠存取。

此特性可從您「CDN 配置」的**設定**頁面存取。

## 快速鏈結保護
{: #hotlink-protection}

「快速鏈結保護」是個規則型行為，可讓您控制是否容許特定網站從您的 CDN 存取內容。從網頁上的鏈結提出一個 HTTP 要求時，以及當該鏈結指向遠端資產時，瀏覽器一般會併入「`Referer` 標頭」。一個網站用來存取另一個網站中資產的鏈結，稱為_快速鏈結_。有兩種類型的行為可用：**ALLOW** 及 **DENY**。

如果您的 `protectionType` 設為 `ALLOW`：
* 在傳送至您 CDN 的要求中，如果 `Referer` 標頭值符合您指定的其中一個 `refererValues`，則您的 CDN **會**提供所要求的內容。
* 否則，您的 CDN **不會**提供內容。

如果您的 `protectionType` 設為 `DENY`：
* 在傳送至您 CDN 的要求中，如果 `Referer` 標頭值符合您指定的其中一個 `refererValues`，則您的 CDN **不會**提供所要求的內容。
* 否則，您的 CDN 會提供內容。
