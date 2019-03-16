---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 已知限制
{: #known-limitations}

下列限制適用於「Akamai 供應商」的 CDN 服務：
* 目前不支援清除目錄層次內容或多個檔案。
* 每個 CDN 的總原點數限制為 75 個。
* 具有 DV SAN 憑證的 HTTPS 僅適用於新的 CDN。
* 「快速鏈結保護」不支援 `refererValues` URL 相符項目，其中，第一個 `.` 字元之前的字集包含字元 `://` 的分組，例如，`http://www.example.com` 或 `www.example.com http://www.example.com`
