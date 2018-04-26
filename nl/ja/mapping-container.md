---

copyright:
  years: 2017
lastupdated: "2017-11-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# マッピング・コンテナー  
`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` コレクションには、マッピング API によって使用される属性が含まれています。それぞれのマッピング API がこのタイプのコレクションを返します。

クラス `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: 有効な IBM Cloud CDN プロバイダーの名前。
* `uniqueId`: 各マッピングに固有の、システムによって生成された 10 桁の ID。マッピングの作成時に生成されます。
* `domain`: 1 次 CDN 名。`hostname` とも呼ばれます。
* `protocol`: サービスのセットアップに使用されるプロトコル。HTTP、HTTPS、またはこれらの 2 つの組み合わせ (HTTP_AND_HTTPS) を指定できます。
* `originType`: オリジン・ホストのタイプ。現在、「HOST_SERVER」または「OBJECT_STORAGE」です。
* `originHost`: オリジン・サーバー・アドレス (オリジン・サーバーのホスト名または IPv4 アドレスのいずれか)。これは、コンテンツのフェッチ元のエンドポイントまたはコンテンツが保管されているバケットの名前です。511 文字未満でなければなりません。
* `httpPort`: HTTP プロトコルに使用されるポートの番号。Akamai には、HTTP ポートと HTTPS ポートのポート番号に関する特定の制限があります。許可されるポート番号については、[よくある質問](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。
* `httpsPort`: HTTPS プロトコルに使用されるポートの番号。Akamai には、HTTP ポートと HTTPS ポートのポート番号に関する特定の制限があります。許可されるポート番号については、[よくある質問](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。
* `cname`: ホスト名の別名である正規名レコード。ユーザーが指定することも、システムによって生成することもできます。ユーザー指定の場合、64 文字未満の英数字で、固有でなければなりません。ユーザーが指定しない場合、マッピングの作成時に生成されます。
* `respectHeaders`: 「true」に設定した場合、オリジンの TTL 設定によって CDN TTL 設定がオーバーライドされるようになるブール値。
* `header`: オリジン・サーバーによって使用されるホスト・ヘッダー情報を指定します。
* `status`: マッピングの状況。状況は、CNAME_CONFIGURATION、SSL_CONFIGURATION、RUNNING、STOPPED、DELETED、または ERROR になります。
* `bucketName`: S3 オブジェクト・ストレージのバケット名。
* `fileExtension`: キャッシュ可能なファイル拡張子。
* `path`: S3 オブジェクト・ストレージへのパス。通常は、オブジェクト・ストア URL か、サービスの API セクションにあります。
* `cacheKeyQueryRule`: キャッシュ・キーの動作を構成するために以下のオプションを使用できます。
  * `include-all`: すべての照会引数を含めます。**デフォルト**です
  * `ignore-all`: すべての照会引数を無視します
  * `ignore: space separated query-args`: スペースで区切られた特定の照会引数を無視します。例えば、「ignore: query1 query2」です
  * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。例えば、「include: query1 query2」です

特に注意すべきなのは、マッピングの作成時に生成され、後続の API 呼び出しに対するパラメーターとして使用される `uniqueId` です。

例えば、`listDomainMappingByUniqueid` に対する次の呼び出しがあるとします。  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid(d'750352919747xxx');
```

これは、次のようなオブジェクトを返します。

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    [vendorName] => akamai
                    [uniqueId] => 750352919747xxx
                    [domain] => test.testingcdn.net
                    [protocol] => HTTP_AND_HTTPS
                    [originType] => HOST_SERVER
                    [originHost] => test.testingcdn.net
                    [httpPort] => 80
                    [httpsPort] => 443
                    [cname] => test.cdnedge.bluemix.net
                    [performanceConfiguration] =>
                    [certificateType] =>
                    [respectHeaders] => 1
                    [header] =>
                    [status] => RUNNING
                    [bucketName] =>
                    [fileExtension] =>
                    [path] => /
                    [cacheKeyQueryRule] => include-all
                )
```


