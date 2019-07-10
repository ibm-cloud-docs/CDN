---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 入力コンテナー
{: #input-container}

入力コンテナーは、マッピング・クラスと (オリジン) パス・クラスの両方によって使用されるコレクションです。 これは、両方のクラスに対して一貫性のある入力属性のセットを提供します。

* `vendorName`: 有効な {{site.data.keyword.cloud}} CDN プロバイダーの名前。
* `oldPath`: updateOriginPath() によって使用されます。 このプロパティーは、現行パスまたは「古い」パスの名前を保管します。

以下の属性は、マッピング・クラスと (オリジン) パス・クラスに共通です。
* `originType`: オリジン・ホストのタイプ。現在、「HOST_SERVER」または「OBJECT_STORAGE」です。
* `origin`: オリジン・サーバー・アドレス (オリジン・サーバーのホスト名または IPv4 アドレスのいずれか)。これは、コンテンツのフェッチ元のエンドポイントまたはコンテンツが保管されているバケットの名前です。 511 文字未満でなければなりません。
* `httpPort`: HTTP プロトコルに使用されるポートの番号。 Akamai には、HTTP ポートと HTTPS ポートのポート番号に関する特定の制限があります。 許可されるポート番号のリストについては、[よくある質問](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。
* `httpsPort`: HTTPS プロトコルに使用されるポートの番号。 Akamai には、HTTP ポートと HTTPS ポートのポート番号に関する特定の制限があります。 許可されるポート番号のリストについては、[よくある質問](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。
* `status`: マッピングまたはパスの状況。 状況は、CNAME_CONFIGURATION、SSL_CONFIGURATION、RUNNING、STOPPED、DELETED、または ERROR になります。
* `path`: キャッシュに入れられたコンテンツの配信元のパス。 デフォルトのパスは /\* です。`updateOriginPath` API によって使用される場合、この属性は追加される新規パスを指します。
* `performanceConfiguration`: マッピングのパフォーマンス構成の仕様。
* `cacheKeyQueryRule`: キャッシュ・キーの動作を構成するために以下のオプションを使用できます。
  * `include-all`: すべての照会引数を含めます
  * `ignore-all`: すべての照会引数を無視します
  * `ignore: space separated query-args`: スペースで区切られた特定の照会引数を無視します。 例えば、`ignore: query1 query2` です
  * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。 例えば、`include: query1 query2` です
* `geoblockingRule`

以下の属性は、マッピング・クラスに固有です。

* `uniqueId`: 各マッピングに固有の、システムによって生成された 10 桁の ID。 マッピングの作成時に生成されます。
* `domain`: 1 次 CDN 名。 ホスト名とも呼ばれます。
* `protocol`: サービスのセットアップに使用されるプロトコル。 HTTP、HTTPS、またはこれらの 2 つの組み合わせ (HTTP_AND_HTTPS) を指定できます。
* `cname`: 正規名レコードはホスト名の別名です。 ユーザーが指定することも、システムによって生成することもできます。 ユーザー指定の場合、64 文字未満の英数字で、固有でなければなりません。 ユーザーが指定しない場合、マッピングの作成時に生成されます。
* `certificateType`: マッピングによって使用されている証明書のタイプ。 `WILDCARD_CERT` または `SHARED_SAN_CERT` です。 値は、HTTP マッピングの場合は 0 です。
* `respectHeaders`: 「true」に設定した場合、オリジンの TTL 設定によって CDN TTL 設定がオーバーライドされるようになるブール値。
* `header`: オリジンによって使用されるホスト・ヘッダー情報を指定します。

以下の属性は、クラウド・オブジェクト・ストレージ (COS) に関連しています。  
* `bucketName`: S3 オブジェクト・ストレージのバケットの固有の名前。  
* `fileExtension`: 許可されるファイル拡張子。

以下の属性は、ホット・リンク保護の構成に関連しています。
* `hotlinkProtection`: 詳細についは、『[ホット・リンク保護クラス](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)』を参照してください。
