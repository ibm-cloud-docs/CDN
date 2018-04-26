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

# 既知の制限

Akamai ベンダーの新しい CDN サービスには、以下の制限があります。
* HTTPS は現在、ワイルドカード証明書を通じてのみサポートされます。
* ディレクトリー・レベルのコンテンツおよび複数ファイルのパージは、現在サポートされていません。
* 特定の {{site.data.keyword.BluSoftlayer_notm}} アカウントに現在許可される CDN の数は 10 に制限されています。
* オリジンと TTL エントリーの合計数は、CDN 当たり 75 に制限されています。
* 「失効コンテンツの提供」オプションの機能は、常に**「オン」**です。 これは、このオプションを**「オフ」**として CDN を作成した場合にも当てはまります。 
* CDN が**「サーバー」**と**「HTTP ポート」**を指定して作成された場合、オリジンは**「サーバー」**オプションを使用してのみ追加できます。
