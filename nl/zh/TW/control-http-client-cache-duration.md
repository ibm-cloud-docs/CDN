---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 使用 Cache-Control 來控制 HTTP 用戶端的快取持續時間
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

使用 CDN 時，有兩個層次的快取可供使用：

  * **在邊緣快取**，在 CDN 邊緣伺服器從原點快取內容時發生。
  * 從伺服器的邊緣網路**往下游快取**，在一般使用者或 HTTP 用戶端（例如，發出要求的瀏覽器）從邊緣伺服器快取內容時發生。

您選擇要用來控制要求端（例如，瀏覽器）內容快取時間長度的方法，取決於下列因素：

  * [遵循標頭設定](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details)為 ON 或 OFF。依預設，其設為 ON。
  * 原點伺服器是否在特定內容的 Cache-Control 標頭中提供 `max-age` 值。 

不管這些因素如何變更，如果您想要邊緣伺服器傳送的 HTTP 回應含有想要內容的 Cache-Control 標頭，則您的原點必須提供該內容的 Cache-Control 標頭給邊緣。

基本上，從邊緣伺服器往下游傳送的 Cache-Control 標頭，會根據邊緣伺服器所指定的快取指引或值，要求要求端快取相關聯的內容。

## 遵循標頭：關閉
{: #respect-header-off}

如果您的原點針對特定的內容，提供含有 `max-age` 指引及值的 Cache-Control 標頭，則在邊緣上快取特定內容的快取持續時間仍然衍生自您 CDN 的 TTL 設定。此外，邊緣伺服器使用 Cache-Control `max-age` 值來回應要求端下游，該值等於下列兩者之中較小的值：
  * 原點的 Cache-Control `max-age` 值。
  * 內容在邊緣上過時之前的剩餘時間。

然而，如果您的原點未提供 Cache-Control 標頭給邊緣伺服器，則邊緣伺服器不會提供 Cache-Control 標頭給要求端。您內容的邊緣快取持續時間仍然衍生自您 CDN 的 TTL 設定。

## 遵循標頭：開啟
{: #respect-header-on}

如果您的原點針對特定的內容，提供含有 `max-age` 的 Cache-Control 標頭，則原點的 Cache-Control `max-age` 值會變成在邊緣上快取該特定內容的快取持續時間，並且取代所有適用於該內容的 TTL 設定。此外，邊緣伺服器使用 Cache-Control `max-age` 值來回應要求端，該值等於內容在邊緣伺服器上過時之前的剩餘時間。

然而，如果您的原點未提供 Cache-Control 標頭給邊緣伺服器，則邊緣伺服器不會提供 Cache-Control 標頭給要求端。您內容的邊緣快取持續時間仍然衍生自您 CDN 的 TTL 設定。

## 摘要
{: #summary}

|遵循標頭|原點提供 Cache-Control|邊緣伺服器上的特定內容快取持續時間|邊緣伺服器提供 Cache-Control|
|---|---|---|---|
|開啟|是，原點指定 `max-age`|邊緣快取持續時間改寫成原點的 `max-age` 值|是，邊緣也提供 `max-age`，其值為邊緣在重新整理原點中內容之前所需要的（置換）時間|
|開啟|是，但原點不指定 `max-age`|邊緣快取持續時間根據 CDN 的 TTL 配置|是，邊緣也提供 `max-age`，其值為邊緣在重新整理原點中內容之前所需要的時間|
|開啟|否 |邊緣快取持續時間根據 CDN 的 TTL 配置|否 |
|關閉|是，原點指定 `max-age`|邊緣快取持續時間根據 CDN 的 TTL 配置|是，邊緣也提供 `max-age` 值，其小於原點的 `max-age` 值，且為邊緣在重新整理原點中內容之前所需要的時間|
|關閉|是，但原點不指定 `max-age`|邊緣快取持續時間根據 CDN 的 TTL 配置|是，邊緣也提供 `max-age`，其值為邊緣在重新整理原點中內容之前所需要的時間|
|關閉|否 |邊緣快取持續時間根據 CDN 的 TTL 配置|否 |

## 快取控制的相關資訊
{: #more-information-on-cache-control}

* 如何[管理 CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* [RFC 2616 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示") 14.9 節所定義的 Cache-Control](https://www.ietf.org/rfc/rfc2616.txt)
