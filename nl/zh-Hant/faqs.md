---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 常見問題 (FAQ)

## 什麼是 Content Delivery Network (CDN)？

Content Delivery Network (CDN) 是 Edge Server 的集合，分佈在國家/地區或全世界各地。其 Web 內容是由 Edge Server 所提供，而 Edge Server 位在最接近要求內容之客戶的地理區域中。此技術可讓使用者接收內容，而且其延遲會低於透過遞送某個集中化位置中的內容所產生的延遲。它可針對您的客戶提供較佳的整體經驗。

## CDN 如何運作？

CDN 透過將 Web 內容快取到全球各地的 Edge Server 來達到其目的。使用者要求 Web 內容時，會將內容要求遞送至地理上最接近該使用者的 Edge Server。透過減少內容必須行進的距離，CDN 可提供最佳化產量、最小化延遲，並提高效能。 

## 如何建立 CDN 帳戶？

如果您在瀏覽「供應商選項」功能表之後按一下**選取**，則會在 CDN 訂購處理程序期間建立您的帳戶。

## CDN 處於 CNAME_CONFIGURATION 狀態時我該怎麼做？

請更新 DNS 記錄，讓網站指向與新 CDN 對映相關聯的 `CNAME`。
**附註**：最多可能需要 15-30 分鐘，更新才會作用。請與 DNS 提供者聯繫，以取得精確的預估時間。

## 向我收費的內容為何？

根據每個 CDN 所使用的頻寬向您收費。如果 CDN 未使用任何頻寬，則不會引發任何費用。頻寬價格取決於 Edge Server 的地區位置。

## 何時向我收費？

CDN 計費是根據「{{site.data.keyword.BluSoftlayer_notm}} 帳戶」中所建立的計費期間進行。

## 如何檢視度量值及使用情形？

您可以在**概觀**頁面上檢視度量值及使用情形，從 **Content Delivery Networks** 頁面中選取 CDN 即可到達此頁面。**附註**：在您建立 CDN 之後，最多可能需要 48 個小時，才會顯示度量值。

## 我最少可以檢視度量值幾天？有上限嗎？

是。最少可以收集度量值 7 天。最多可以檢視度量值 90 天。如果是使用 API，則建議使用 89 天作為上限，以說明任何時區差異。

## HTTPS 萬用字元憑證如何運作？

萬用字元憑證是將 Web 內容安全地遞送給一般使用者的最經濟方式。若要使用萬用字元憑證，客戶必須使用 CNAME 主機名稱作為服務進入點（例如，_https://example.cdnedge.bluemix.net_）。使用萬用字元憑證針對 HTTPS 啟用 CDN 對映之後，Edge Server 會透過 HTTPS 聯絡「原始伺服器」。依預設，如果將「原始伺服器」指定為主機名稱，則 Edge Server 會使用原始網域作為「伺服器名稱指示 (SNI)」標頭，以進行與「原始伺服器」的「傳輸層安全 (TLS)」協議。不過，如果將原始網域指定為 IP 位址，則應該提供原始主機標頭。另一個提示是必須由已辨識的「憑證管理中心 (CA)」簽署原始憑證。

## 如果我在 CDN 的「溢位」功能表中選取 `delete`，這會刪除我的帳戶嗎？

不會；它只會刪除該 CDN。您的帳戶仍然存在，而且您可以建立其他 CDN。

## 快取是使用推送還是取回？

「內容快取」是使用_原始取回_ 模型進行。「原始取回」是 Edge Server 用來從「原始伺服器」「取回」資料的方法，而不是用來將內容手動上傳至 Edge Server。 

## 「存活時間」有最大值嗎？最小值？

「存活時間」的最大值是 21,474,836,471 秒，約略等於 681 年！最小值是 30 秒。

## 何謂「提供過時選項」？

如果內容的快取時間到期，則 Edge Server 會嘗試從「原始伺服器」中提取內容。如果已設定該選項（這是大部分客戶偏好的方式），則「原始伺服器」因某個原因而關閉或無法通訊時，Edge Server 可能會提供過時內容。目前，只有在此選項設為**是**（預設值）時，才能配置 CDN。將此選項變更為**否**不會有任何影響：**提供過時內容**選項將繼續設為**是**。

## 「原始」及 TTL 項目數是否有所限制？

是的，合併限制為每個 CDN 有 75 個項目。

## 我可以擁有的 CDN 數目是否有所限制？

是的，限制是每個帳戶有 10 個 CDN。

## 是否有建議用於 CDN 服務配置的瀏覽器？

是，建議瀏覽器是最新版的 Firefox 及 Chrome。

## 為何要在建立 CDN 時提供「路徑」？

如果您在建立 CDN 時提供路徑，則可讓您隔離可透過 CDN 提供的檔案與特定「原始伺服器」。

## 如何知道 CDN 正常運作？
請將 "origin.cdntesting.net/assets/ibm_3d.gif" 取代為您原始伺服器上的個別檔案路徑，以執行下列 'curl' 指令： 
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

## 如果我未提供 CName，則在哪裏可以找到它？ 
按一下 CDN，以移至**概觀**頁面。您會在右上角看到包含 `CName` 資訊的**詳細資料**區段。

## 同時可作用的「清除」要求數目是否有所限制？
是。一次只能有 5 個作用中清除要求。

## 對於給定檔案路徑的「我的清除要求」正在進行中。我可以針對相同的檔案路徑提交新的要求嗎？
不行。一次只能有一個對於給定檔案路徑的作用中「清除」要求。
