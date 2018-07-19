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

# 既知の制限

Akamai ベンダーの新しい CDN サービスには、以下の制限があります。
* ディレクトリー・レベルのコンテンツおよび複数ファイルのパージは、現在サポートされていません。
* 特定の {{site.data.keyword.BluSoftlayer_notm}} アカウントに現在許可される CDN の数は 10 に制限されています。
* オリジンと TTL エントリーの合計数は、CDN 当たり 75 に制限されています。
* 失効コンテンツの提供オプションの機能は、常に**オン**になります。
* マッピングが作成された後で HTTPS 証明書タイプを (例えば、ワイルドカードから DV SAN に) 変更することはできません。
* DV SAN 証明書を使用する HTTPS は、新規 CDN にのみ使用可能です。
* DV SAN 証明書を指定して作成された CDN は、RUNNING、CNAME_Configuration、CREATE_ERROR、または DELETE_ERROR 状態でない限り削除できません。
