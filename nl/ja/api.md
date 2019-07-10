---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}


# CDN API リファレンス
{: #cdn-api-reference}

{{site.data.keyword.cloud}} で提供される {{site.data.keyword.cloud}} Infrastructure アプリケーション・プログラミング・インターフェース (通常 SLAPI と呼ばれる) は、開発者やシステム管理者が {{site.data.keyword.cloud_notm}} Infrastructure バックエンド・システムと直接対話することができる開発用インターフェースです。

SLAPI は、カスタマー・ポータルにある多くの機能を実装しています。カスタマー・ポータルで対話が可能であれば、SLAPI でも実現できます。 SLAPI 内で {{site.data.keyword.cloud_notm}} Infrastructure 環境のすべての部分とプログラムで対話できるため、API を使用してタスクを自動化することができます。

SLAPI は、リモート・プロシージャー・コール (RPC) システムです。 各呼び出しでは、API エンドポイントにデータが送信され、戻りとして構造化データを受け取ります。 SLAPI でのデータの送受信に使用される形式は、選択した API の実装によって異なります。 SLAPI は現在、データ伝送に SOAP、XML-RPC、または REST を使用しています。

SLAPI、または {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN) サービス API について詳しくは、{{site.data.keyword.cloud_notm}} Development Network にある以下のリソースを参照してください。

* [SLAPI Overview](https://softlayer.github.io/ )
* [Getting Started with SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API Guide](https://softlayer.github.io/article/php/ )

----

始めに、API 呼び出しの推奨手順を以下に示します。
* `listVendors` - サポートされているベンダーのリストを提供します。
* `verifyOrder` - 発注できるかどうかを確認します。
* `placeOrder`  - 指定のベンダーを使用して CDN アカウントを作成します。 placeOrder 呼び出しが正常に実行されると、最大 10 CDN マッピングが作成できます。
* `createDomainMapping` - CDN マッピングを作成します。
* `verifyDomainMapping` - CDN の状況を _RUNNING_ に変更します。

前述の手順に従った後、その他の API を使用できます。

[この呼び出し手順の各ステップについて、コード例が用意されています。](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

この文書にあるほとんどの API 呼び出しで、`CDN_ACCOUNT_MANAGE` アクセス権を備えたユーザーの API ユーザー名と API キーを使用する**必要があります**。このアクセス権を自分用に有効にする必要がある場合は、ご使用のアカウントのマスター・ユーザーにお問い合わせください。 (各 IBM Cloud カスタマー・アカウントに 1 つのマスター・ユーザーが指定されます)。
{: note}

----
## ベンダー用の API
{: #api-for-vendor}

### listVendors
この API で、ユーザーはサポートされている CDN ベンダーをリストできます。 CDN アカウントを作成し、CDN の注文を開始するには、`vendorName` が必要です。

* **必須パラメーター**: なし
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Vendor` のコレクション

  ベンダー・コンテナーと使用例は、[ベンダー・コンテナー](/docs/infrastructure/CDN?topic=CDN-vendor-container)で表示されます。

----
## アカウント用の API
{: #api-for-account}

### verifyCdnAccountExists
指定された `vendorName` について、API を呼び出しているユーザーの CDN アカウントが存在するかどうかを確認します。

* **必須パラメーター**: `vendorName`: 有効な CDN プロバイダーの名前を指定します。
* **戻り**: アカウントが存在する場合は `true`、存在しない場合は `false`。

----
## ドメイン・マッピング用の API
{: #api-for-domain-mapping}

### createDomainMapping
この関数は、提供された入力を使用して、指定のベンダー用のドメイン・マッピングを作成し、ユーザーの {{site.data.keyword.cloud_notm}} Infrastructure アカウント ID と関連付けます。CDN アカウントは、この API が動作するように、`placeOrder` を使用して最初に作成される必要があります ([コード例](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)の `placeOrder` API 呼び出しの例を参照してください。CDN が正常に作成されると、`defaultTTL` が 3600 秒の値で作成されます。

* **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
  入力コンテナーのすべての属性を次の場所で表示できます。

  [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

  次の属性は入力コンテナーの一部で、ドメイン・マッピングの作成時に指定できます (特に記載がない限り属性はオプションです)。
    * `vendorName`: **必須** 有効な IBM Cloud CDN プロバイダーの名前を指定します。
    * `origin`: **必須** オリジン・サーバー・アドレスを文字列として指定します。
    * `originType`: **必須** オリジン・タイプは `HOST_SERVER` または `OBJECT_STORAGE` の可能性があります。
    * `domain`: **必須** ホスト名を文字列として指定します。
    * `protocol`: **必須** サポート対象のプロトコルは、`HTTP`、`HTTPS`、または`HTTP_AND_HTTPS` です。
    * `certificateType`: HTTPS プロトコルの場合は**必須**。 `SHARED_SAN_CERT` または `WILDCARD_CERT`。
    * `path`: キャッシュに入れられたコンテンツの配信元のパス。 デフォルトのパスは `/*` です。
    * `httpPort` および/または `httpsPort`: (ホスト・サーバーでは**必須**) これらの 2 つのオプションは、要求されたプロトコルに対応している必要があります。 プロトコルが `HTTP` の場合は、`httpPort` が設定される必要があり、`httpsPort` は設定されては_なりません_。 同様に、プロトコルが `HTTPS` の場合は、`httpsPort` が設定される必要があり、`httpPort` は設定されては_なりません_。 プロトコルが `HTTP_AND_HTTPS` の場合は、`httpPort` と `httpsPort` の_両方_を設定する_必要があります_。 Akamai はポート番号に制限があります。 許可ポート番号については、[FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) を参照してください。
    * `header`: オリジン・サーバーによって使用されるホスト・ヘッダー情報を指定します。
    * `respectHeader`: `true` に設定された場合はオリジンの TTL 設定によって CDN TTL 設定がオーバーライドされるようになるブール値。
    * `cname`: ホスト名に別名を指定します。 指定しなかった場合は、生成されます。
    * `bucketName`: (オブジェクト・ストレージのみ**必須**) S3 オブジェクト・ストレージのバケット名。
    * `fileExtension`: (オブジェクト・ストレージのオプション) キャッシュに入れることができるファイル拡張子。
    * `cacheKeyQueryRule`: キャッシュ・キーの動作を構成するために以下のオプションを使用できます。 `cacheKeyQueryRule` 引数が指定されていない場合は、「include-all」がデフォルトになります。
      * `include-all` - すべての照会引数を含みます。**デフォルト**
      * `ignore-all` - すべての照会引数を無視します。
      * `ignore: space separated query-args` - スペースで区切られた特定の照会引数を無視します。 例えば、`ignore: query1 query2` です
      * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。 例えば、`include: query1 query2` です

* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション。

  **注**: このコレクションは `uniqueId` 値を提供します。この値を、マッピングとオリジン・パスに関連した後続の API 呼び出しの入力として送信する必要があります。

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
`uniqueId` に基づいてドメイン・マッピングを削除します。 ドメイン・マッピングの状態は、_RUNNING_、_STOPPED_、_DELETED_、_ERROR_、_CNAME_CONFIGURATION_、または _SSL_CONFIGURATION_ のいずれかでなければなりません。

* **必須パラメーター**: `uniqueId`: 削除するマッピングの固有 ID
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション
  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
CDN の状況を確認し、CDN マッピングの `status` が変更されていたら更新します。 CDN マッピングが最初に作成された時点では、状況は _CNAME_CONFIGURATION_ と表示されます。 この時点で、DNS レコードを更新して、CDN マッピングが CNAME のホスト名を指すようにする必要があります。 更新の方法および変更がインターネットに伝搬するのに要する時間については、DNS プロバイダーに確認してください。 通常は、15 分から 30 分かかります。 その時間が経過した後で、この `verifyDomainMapping` API を呼び出して、CNAME チェーンが完了したかどうかを確認する必要があります。 CNAME チェーンが完了すると、CDN マッピングの状況は _RUNNING_ に変更されます。

この API は、最新の CDN マッピング状況を取得するために、いつでも呼び出すことができます。 ドメイン・マッピングの状態は、_RUNNING_ または _CNAME_CONFIGURATION_ のいずれかでなければなりません。

* **必須パラメーター**: `uniqueId`: 検証するマッピングの固有 ID
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
`uniqueId` に基づいて CDN ドメイン・マッピングを開始します。 開始するには、ドメイン・マッピングが _STOPPED_ 状態でなければなりません。

* **必須パラメーター**: `uniqueId`: 開始するマッピングの固有 ID
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
`uniqueId` に基づいて CDN ドメイン・マッピングを停止します。 停止を開始するには、ドメイン・マッピングが _RUNNING_ 状態でなければなりません。

* **必須パラメーター**: `uniqueId`: 停止するマッピングの固有 ID
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
ユーザーが `uniqueId` で示されたマッピングのプロパティーを更新できるようにします。 変更できるフィールドは、`originHost`、`httpPort`、`httpsPort`、`respectHeader`、`header`、`cacheKeyQueryRule` 引数です。オリジン・タイプがオブジェクト・ストレージの場合は、`bucketName` と `fileExtension` も変更することができます。 更新を実行するには、ドメイン・マッピングが _RUNNING_ 状態でなければなりません。

* **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
  入力コンテナーのすべての属性を、
  [入力コンテナーの表示 (View the Input Container)](/docs/infrastructure/CDN?topic=CDN-input-container) で表示することができます。

  次の属性は入力コンテナーの一部で、ドメイン・マッピングの更新時に提供される**必要があります**。
    * `vendorName`: このマッピング用の CDN プロバイダーの名前を指定します。
    * `path`: このマッピングの現行パスを指定します。
    * `origin`: オリジン・サーバー・アドレスを文字列として指定します。
    * `originType`: オリジン・タイプは `HOST_SERVER` または `OBJECT_STORAGE` の可能性があります。
    * `domain`: ホスト名を指定します。
    * `protocol`: サポート対象のプロトコルは、`HTTP`、`HTTPS`、または `HTTP_AND_HTTPS` です。
    * `httpPort` および/または `httpsPort`: これらの 2 つのオプションは、要求されたプロトコルに対応する必要があります。 プロトコルが `HTTP` の場合は、`httpPort` が設定される必要があり、`httpsPort` は設定されては_なりません_。 同様に、プロトコルが `HTTPS` の場合は、`httpsPort` が設定される必要があり、`httpPort` は設定されては_なりません_。 プロトコルが `HTTP_AND_HTTPS` の場合は、`httpPort` と `httpsPort` の_両方_を設定する_必要があります_。 Akamai はポート番号に制限があります。 許可ポート番号については、[FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) を参照してください。
    * `header`: オリジン・サーバーによって使用されるホスト・ヘッダー情報を指定します。
    * `respectHeader`: `true` に設定された場合はオリジンの TTL 設定によって CDN TTL 設定がオーバーライドされるようになるブール値。
    * `uniqueId`: マッピングの作成後に生成されます。
    * `cname`: cname を指定します。 指定しなかった場合は、マッピングの作成時に作成されています。
    * `bucketName`: (オブジェクト・ストレージのみ**必須**) S3 オブジェクト・ストレージのバケット名。
    * `fileExtension`: (オブジェクト・ストレージのみ**必須**) キャッシュに入れることができるファイル拡張子。
    * `cacheKeyQueryRule`: キャッシュ・キーの動作ルールは、2017 年 11 月 16 日_以降に_作成された CDN マッピングに対してのみ更新することができます。 キャッシュ・キーの動作を構成するために以下のオプションを使用できます。
      * `include-all` - すべての照会引数を含みます。**デフォルト**
      * `ignore-all` - すべての照会引数を無視します。
      * `ignore: space separated query-args` - スペースで区切られた特定の照会引数を無視します。 例えば、`ignore: query1 query2` です
      * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。 例えば、`include: query1 query2` です
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション。

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
現行のカスタマーのすべてのドメイン・マッピングのコレクションを返します。

* **必須パラメーター**: なし
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
CDN の `uniqueId` に基づいて、単一のドメイン・オブジェクトを含むコレクションを返します。

* **必須パラメーター**: `uniqueId`: 返されるマッピングの固有 ID
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` の単一オブジェクト・コレクション

  [マッピング・コンテナーの表示 (View the Mapping Container)](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## オリジン用の API
{: #apis-for-origin}

### createOriginPath
既存の CDN および特定のお客様用のオリジン・パスを作成します。 オリジン・パスは、ホスト・サーバーまたはオブジェクト・ストレージに基づくことができます。 オリジン・パスを作成するには、ドメイン・マッピングの状態が _RUNNING_ または _CNAME_CONFIGURATION_ のいずれかでなければなりません。

* **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
  入力コンテナーのすべての属性を次の場所で表示できます。

  [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

  次の属性は入力コンテナーの一部で、オリジン・パスの作成時に提供される場合があります (特に記載がない限り属性はオプションです)。
    * `vendorName`: **必須** 有効な IBM Cloud CDN プロバイダーの名前を指定します。
    * `origin`: **必須** オリジン・サーバー・アドレスを文字列として指定します。
    * `originType`: **必須** オリジン・タイプは `HOST_SERVER` または `OBJECT_STORAGE` の可能性があります。
    * `domain`: **必須** ホスト名を文字列として指定します。
    * `protocol`: **必須** サポート対象のプロトコルは、`HTTP`、`HTTPS`、または`HTTP_AND_HTTPS` です。
    * `path`: キャッシュに入れられたコンテンツの配信元のパス。 マッピング・パスで始める必要があります。 例えば、マッピング・パスが `/test` の場合、オリジン・パスは `/test/media` になります。
    * `httpPort` および/または `httpsPort`: **必須** これらの 2 つのオプションは、要求されたプロトコルに対応する必要があります。 プロトコルが `HTTP` の場合は、`httpPort` が設定される必要があり、`httpsPort` は設定されては_なりません_。 同様に、プロトコルが `HTTPS` の場合は、`httpsPort` が設定される必要があり、`httpPort` は設定されては_なりません_。 プロトコルが `HTTP_AND_HTTPS` の場合は、`httpPort` と `httpsPort` の_両方_を設定する_必要があります_。 Akamai はポート番号に制限があります。 許可ポート番号については、[FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) を参照してください。
    * `header`: オリジン・サーバーによって使用されるホスト・ヘッダー情報を指定します。
    * `uniqueId`: **必須** マッピングの作成後に生成されます。
    * `cname`: ホスト名に別名を指定します。 固有の cname を指定しなかった場合は、マッピングの作成時に生成されています。
    * `bucketName`: (オブジェクト・ストレージは**必須**) S3 オブジェクト・ストレージのバケット名。
    * `fileExtension`: (オブジェクト・ストレージのオプション) キャッシュに入れることができるファイル拡張子。
    * `cacheKeyQueryRule`: キャッシュ・キーの動作を構成するために以下のオプションを使用できます。
      * `include-all` - すべての照会引数を含みます。**デフォルト**
      * `ignore-all` - すべての照会引数を無視します。
      * `ignore: space separated query-args` - スペースで区切られた特定の照会引数を無視します。 例えば、`ignore: query1 query2` です
      * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。 例えば、`include: query1 query2` です

* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` のコレクション

  [オリジン・パス・コンテナーの表示 (View the Origin Path Container)](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
既存のマッピングおよび特定のお客様用の既存のオリジン・パスを更新します。 オリジン・タイプはこの API では変更できません。 変更できるプロパティーは `path`、`origin`、`httpPort` と `httpsPort`、`header`、および `cacheKeyQueryRule` 引数です。 更新するには、ドメイン・マッピングの状態が _RUNNING_ または _CNAME_CONFIGURATION_ のいずれかでなければなりません。

* **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
  入力コンテナーのすべての属性を次の場所で表示できます。

  [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

  次の属性は入力コンテナーの一部で、オリジン・パスの更新時に提供される場合があります (特に記載がない限り属性はオプションです)。
    * `oldPath`: **必須** 変更する現行パス。
    * `origin`: (更新中の場合は**必須**) オリジン・サーバー・アドレスを文字列として指定します。
    * `originType`: **必須** オリジン・タイプは `HOST_SERVER` または `OBJECT_STORAGE` の可能性があります。
    * `path`: **必須** 追加する新規のパス。 マッピング・パスを基準とした相対パス。
    * `httpPort` および/または `httpsPort`: (更新中の場合はホスト・サーバーでは**必須**) これらの 2 つのオプションは、要求されたプロトコルに対応している必要があります。 プロトコルが `HTTP` の場合は、`httpPort` が設定される必要があり、`httpsPort` は設定されては_なりません_。 同様に、プロトコルが `HTTPS` の場合は、`httpsPort` が設定される必要があり、`httpPort` は設定されては_なりません_。 プロトコルが `HTTP_AND_HTTPS` の場合は、`httpPort` と `httpsPort` の_両方_を設定する_必要があります_。 Akamai はポート番号に制限があります。 許可ポート番号については、[FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) を参照してください。
    * `uniqueId`: **必須**: このオリジンが属するマッピングの固有 ID
    * `bucketName`: (オブジェクト・ストレージのみ**必須**) S3 オブジェクト・ストレージのバケット名。
    * `fileExtension`: (オブジェクト・ストレージのみ**必須**) キャッシュに入れることができるファイル拡張子。
    * `cacheKeyQueryRule`: (更新中の場合は**必須**) キャッシュ・キーの動作ルールは、2017 年 11 月 16 日_以降に_作成されたオリジン・パスに対してのみ更新することができます。キャッシュ・キーの動作を構成するために以下のオプションを使用できます。
      * `include-all` - すべての照会引数を含みます。**デフォルト**
      * `ignore-all` - すべての照会引数を無視します。
      * `ignore: space separated query-args` - スペースで区切られた特定の照会引数を無視します。 例えば、`ignore: query1 query2` です
      * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。 例えば、`include: query1 query2` です

* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` のコレクション

  [オリジン・パス・コンテナーの表示 (View the Origin Path Container)](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
既存の CDN および特定のお客様用の既存のオリジン・パスを削除します。 削除するには、ドメイン・マッピングの状態が _RUNNING_ または _CNAME_CONFIGURATION_ のいずれかでなければなりません。

* **必須パラメーター**:
  * `uniqueId`: このオリジン・パスが属するマッピングの固有 ID
  * `path`: 削除するパス

* **戻り**: 削除に成功した場合は状況メッセージ、それ以外の場合は、例外がスローされます。

----
### listOriginPath
`uniqueId` に基づいて既存のマッピングに対するオリジン・パスをリストします。

* **必須パラメーター**:
  * `uniqueId`: オリジン・パスをリスト表示するマッピングの固有 ID を指定します。
* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` のオブジェクトのコレクション

  [オリジン・パス・コンテナーの表示 (View the Origin Path Container)](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## パージ用の API
{: #api-for-purge}

### パージするコンテナーのクラス:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
パージ・レコードを作成して、データベースに挿入します。

* **パラメーター**:
  * `uniqueId`: パージが作成されるマッピングの固有 ID
  * `path`: 作成されるパージのパス

* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` のコレクション

----
### getPurgeHistoryPerMapping
`uniqueId` と `saved` 状況に基づいて、CDN のパージ履歴を返します (デフォルトでは、`saved` の値は _UNSAVED_ に設定されています。)

* **パラメーター**:
  * `uniqueId`: パージ履歴を取り出すマッピングの固有 ID
  * `saved`: _SAVED_ または _UNSAVED_ されたパージを返します。

* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` のコレクション

----
### saveOrUnsavePurgePath
パージ・パス項目の状況を更新して、そのパージ・パスを保存する必要があるかどうかを示します。 パージ・パスが保存されている場合、新規 `saved` パージを作成します。 パスが `unsaved` の場合、保存されたパージ・レコードを削除します。

* **パラメーター**:
  * `uniqueId`: パージが属するマッピングの固有 ID
  * `path`: 保存されるまたは保存されないパージのパス
  * `saved`: _SAVED_ または _UNSAVED_

* **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` のコレクション

----
## 存続時間用の API
{: #api-for-time-to-live}

### TimeToLive クラス変数:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
新規 `TimeToLive` オブジェクトを作成して、データベースに挿入します。

 * **パラメーター**: `string` `uniqueId`、`string` `path`、`int` `ttl`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` のオブジェクト
___
### updateTtl
既存の `TimeToLive` オブジェクトを更新します。 _oldTtl_ と _newTtl_ の入力が等しい場合は、早く終了します。

 * **パラメーター**: `string` `uniqueId`、`string` `oldPath`、`string` `newPath`、`int` `oldTtl`、`int` `newTtl`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` のオブジェクト
___
### deleteTtl
既存の `TimeToLive` オブジェクトをデータベースから削除します。

 * **パラメーター**: `string` `uniqueId`、`string` `pathName`
 * **戻り**: 削除の状況を含むストリング
___
### listTtl
CDN の `uniqueId` に基づいて、既存の `TimeToLive` オブジェクトをリストします。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` のオブジェクトの配列

 ----
## メトリック用の API
{: #api-for-metrics}

[メトリック・コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
指定された期間について、お客様のアカウントに関して直接表示 (グラフなし) するための事前定義された統計の総数を返します。

 * **パラメーター**:
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingUsageMetrics
指定されたマッピングについて直接表示するための事前定義された統計の総数を返します。 `frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingHitsMetrics
ドメイン・マッピングごとに、指定された時間範囲について特定の頻度でヒットの総数を返します。 頻度には「day」、「week」、および「month」を指定でき、各間隔がグラフの 1 つのプロット・ポイントになります。 戻りデータは、`startDate`、`endDate`、および `frequency` に基づいて配列されます。 `frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingHitsByTypeMetrics
指定された時間範囲について特定の頻度でヒットの総数を返します。 頻度には「day」、「week」、および「month」を指定でき、各間隔がグラフの 1 つのプロット・ポイントになります。 戻りデータは、`startDate`、`endDate`、および `frequency` に基づいて配列される必要があります。 `frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingBandwidthMetrics
個々の CDN のエッジ・ヒットの数を返します。 各マッピングで、地域はベンダーごとに異なっても構いません。

 * **パラメーター**:  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingBandwidthByRegionMetrics
指定された期間について、指定のマッピングに関する直接表示 (グラフなし) 用の事前定義された統計の総数を返します。 `frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション

----
## 地理的アクセス制御用の API
{: #api-for-geographical-access-control}

### createGeoblocking
新しい地理的アクセス制御のルールを作成し、新しく作成されたルールを返します。

  * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
    入力コンテナーのすべての属性を次の場所で表示できます。

    [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下の属性は入力コンテナーの一部であり、新しい地理的アクセス制御のルールを作成するときは**必須**です。
    * `uniqueId`: ルールを割り当てるマッピングの固有 ID
    * `accessType`: ルールが指定地域へのトラフィックを許可するか (`ALLOW`)、拒否するか (`DENY`) を指定します。
    * `regionType`: 地理的アクセス制御のルールを適用する地域のタイプ。`CONTINENT` または `COUNTRY_OR_REGION` のいずれか。
    * `regions`: `accessType` が適用されるロケーションをリストした配列

      指定可能な地域のリストを確認するには、[`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) ページを参照してください。

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` のオブジェクト

    [ジオブロッキング・クラスの表示](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
既存ドメイン・マッピングの既存の地理的アクセス制御のルールを更新し、更新されたルールを返します。

  * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
    入力コンテナーのすべての属性を次の場所で表示できます。

    [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

    次の属性は入力コンテナーの一部であり、地理的アクセス制御のルールを更新するときに指定できます (特に記載がない限りパラメーターはオプションです)。
    * `uniqueId`: **必須**。更新対象のルールが属するマッピングの固有 ID
    * `accessType`: ルールが指定地域へのトラフィックを許可するか (`ALLOW`)、拒否するか (`DENY`) を指定します。
    * `regionType`: ルールを適用する地域のタイプ。`CONTINENT` または `COUNTRY_OR_REGION` のいずれか。
    * `regions`: `accessType` が適用されるロケーションをリストした配列

      指定可能な地域のリストを確認するには、[`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) ページを参照してください。

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` のオブジェクト

    [ジオブロッキング・クラスの表示](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
既存のドメイン・マッピングから既存の地理的アクセス制御のルールを削除します。

  * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
    入力コンテナーのすべての属性を次の場所で表示できます。

    [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下の属性は入力コンテナーの一部であり、地理的アクセス制御のルールを削除するときは**必須**です。
    * `uniqueId`: 削除するルールが属するマッピングの固有 ID を指定します。

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` のオブジェクト

    [ジオブロッキング・クラスの表示](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
マッピングの地理的アクセス制御の動作をデータベースから取得します。

  * **パラメーター**:
    * `uniqueId`: ルールが属するマッピングの固有 ID。

  * **戻り**: タイプ
         `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` のオブジェクト

    [ジオブロッキング・クラスの表示](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
地理的アクセス制御のルールの作成が許可されるタイプおよび地域のリストを返します。

  * **パラメーター**:
    * `uniqueId`: ドメイン・マッピングの固有 ID。

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type` のオブジェクト

    [ジオブロッキング・クラスの表示](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## ホット・リンク保護用の API
{: #api-for-hotlink-protection}

### createHotlinkProtection
新しいホット・リンク保護を作成し、新しく作成された動作を返します。

  * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
    入力コンテナーのすべての属性を次の場所で表示できます。

    [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下の属性は入力コンテナーの一部であり、新しいホット・リンク保護を作成するときは**必須**です。
    * `uniqueId`: 動作を割り当てるマッピングの固有 ID
    * `protectionType`: 指定された refererValues 内にあるいずれかの語に一致する Referer ヘッダー値が含まれているコンテンツを Web ページが要求したときに、そのコンテンツへのアクセスを許可する (ALLOW) か、拒否する (DENY) かを指定します
    * `refererValues`: `protectionType` の動作が有効になるリファラー URL 一致語のスペース区切りリスト

      有効なホット・リンク保護値のリストを確認するには、[`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) ページを参照してください。

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` のオブジェクト

    [ホット・リンク保護クラスの表示](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
既存ドメイン・マッピングの既存のホット・リンク保護の動作を更新し、更新された動作を返します。

  * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
    入力コンテナーのすべての属性を次の場所で表示できます。

    [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下の属性は入力コンテナーの一部であり、既存のホット・リンク保護を更新するときは**必須**です。
    * `uniqueId`: 既存の動作が属するマッピングの固有 ID
    * `protectionType`: 指定された refererValues 内にあるいずれかの語に一致する Referer ヘッダー値が含まれているコンテンツを Web ページが要求したときに、そのコンテンツへのアクセスを許可する (ALLOW) か、拒否する (DENY) かを指定します 
    * `refererValues`: `protectionType` の動作が有効になるリファラー URL 一致語のスペース区切りリスト

      有効なホット・リンク保護値のリストを確認するには、[`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) ページを参照してください。

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` のオブジェクト

    [ホット・リンク保護クラスの表示](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
既存のドメイン・マッピングから既存のホット・リンク保護の動作を削除します。

  * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。
    入力コンテナーのすべての属性を次の場所で表示できます。

    [入力コンテナーの概要](/docs/infrastructure/CDN?topic=CDN-input-container)

    以下の属性は入力コンテナーの一部であり、新しいホット・リンク保護を作成するときは**必須**です。
    * `uniqueId`: 動作を削除するマッピングの固有 ID

  * **戻り**: null

----
### getHotlinkProtection
マッピングの現在のホット・リンク保護の動作を取得します。

  * **パラメーター**:
    * `uniqueId`: 動作が属するマッピングの固有 ID

  * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` のオブジェクト

    [ホット・リンク保護クラスの表示](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
