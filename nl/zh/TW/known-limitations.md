---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 已知限制

下列限制適用於 Akamai 供應商的新 CDN 服務：
* 目前不支援清除目錄層次內容或多個檔案。
* 給定「{{site.data.keyword.BluSoftlayer_notm}} 帳戶」目前容許 10 個 CDN 的限制。
* 「原點」及 TTL 項目總數的限制是每個 CDN 為 75 個。
* 「提供過時內容選項」功能將一律為**開啟**。
* 在建立對映之後，即無法變更 HTTPS 憑證類型，例如，從「萬用字元」到 DV SAN。
* 具有 DV SAN 憑證的 HTTPS 僅適用於新的 CDN。
* 無法刪除使用 DV SAN 憑證所建立的 CDN，除非它處於 RUNNING、CNAME_Configuration、CREATE_ERROR 或 DELETE_ERROR 狀態。
