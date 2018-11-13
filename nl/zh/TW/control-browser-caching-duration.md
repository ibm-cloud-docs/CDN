---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 使用 Cache-Control `max-age` 來控制瀏覽器快取持續時間

使用 CDN 時，針對可以發生快取的每個位置有一種快取類型：
  * **瀏覽器層次快取**發生於瀏覽器快取一項內容，長達特定的時間量。
  * **邊緣層次快取**發生於 CDN 邊緣伺服器快取來自原點的一項內容，長達特定但不一定相同的時間量。

有一些方法可以控制瀏覽器層次的內容快取時間長度。要使用的方法取決於下列因素：
  * 您是否已更新，或是將會[更新 CDN 的配置詳細資料](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details)，並使用「遵循標頭：開啟」。
  * 原點伺服器是否在特定內容的 Cache-Control 標頭中提供 `max-age` 值。 

不管這些因素如何變更，您的原點需要為目標內容提供非空白的 Cache-Control 標頭。如果您的原點不提供非空白的 Cache-Control 標頭給邊緣伺服器，邊緣伺服器將不會提供它自己的 Cache-Control 標頭給瀏覽器。

當邊緣伺服器傳送 Cache-Control 標頭給瀏覽器時，此 Cache-Control 標頭的 `max-age` 值會對應於剩餘的時間量，之後邊緣伺服器上的內容便會過舊，而需要從原點重新驗證。 

## 遵循標頭：關閉
當「遵循標頭」設為**關閉**時，您可以藉由配置 CDN 的 TTL 設定來控制瀏覽器層次快取時間。針對每個檔案或檔案目錄，您需要為其路徑和想要的快取持續時間建立 TTL 規則。

這項設定會進行兩件事：
  * TTL 規則告訴邊緣伺服器將內容快取長達指定的時間量。
  * 從邊緣伺服器提供內容時，邊緣伺服器會傳送它自己的 Cache-Control 標頭以及 `max-age` 值（以 TTL 持續時間為基礎）給瀏覽器。

## 遵循標頭：開啟
當**遵循標頭**設為**開啟**時，您可以選擇不要配置 TTL 規則來控制內容的瀏覽器層次快取時間。在該情況下，您需要配置原點伺服器，以為特定內容提供 Cache-Control 標頭與 `max-age` 值。由於「遵循標頭」為開啟，邊緣伺服器會使用來自原點的 Cache-Control 的 `max-age` 值，作為該特定內容的 TTL 持續時間。如果您已有涵蓋該內容的 CDN TTL 規則項目，邊緣伺服器實際上仍會使用 `max-age` 值，並改寫該內容的任何現有邊緣層次快取持續時間。

這項設定會進行兩件事：
  * 來自原點的 Cache-Control `max-age` 指引的值會告訴邊緣伺服器將內容快取長達指定的時間量。
  * 從邊緣伺服器提供內容時，邊緣伺服器會傳送它自己的 Cache-Control 標頭以及 `max-age` 值（以來自原點的 `max-age` 值為基礎）給瀏覽器。

如果您使用範例中所示的目標方法，該 TTL 規則所涵蓋的任何其他內容都不受影響。

當**遵循標頭**設為**開啟**時，瀏覽器上的快取持續時間可以由 TTL 規則控制，如果原點伺服器不在 Cache-Control 標頭中傳送 `max-age` 指引的話。如果它指定了 `max-age`，則該值可能會意外地置換 TTL 規則上的時間值。

## 摘要

|遵循標頭|原點提供 Cache-Control|邊緣伺服器上的特定內容快取持續時間|邊緣伺服器提供 Cache-Control|
|---|---|---|---|
|開啟|是，指定 `max-age`|使用原點的 `max-age` 值改寫|是，`max-age` 是邊緣的內容需要從原點進行重新驗證之前的時間量|
|開啟|是，不指定 `max-age`|根據 CDN 的 TTL 配置|是，`max-age` 是邊緣的內容需要從原點進行重新驗證之前的時間量|
|開啟|否 |根據 CDN 的 TTL 配置|否 |
|關閉|是，指定 `max-age`|根據 CDN 的 TTL 配置|是，`max-age` 是邊緣的內容需要從原點進行重新驗證之前的時間量|
|關閉|是，不指定 `max-age`|根據 CDN 的 TTL 配置|是，`max-age` 是邊緣的內容需要從原點進行重新驗證之前的時間量|
|關閉|否 |根據 CDN 的 TTL 配置|否 |

## 相關資訊
* 如何[管理 CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt) 14.9 節所定義的 Cache-Control
