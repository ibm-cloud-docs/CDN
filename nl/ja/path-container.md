---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: origin, path, container, collection, attributes, query, arguments, class, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# パス (オリジン) コンテナー
{: #path-origin-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` コレクションには、(オリジン) パス API によって使用される属性が含まれています。 それぞれのパス API がこのタイプのコレクションを返します。

**クラス** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: このオリジン・パスが属するマッピングの固有 ID。  
* `path`: このオリジンに到達するために使用できるドメインを基準とした相対パス。  
* `originType`: オリジン・ホストのタイプ。現在、「HOST\_SERVER」または「OBJECT\_STORAGE」です。  
* `httpPort`: HTTP プロトコルに使用されるポートの番号。  
* `httpsPort`: HTTPS プロトコルに使用されるポートの番号。  
* `status`: (オリジン) パスの状況。 有効な状況は _CREATING_、_STARTING_、_RUNNING_、_UPDATING_、_STOPPING_、および _DELETING_ です。
* `fileExtension`: キャッシュ可能なファイル拡張子。  
* `header`: オリジン・サーバーによって使用されるホスト・ヘッダー情報を指定します。
* `cacheKeyQueryRule`: キャッシュ・キーの動作を構成するために以下のオプションを使用できます。
  * `include-all`: すべての照会引数を含めます。**デフォルト**です
  * `ignore-all`: すべての照会引数を無視します
  * `ignore: space separated query-args`: スペースで区切られた特定の照会引数を無視します。 例えば、「ignore: query1 query2」です
  * `include: space separated query-args`: スペースで区切られた特定の照会引数を含めます。 例えば、「include: query1 query2」です
