---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 既知の制限

Akamai ベンダーの新しい CDN サービスには、以下の制限があります。
* ディレクトリー・レベルのコンテンツおよび複数ファイルのパージは、現在サポートされていません。
* 1 つの CDN につきオリジンの総数は 75 個に制限されます。
* DV SAN 証明書を使用する HTTPS は、新規 CDN にのみ使用可能です。
* ホット・リンク保護は、最初の `.` 文字の前の文字セットに `://` が含まれる `refererValues` URL 一致語をサポートしていません (例えば、`http://www.example.com` または `www.example.com http://www.example.com` など)。
