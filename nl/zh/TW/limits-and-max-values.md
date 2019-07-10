---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 限制及最大值
{: #limits-and-maximum-values}

## 「存活時間」有最大值嗎？最小值？
{: #is-there-a-maximum-value-for-time-to-live}

「存活時間」的最大值是 2,147,483,647 秒，約略等於 68 年！最小值是 0 秒。

## 「原點」及 TTL 項目數是否有所限制？
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

是的，合併限制為每個 CDN 有 75 個項目。

## 可以透過 Akamai CDN 遞送的最大檔案大小為何？
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

試圖擷取或遞送大於 1.8GB 的檔案時，會針對預設效能配置收到 `403 Access Forbidden` 的回應。如果已啟用「大型檔案最佳化」，檔案下載最大可達到 320GB。

## 原點回應標頭的大小上限為何？
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

回應標頭大小上限為 16KB。如果特定原點嘗試設定的 Cookie 太大，導致用戶端無法在後續要求中傳回，則 Akamai 會在邊緣伺服器中設定此限制。如果回應標頭大於 16KB，則要求會取得 `502 Bad Gateway` 回應。

## 用戶端要求標頭的大小上限為何？
{: #what-is-the-maximum-size-for-the-client-request-headers}

要求標頭大小上限為 32KB。如果要求標頭大於 32KB，則 Akamai 邊緣伺服器會傳回 `400 Bad Request` 回應。
