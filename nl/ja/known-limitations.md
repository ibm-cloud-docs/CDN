---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-30"

keywords: limitations, Akamai, multiple files, purge, hotlink, limit

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# 既知の制限
{: #known-limitations}

Akamai ベンダーの CDN サービスには、以下の制限があります。
* ディレクトリー・レベルのコンテンツや複数ファイルのパージは、現在サポートされていません。
* 1 つの CDN につきオリジンの総数は 75 個に制限されます。
* DV SAN 証明書を使用する HTTPS は、新規 CDN にのみ使用可能です。
* ホット・リンク保護は、最初の `.` 文字の前の文字セットに `://` 文字のグループが含まれる `refererValues` URL 一致語をサポートしていません (例えば、`http://www.example.com` または `www.example.com http://www.example.com` など)。
* 現時点では、ワイルドカード証明書を使用した HTTPS はサポートされていません。
* ワイルドカード証明書ドメインでは STOP CDN 機能はサポートされていません。

STOP CDN 機能は、7 日以内の保守ウィンドウを対象としています。7 日後に CDN を起動する必要があります。そうしないと、この機能は無効になり、CDN CNAME へのトラフィックはオリジン・サーバーにリダイレクトされません。
{: important}
