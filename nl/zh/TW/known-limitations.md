---

copyright:
  years: 2017
lastupdated: "2017-09-10"

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
* 目前只能透過萬用字元憑證支援 HTTPS。
* 目前不支援清除目錄層次內容或多個檔案。
* 給定「{{site.data.keyword.BluSoftlayer_notm}} 帳戶」目前容許 10 個 CDN 的限制。
* 「原點」及 TTL 項目總數的限制是每個 CDN 為 75 個。
* 「提供過時內容選項」功能將一律為**開啟**，即使是 CDN 建立時為**關閉**。 
* 如果 CDN 建立時有**伺服器**和 **HTTP 埠**，則「原點」只能使用**伺服器**選項來新增。
