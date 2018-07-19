---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 限制及最大值

## 「存活時間」有最大值嗎？最小值？

「存活時間」的最大值是 2,147,483,647 秒，約略等於 68 年！最小值是 30 秒。

## 「原點」及 TTL 項目數是否有所限制？

是的，合併限制為每個 CDN 有 75 個項目。

## 我可以擁有的 CDN 數目是否有所限制？

是的，限制是每個帳戶有 10 個 CDN。

## 同時可作用的「清除」要求數目是否有所限制？
是。一次只能有 5 個作用中清除要求。

## 可以透過 Akamai CDN 遞送的最大檔案大小為何？

試圖擷取或遞送大於 1.8GB 的檔案時，會針對預設效能配置收到 `403 Access Forbidden` 的回應。如果已啟用「大型檔案最佳化」，檔案下載最大可達到 320GB。
