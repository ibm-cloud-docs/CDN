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

# API 參考資料

如需 PHP 及 SOAP 相關配置的相關資訊，請造訪 https://sldn.softlayer.com/article/PHP。 
 
## 帳戶的 API
### verifyCdnAccountExists
確認呼叫 API 的使用者是否有給定 `vendorName` 的 CDN 帳戶

 * **參數**：`vendorName`
 * **傳回**：如果帳戶存在，則為 `true`，否則為 `false`
___

## 網域對映的 API
### createDomainMapping
使用提供的輸入，此功能會建立給定供應商的網域對映，並建立它與使用者的「{{site.data.keyword.BluSoftlayer_notm}} 帳戶 ID」的關聯。必須使用 `createCustomerSubAccount` 建立 CDN 帳戶，此 API 才會運作。順利建立 CDN 之後，會建立值為 3600 秒的 `defaultTTL`。

 * **參數**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 的集合。此集合應該包括下列項目：`vendorName`、`hostname`、`protocol`、`originType`、`originHost`、`originHostPort`、`respectHeader`、`serveStale`、`cname`、`performanceConfiguration`、`header`、`certificateType`、`path`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合。此集合提供 `uniqueId` 值，需要傳送此值作為對映相關之後續 API 呼叫的輸入。
___ 
### deleteDomainMapping
根據 `uniqueId`，刪除網域對映。網域對映必須處於下列其中一種狀態：_RUNNING_、_STOPPED_、_DELETED_、_ERROR_、_CNAME\_CONFIGURATION_ 或 _SSL\_CONFIGURATION_。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的集合
___
### verifyDomainMapping
驗證 CDN 的狀態，並更新 `status`、`cname` 及（或）`vendorCname`（必要的話）。傳回已更新值（如果適用的話）。網域對映必須處於下列其中一種狀態：_RUNNING_、_CNAME\_CONFIGURATION_ 或 _SSL\_CONFIGURATION_。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的集合
___ 
### startDomainMapping
根據 `uniqueId`，啟動 CDN 網域對映。若要啟動，網域對映必須處於 _STOPPED_ 狀態。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的集合
___ 
### stopDomainMapping
根據 `uniqueId`，停止 CDN 網域對映。若要起始停止，網域對映必須處於 _RUNNING_ 狀態。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的集合
___ 
### updateDomainMapping
讓使用者更新 `uniqueId` 所識別對映的內容。下列欄位可能已變更：`originHost`、`performanceConfiguration`、`header`、`httpPort`、`httpsPort`、`certificateType`、`respectHeader`、`serveStale`、`path`，而且，如果您路徑的原始類型是 Object Storage，則也可能會變更 `bucketName` 及 `fileExtension`。若要進行更新，網域對映必須處於 _RUNNING_ 狀態。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的集合
___
### listDomainMappings
傳回特定客戶的所有網域的集合。

 * **參數**：_none_ 
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的物件集合
___ 
### listDomainMappingByUniqueId
根據 CDN 的 `uniqueId`，傳回具有單一網域物件的集合。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 的物件的單一物件集合
___

## 要清除的 API
### createPurge
建立清除記錄，並將它插入資料庫。

 * **參數**：`string` `uniqueId`、`string` `path`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 的物件集合
___
### getPurgeHistoryPerMapping
根據 `uniqueId` 及 `saved` 狀態，傳回 CDN 的清除歷程（依預設，`saved` 的值會設為 _unsaved_）。

 * **參數**：`string` `uniqueId`、`int` `saved`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 的物件集合
___
### saveOrUnsavePurgePath
更新清除路徑項目的狀態，以指出是否應該儲存該清除路徑。如果儲存清除路徑，則請建立新的 `saved` 清除。如果路徑是 `unsaved`，則請刪除已儲存的清除記錄。

 * **參數**：`string` `uniqueId`、`string` `path`、`int` `saved`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 的物件集合
___

## 存活時間的 API
### createTimeToLive
建立新的 `TimeToLive` 物件，並將它插入資料庫。

 * **參數**：`string` `uniqueId`、`string` `path`、`int` `ttl`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` 的物件
___
### updateTtl
更新現有 `TimeToLive` 物件。如果 _oldTtl_ 與 _newTtl_ 輸入相等，則會過早結束。

 * **參數**：`string` `uniqueId`、`string` `oldPath`、`string` `newPath`、`int` `oldTtl`、`int` `newTtl`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` 的物件
___
### deleteTtl
刪除資料庫中的現有 `TimeToLive` 物件。

 * **參數**：`string` `uniqueId`、`string` `pathName`
 * **傳回**：具有刪除狀態的字串
___
### listTtl
根據 CDN 的 `uniqueId`，列出現有 `TimeToLive` 物件。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive` 的物件陣列
___

## 原始的 API
### createOriginPath
建立現有 CDN 及特定客戶的「原始路徑」。「原始路徑」可以根據主伺服器或 Object Storage。若要建立「原始路徑」，網域對映必須處於 _RUNNING_ 或 _CNAME\_CONFIGURATION_ 狀態。  

 * **參數：**：預期設定下列內容的 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 物件：`domainName`、`vendorName`、`path`、`originType` 及 `origin`。如果原始類型是伺服器，則也必須設定 `httpPort` 及（或）`httpsPort`。如果原始類型是 Object Storage，則也必須提供 `bucketName` 以及選用的 `fileExtension`。  
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 的物件集合

___ 
### updateOrigin
更新現有對映及特定客戶的現有「原始路徑」。無法變更 `originType`。您只能變更下列內容：`path`、`origin`、`httpPort` 及 `httpsPort`。若要更新，網域對映必須處於 _RUNNING_ 或 _CNAME\_CONFIGURATION_ 狀態。

 * **參數：**：預期設定下列內容的 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 物件：`domainName`、`vendorName`、`path`、`originType` 及 `origin`。如果原始類型是伺服器，則也必須設定 `httpPort` 及（或）`httpsPort`。如果路徑的原始類型是 Object Storage，則必須提供 `bucketName`，並可能選擇性地提供 `fileExtension`。  
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 的物件集合
___ 
### deleteOriginPath
刪除現有 CDN 及特定客戶的現有「原始路徑」。若要刪除，網域對映必須處於 _RUNNING_ 或 _CNAME\_CONFIGURATION_ 狀態。  

 * **參數**：`string` `uniqueId`、`string` `path`
 * **傳回**：如果刪除成功，則為狀態訊息，否則為異常狀況

___
### listOriginPath
列出現有對映及特定客戶的原始路徑。

 * **參數**：`string` `uniqueId`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 的物件集合
___

## 度量值的 API
### getCustomerUsageMetrics
傳回客戶帳戶在一段指定期間的直接顯示畫面（沒有圖形）的預定統計資料總數。

 * **參數**：`string` `vendorName`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___ 
### getMappingUsageMetrics
傳回給定對映的直接顯示畫面的預定統計資料總數。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___ 
### getMappingHitsMetrics
傳回每個網域對映在給定時間範圍內依特定頻率的命中總數。頻率可以是 'day'、'week' 及 'month'，而每一個間隔都是圖形的一個圖點。根據 `startDate`、`endDate` 及 `frequency`，排序傳回資料。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingHitsByTypeMetrics
傳回給定時間範圍內依特定頻率的命中總數。頻率可以是 'hour'、'day'、'week' 及 'month'，而每一個間隔都是圖形的一個圖點。根據 `startDate`、`endDate` 及 `frequency`，必須排序傳回資料。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingBandwidthMetrics
傳回個別 CDN 的每個地區的邊緣命中數。每一個供應商的地區可能會不同。每個對映。

 * **參數**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingBandwidthByRegionMetrics
傳回客戶帳戶在一段指定期間的直接顯示畫面（沒有圖形）的預定統計資料總數。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：`string` `mappingUniqueId`、`int` `startDate`、`int` `endDate`、`string` `frequency`
 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
