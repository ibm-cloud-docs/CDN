---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# API リファレンス

構成に関連した PHP および SOAP についての詳細情報は、https://sldn.softlayer.com/article/PHP をご覧ください。 
 
## アカウント用の API
### verifyCdnAccountExists
指定された `vendorName` について、API を呼び出しているユーザーの CDN アカウントが存在するかどうかを確認します。

 * **パラメーター**: `vendorName`
 * **戻り**: アカウントが存在する場合は `true`、存在しない場合は `false`
___

## ドメイン・マッピング用の API
### createDomainMapping
この関数は、提供された入力を使用して、指定のベンダー用のドメイン・マッピングを作成し、ユーザーの {{site.data.keyword.BluSoftlayer_notm}} アカウント ID と関連付けます。この API が機能するには、`createCustomerSubAccount` を使用して CDN アカウントを作成する必要があります。CDN が正常に作成されると、`defaultTTL` が 3600 秒の値で作成されます。

 * **パラメーター**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` のコレクション。このコレクションには、項目として `vendorName`、`hostname`、`protocol`、`originType`、`originHost`、`originHostPort`、`respectHeader`、`serveStale`、`cname`、`performanceConfiguration`、`header`、`certificateType`、`path` が含まれている必要があります。
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` のコレクション。このコレクションは `uniqueId` 値を提供します。この値を、マッピングに関連した後続の API 呼び出しの入力として送信する必要があります。
___ 
### deleteDomainMapping
`uniqueId` に基づいてドメイン・マッピングを削除します。ドメイン・マッピングの状態は、_RUNNING_、_STOPPED_、_DELETED_、_ERROR_、_CNAME\_CONFIGURATION_、または _SSL\_CONFIGURATION_ のいずれかでなければなりません。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のコレクション
___
### verifyDomainMapping
CDN の状況を確認し、必要に応じて `status`、`cname`、または `vendorCname` (あるいは、そのすべて) を更新します。該当する場合、更新された値を返します。ドメイン・マッピングの状態は、_RUNNING_、_CNAME\_CONFIGURATION_、または _SSL\_CONFIGURATION_ のいずれかでなければなりません。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のコレクション
___ 
### startDomainMapping
`uniqueId` に基づいて CDN ドメイン・マッピングを開始します。開始するには、ドメイン・マッピングが _STOPPED_ 状態でなければなりません。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のコレクション
___ 
### stopDomainMapping
`uniqueId` に基づいて CDN ドメイン・マッピングを停止します。停止を開始するには、ドメイン・マッピングが _RUNNING_ 状態でなければなりません。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のコレクション
___ 
### updateDomainMapping
ユーザーが `uniqueId` で示されたマッピングのプロパティーを更新できるようにします。変更できるフィールドは `originHost`、`performanceConfiguration`、`header`、`httpPort`、`httpsPort`、`certificateType`、`respectHeader`、`serveStale`、`path` です。また、パスのオリジン・タイプがオブジェクト・ストレージの場合は、`bucketName` と `fileExtension` も変更できます。更新を実行するには、ドメイン・マッピングが _RUNNING_ 状態でなければなりません。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のコレクション
___
### listDomainMappings
特定のお客様のすべてのドメインのコレクションを返します。

 * **パラメーター**: _なし_ 
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のオブジェクトのコレクション
___ 
### listDomainMappingByUniqueId
CDN の `uniqueId` に基づいて、単一のドメイン・オブジェクトを含むコレクションを返します。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` のオブジェクトの単一オブジェクト・コレクション
___

## パージ用の API
### createPurge
パージ・レコードを作成して、データベースに挿入します。

 * **パラメーター**: `string` `uniqueId`、`string` `path`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` のオブジェクトのコレクション
___
### getPurgeHistoryPerMapping
`uniqueId` と `saved` 状況に基づいて、CDN のパージ履歴を返します。(デフォルトでは、`saved` の値は _unsaved_ に設定されます。)

 * **パラメーター**: `string` `uniqueId`、`int` `saved`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` のオブジェクトのコレクション
___
### saveOrUnsavePurgePath
パージ・パス項目の状況を更新して、そのパージ・パスを保存する必要があるかどうかを示します。パージ・パスが保存されている場合、新規 `saved` パージを作成します。パスが `unsaved` の場合、保存されたパージ・レコードを削除します。

 * **パラメーター**: `string` `uniqueId`、`string` `path`、`int` `saved`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` のオブジェクトのコレクション
___

## 存続時間用の API
### createTimeToLive
新規 `TimeToLive` オブジェクトを作成して、データベースに挿入します。

 * **パラメーター**: `string` `uniqueId`、`string` `path`、`int` `ttl`
 * **戻り**: タイプ `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` のオブジェクト
___
### updateTtl
既存の `TimeToLive` オブジェクトを更新します。_oldTtl_ と _newTtl_ の入力が等しい場合は、早く終了します。

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
___

## オリジン用の API
### createOriginPath
既存の CDN および特定のお客様用のオリジン・パスを作成します。オリジン・パスは、ホスト・サーバーまたはオブジェクト・ストレージに基づくことができます。オリジン・パスを作成するには、ドメイン・マッピングの状態が _RUNNING_ または _CNAME\_CONFIGURATION_ のいずれかでなければなりません。  

 * **パラメーター**: `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` オブジェクトで、プロパティー `domainName`、`vendorName`、`path`、`originType`、および `origin` が設定されているものと想定されます。オリジン・タイプがサーバーの場合は、`httpPort` または `httpsPort` (あるいは、その両方) も設定されている必要があります。オリジン・タイプがオブジェクト・ストレージの場合は、`bucketName` も指定されている必要があり、オプションとして `fileExtension` を指定できます。  
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` のオブジェクトのコレクション

___ 
### updateOrigin
既存のマッピングおよび特定のお客様用の既存のオリジン・パスを更新します。`originType` は変更できません。変更できるプロパティーは `path`、`origin`、`httpPort`、および `httpsPort` のみです。更新するには、ドメイン・マッピングの状態が _RUNNING_ または _CNAME\_CONFIGURATION_ のいずれかでなければなりません。

 * **パラメーター**: `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` オブジェクトで、プロパティー `domainName`、`vendorName`、`path`、`originType`、および `origin` が設定されているものと想定されます。オリジン・タイプがサーバーの場合、`httpPort` または `httpsPort` (あるいは、その両方) も設定されている必要があります。パスのオリジン・タイプがオブジェクト・ストレージの場合、`bucketName` が指定されている必要があり、オプションとして `fileExtension` を指定できます。  
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` のオブジェクトのコレクション
___ 
### deleteOriginPath
既存の CDN および特定のお客様用の既存のオリジン・パスを削除します。削除するには、ドメイン・マッピングの状態が _RUNNING_ または _CNAME\_CONFIGURATION_ のいずれかでなければなりません。  

 * **パラメーター**: `string` `uniqueId`、`string` `path`
 * **戻り**: 削除に成功した場合は状況メッセージ、それ以外の場合は、例外

___
### listOriginPath
既存のマッピングおよび特定のお客様用のオリジン・パスをリストします。

 * **パラメーター**: `string` `uniqueId`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` のオブジェクトのコレクション
___

## メトリック用の API
### getCustomerUsageMetrics
指定された期間について、お客様のアカウントに関して直接表示 (グラフなし) するための事前定義された統計の総数を返します。

 * **パラメーター**: `string` `vendorName`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___ 
### getMappingUsageMetrics
指定されたマッピングについて直接表示するための事前定義された統計の総数を返します。`frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**: `string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___ 
### getMappingHitsMetrics
ドメイン・マッピングごとに、指定された時間範囲について特定の頻度でヒットの総数を返します。頻度には「day」、「week」、および「month」を指定でき、各間隔がグラフの 1 つのプロット・ポイントになります。戻りデータは、`startDate`、`endDate`、および `frequency` に基づいて配列されます。`frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**: `string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingHitsByTypeMetrics
指定された時間範囲について特定の頻度でヒットの総数を返します。頻度には「hour」、「day」、「week」、および「month」を指定でき、各間隔がグラフの 1 つのプロット・ポイントになります。戻りデータは、`startDate`、`endDate`、および `frequency` に基づいて配列される必要があります。`frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**: `string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingBandwidthMetrics
個々の CDN について、地域別のエッジ・ヒットの数を返します。各マッピングで、地域はベンダーごとに異なっても構いません。

 * **パラメーター**: `string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
### getMappingBandwidthByRegionMetrics
指定された期間について、お客様のアカウントに関して直接表示 (グラフなし) するための事前定義された統計の総数を返します。`frequency` の値は、デフォルトでは「aggregate」です。

 * **パラメーター**: `string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **戻り**: タイプ `SoftLayer_Container_Network_CdnMarketplace_Metrics` のオブジェクトのコレクション
___
