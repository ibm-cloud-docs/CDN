---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}



# CDN API 參考資料

{{site.data.keyword.BluSoftlayer_notm}} 應用程式設計介面（一般稱為 SLAPI）由 IBM Cloud 提供，它是讓開發人員及系統管理者能直接與 {{site.data.keyword.BluSoftlayer_notm}} 後端系統互動的開發介面。

SLAPI 實作客戶入口網站中的許多特性：如果能在客戶入口網站中進行互動，便也能在 SLAPI 中達成。因為您可以在 SLAPI 內用程式設計方式與 {{site.data.keyword.BluSoftlayer_notm}} 環境的所有部分互動，所以也可以用 API 來將作業自動化。

SLAPI 是遠端程序呼叫 (RPC) 系統。每個呼叫包含傳送資料給 API 端點，以及接收傳回的結構化資料。透過 SLAPI 傳送及接收資料時使用何種格式，視您選擇哪一個 API 實作而定。SLAPI 目前使用 SOAP、XML-RPC 或 REST 來進行資料傳輸。

如需 SLAPI 的相關資訊，或 IBM Cloud Content Delivery Network (CDN) 服務 API 的相關資訊，請參閱 IBM Cloud Development Network 中的下列資源：

* [SLAPI Overview](https://softlayer.github.io/ )
* [Getting Started with SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API Guide](https://softlayer.github.io/article/PHP/ )

----

若要開始使用，以下是建議遵循的 API 呼叫順序：
* `listVendors` - 提供支援供應商的清單
* `verifyOrder` - 驗證是否可以下單
* `placeOrder`  - 以給定的供應商建立 CDN 帳戶。成功呼叫 placeOrder 之後可以建立最多 10 個 CDN 對映。
* `createDomainMapping` - 建立 CDN 對映
* `verifyDomainMapping` - 將 CDN 狀態變更為 _RUNNING_

在遵循前面的順序之後，您可以使用其他 API。

[此呼叫順序中的每個步驟都有程式碼範例。](cdn-example-code.html#code-examples-using-the-cdn-api)

**附註**：您**必須**使用具有 `CDN_ACCOUNT_MANAGE` 許可權之使用者的 API 使用者名稱和 API 金鑰，來進行本文件所示的大部分 API 呼叫。如果您需要啟用這個許可權，請與帳戶的主要使用者聯絡。（每個 IBM Cloud 客戶帳戶都有一個主要使用者。）

----
## 供應商的 API
### listVendors
此 API 容許使用者列出支援的 CDN 供應商。需要 `vendorName` 才能建立 CDN 帳戶及開始訂購 CDN。

* **必要參數**：無
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Vendor` 的集合

  供應商容器及用法範例可在這裡檢視：[供應商容器](vendor-container.html)

----
## 帳戶的 API
### verifyCdnAccountExists
確認呼叫 API 的使用者是否有給定 `vendorName` 的 CDN 帳戶。

* **必要參數**：`vendorName`：請提供有效 CDN 提供者的名稱。
* **傳回**：如果帳戶存在，則為 `true`，否則為 `false`。

----
## 網域對映的 API
### createDomainMapping
使用提供的輸入，此功能會建立給定供應商的網域對映，並建立它與使用者的「{{site.data.keyword.BluSoftlayer_notm}} 帳戶 ID」的關聯。必須先使用 `placeOrder` 建立 CDN 帳戶，此 API 才能運作（如需 `placeOrder` API 呼叫的範例，請參閱[程式碼範例](cdn-example-code.html)）。順利建立 CDN 之後，會建立值為 3600 秒的 `defaultTTL`。

* **參數**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 的集合。您可以在這裡檢視「輸入容器」中的所有屬性：

  [檢視輸入容器](input-container.html)

  下列屬性是「輸入容器」的一部分，可以在建立網域對映時提供（除非另有說明，否則屬性為選用性）：
    * `vendorName`：**必要**。請提供有效 IBM Cloud CDN 提供者的名稱。
    * `origin`：**必要**。請以字串形式提供原點伺服器位址。
    * `originType`：**必要**。原點類型可以為 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：**必要**。請以字串形式提供主機名稱。
    * `protocol`：**必要**。支援的通訊協定為 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `certificateType`：對於 HTTPS 通訊協定為**必要**。`SHARED_SAN_CERT` 或 `WILDCARD_CERT`
    * `path`：將從該處提供快取內容的路徑。預設路徑是 `/*`
    * `httpPort` 及/或 `httpsPort`：（對於主伺服器為**必要**）這兩個選項必須對應於想要的通訊協定。如果通訊協定是 `HTTP`，則必須設定 `httpPort`，且_不得_ 設定 `httpsPort`。同樣地，如果通訊協定是 `HTTPS`，則必須設定 `httpsPort`，且_不得_ 設定 `httpPort`。如果通訊協定是 `HTTP_AND_HTTPS`，則 `httpPort` 和 `httpsPort` _都必須_ 設定。Akamai 對於埠號有特定的限制。如需允許的埠號，請參閱[常見問題](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
    * `header`：指定原點伺服器所使用的主機標頭資訊。
    * `respectHeader`：布林值，如果設為 `true` 將導致原點的 TTL 設定置換 CDN TTL 設定。
    * `cname`：請提供主機名稱的別名。如果未提供，將會予以產生。
    * `bucketName`：（僅對於 Object Storage 為**必要**）S3 Object Storage 的儲存區名稱。
    * `fileExtension`：（對於 Object Storage 為選用性）允許快取的副檔名。
    * `cacheKeyQueryRule`：下列選項可用來配置快取金鑰行為。如果未提供任何 `cacheKeyQueryRule` 引數，將預設為 "include-all"
      * `include-all` - 包含所有查詢引數（**預設**）
      * `ignore-all` - 忽略所有查詢引數
      * `ignore: 以空格區隔的查詢引數` - 忽略那些特定的查詢引數。例如 `ignore: query1 query2`
      * `include: 以空格區隔的查詢引數` - 包含那些特定的查詢引數。例如 `include: query1 query2`

* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合。

  **附註**：此集合提供 `uniqueId` 值，需要傳送此值作為對映和原點路徑相關之後續 API 呼叫的輸入。

  [檢視對映容器](mapping-container.html)

----
### deleteDomainMapping
根據 `uniqueId`，刪除網域對映。網域對映必須處於下列其中一種狀態：_RUNNING_、_STOPPED_、_DELETED_、_ERROR_、_CNAME_CONFIGURATION_ 或 _SSL_CONFIGURATION_。

* **必要參數**：`uniqueId`：要刪除之對映的唯一 ID
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合
  [檢視對映容器](mapping-container.html)

----
### verifyDomainMapping
驗證 CDN 的狀態，並更新 CDN 對映的 `status`（如果它已變更的話）。一開始建立 CDN 對映時，它的狀態會顯示為_CNAME_CONFIGURATION_。此時，您必須更新 DNS 記錄，使 CDN 對映將主機名稱指向 CNAME。如果您有如何進行更新和變更可能需要多久才能在網際網路上傳播的相關問題，請洽詢您的 DNS 提供者。一般而言，應該需要 15 到 30 分鐘。該時間之後，必須呼叫這個 `verifyDomainMapping` API 以驗證 CNAME 鏈是否完整。如果 CNAME 鏈完整，CDN 對映的狀態會變更為 _RUNNING_。

可以隨時呼叫此 API 以取得最新的 CDN 對映狀態。網域對映必須處於下列其中一種狀態：_RUNNING_ 或 _CNAME_CONFIGURATION_。

* **必要參數**：`uniqueId`：您要驗證之對映的唯一 ID
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合

  [檢視對映容器](mapping-container.html)

----
### startDomainMapping
根據 `uniqueId`，啟動 CDN 網域對映。若要啟動，網域對映必須處於 _STOPPED_ 狀態。

* **必要參數**：`uniqueId`：要啟動之對映的唯一 ID
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合

  [檢視對映容器](mapping-container.html)

----
### stopDomainMapping
根據 `uniqueId`，停止 CDN 網域對映。若要起始停止，網域對映必須處於 _RUNNING_ 狀態。

* **必要參數**：`uniqueId`：要停止之對映的唯一 ID
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合

  [檢視對映容器](mapping-container.html)

----
### updateDomainMapping
讓使用者更新 `uniqueId` 所識別對映的內容。下列欄位可能已變更：`originHost`、`httpPort`、`httpsPort`、`respectHeader`、`header`、`cacheKeyQueryRule` 引數，如果您的原點類型為 Object Storage，則 `bucketName` 及 `fileExtension` 也可能變更。若要進行更新，網域對映必須處於 _RUNNING_ 狀態。

* **參數**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 的集合。您可以在這裡檢視「輸入容器」中的所有屬性：
  [檢視輸入容器](input-container.html)

  下列屬性是「輸入容器」的一部分，**必須**在更新網域對映時提供：
    * `vendorName`：請提供此對映之 CDN 提供者的名稱。
    * `path`：請提供此對映的現行路徑。
    * `origin`：請以字串形式提供原點伺服器位址。
    * `originType`：原點類型可以為 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：請提供主機名稱。
    * `protocol`：支援的通訊協定為 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `httpPort` 及/或 `httpsPort`：這兩個選項必須對應於想要的通訊協定。如果通訊協定是 `HTTP`，則必須設定 `httpPort`，且_不得_ 設定 `httpsPort`。同樣地，如果通訊協定是 `HTTPS`，則必須設定 `httpsPort`，且_不得_ 設定 `httpPort`。如果通訊協定是 `HTTP_AND_HTTPS`，則 `httpPort` 和 `httpsPort` _都必須_ 設定。Akamai 對於埠號有特定的限制。如需允許的埠號，請參閱[常見問題](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
    * `header`：指定原點伺服器所使用的主機標頭資訊。
    * `respectHeader`：布林值，如果設為 `true`，將導致原點的 TTL 設定置換 CDN TTL 設定。
    * `uniqueId`：在對映建立之後產生。
    * `cname`：請提供 CNAME。如果您未提供，則在建立對映時會產生。
    * `bucketName`：（僅對於 Object Storage 為**必要**）S3 Object Storage 的儲存區名稱。
    * `fileExtension`：（僅對於 Object Storage 為**必要**）允許快取的副檔名。
    * `cacheKeyQueryRule`：快取金鑰行為規則只能針對 11/16/17 _之後_ 建立的 CDN 對映更新。下列選項可用來配置快取金鑰行為：
      * `include-all` - 包含所有查詢引數（**預設**）
      * `ignore-all` - 忽略所有查詢引數
      * `ignore: 以空格區隔的查詢引數` - 忽略那些特定的查詢引數。例如 `ignore: query1 query2`
      * `include: 以空格區隔的查詢引數` - 包含那些特定的查詢引數。例如 `include: query1 query2`
* **傳回**類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合

  [檢視對映容器](mapping-container.html)

----
### listDomainMappings
傳回現行客戶的所有網域對映的集合。

* **必要參數**：無
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的集合

  [檢視對映容器](mapping-container.html)

----
### listDomainMappingByUniqueId
根據 CDN 的 `uniqueId`，傳回具有單一網域物件的集合。

* **必要參數**：`uniqueId`：要傳回之對映的唯一 ID
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 的單一物件集合

  [檢視對映容器](mapping-container.html)

----
## 原點的 API
### createOriginPath
建立現有 CDN 及特定客戶的「原點路徑」。「原點路徑」可以根據「主伺服器」或 Object Storage。若要建立「原點路徑」，網域對映必須處於 _RUNNING_ 或 _CNAME_CONFIGURATION_ 狀態。

* **參數**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 的集合。您可以在這裡檢視「輸入容器」中的所有屬性：

  [檢視輸入容器](input-container.html)

  下列屬性是「輸入容器」的一部分，可以在建立原點路徑時提供（除非另有說明，否則屬性為選用性）：
    * `vendorName`：**必要**。請提供有效 IBM Cloud CDN 提供者的名稱。
    * `origin`：**必要**。請以字串形式提供原點伺服器位址。
    * `originType`：**必要**。原點類型可以為 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `domain`：**必要**。請以字串形式提供主機名稱。
    * `protocol`：**必要**。支援的通訊協定為 `HTTP`、`HTTPS` 或 `HTTP_AND_HTTPS`。
    * `path`：將從該處提供快取內容的路徑。開頭必須為對映路徑。例如，如果對映路徑是 `/test`，則您的原點路徑可能是 `/test/media`。
    * `httpPort` 及/或 `httpsPort`：**必要**。這兩個選項必須對應於想要的通訊協定。如果通訊協定是 `HTTP`，則必須設定 `httpPort`，且_不得_ 設定 `httpsPort`。同樣地，如果通訊協定是 `HTTPS`，則必須設定 `httpsPort`，且_不得_ 設定 `httpPort`。如果通訊協定是 `HTTP_AND_HTTPS`，則 `httpPort` 和 `httpsPort` _都必須_ 設定。Akamai 對於埠號有特定的限制。如需允許的埠號，請參閱[常見問題](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
    * `header`：指定原點伺服器所使用的主機標頭資訊。
    * `uniqueId`：**必要**。在對映建立之後產生。
    * `cname`：請提供主機名稱的別名。如果您未提供唯一的 CNAME，則在建立對映時會為您產生一個。
    * `bucketName`：（對於 Object Storage 為**必要**）S3 Object Storage 的儲存區名稱。
    * `fileExtension`：（對於 Object Storage 為選用性）允許快取的副檔名。
    * `cacheKeyQueryRule`：下列選項可用來配置快取金鑰行為：
      * `include-all` - 包含所有查詢引數（**預設**）
      * `ignore-all` - 忽略所有查詢引數
      * `ignore: 以空格區隔的查詢引數` - 忽略那些特定的查詢引數。例如 `ignore: query1 query2`
      * `include: 以空格區隔的查詢引數` - 包含那些特定的查詢引數。例如 `include: query1 query2`

* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 的集合

  [檢視原點路徑容器](path-container.html)

----
### updateOriginPath
更新現有對映及特定客戶的現有「原點路徑」。無法使用此 API 來變更原點類型。可以變更下列內容：`path`、`origin`、`httpPort` 及 `httpsPort`、`header` 及 `cacheKeyQueryRule` 引數。若要更新，網域對映必須處於 _RUNNING_ 或 _CNAME_CONFIGURATION_ 狀態。

* **參數**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 的集合。您可以在這裡檢視「輸入容器」中的所有屬性：

  [檢視輸入容器](input-container.html)

  下列屬性是「輸入容器」的一部分，可以在更新原點路徑時提供（除非另有說明，否則屬性為選用性）：
    * `oldPath`：**必要**。要變更的現行路徑
    * `origin`：（如果更新則為**必要**）請以字串形式提供原點伺服器位址。
    * `originType`：**必要**。原點類型可以為 `HOST_SERVER` 或 `OBJECT_STORAGE`。
    * `path`：**必要**。要新增的新路徑。相對於對映路徑。
    * `httpPort` 及/或 `httpsPort`：（對於主伺服器為**必要**，如果更新的話）這兩個選項必須對應於想要的通訊協定。如果通訊協定是 `HTTP`，則必須設定 `httpPort`，且_不得_ 設定 `httpsPort`。同樣地，如果通訊協定是 `HTTPS`，則必須設定 `httpsPort`，且_不得_ 設定 `httpPort`。如果通訊協定是 `HTTP_AND_HTTPS`，則 `httpPort` 和 `httpsPort` _都必須_ 設定。Akamai 對於埠號有特定的限制。如需允許的埠號，請參閱[常見問題](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
    * `uniqueId`：**必要**。此原點所屬對映的唯一 ID。
    * `bucketName`：（僅對於 Object Storage 為**必要**）S3 Object Storage 的儲存區名稱。
    * `fileExtension`：（僅對於 Object Storage 為**必要**）允許快取的副檔名。
    * `cacheKeyQueryRule`：（如果更新則為**必要**）下列選項可用來配置快取金鑰行為：
      * `include-all` - 包含所有查詢引數（**預設**）
      * `ignore-all` - 忽略所有查詢引數
      * `ignore: 以空格區隔的查詢引數` - 忽略那些特定的查詢引數。例如 `ignore: query1 query2`
      * `include: 以空格區隔的查詢引數` - 包含那些特定的查詢引數。例如 `include: query1 query2`

* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 的集合

  [檢視原點路徑容器](path-container.html)

----
### deleteOriginPath
刪除現有 CDN 及特定客戶的現有「原點路徑」。若要刪除，網域對映必須處於 _RUNNING_ 或 _CNAME_CONFIGURATION_ 狀態。

* **必要參數**：
  * `uniqueId`：此原點路徑所屬對映的唯一 ID
  * `path`：要刪除的路徑

* **傳回**：如果刪除成功，則為狀態訊息，否則會擲出異常狀況

----
### listOriginPath
根據 `uniqueId`，列出現有對映的原點路徑。

* **必要參數**：
  * `uniqueId`：請提供您要列出原點路徑之對映的唯一 ID。
* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 的物件集合

  [檢視原點路徑容器](path-container.html)

----
## 清除的 API
### 清除的容器類別
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
建立清除記錄，並將它插入資料庫。

* **參數**：
  * `uniqueId`：將建立「清除」之對映的唯一 ID
  * `path`：要建立的「清除」

* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 的集合

----
### getPurgeHistoryPerMapping
根據 `uniqueId` 及 `saved` 狀態，傳回 CDN 的清除歷程。（依預設，`saved` 的值會設為 _UNSAVED_。）

* **參數**：
  * `uniqueId`：要擷取「清除」歷程之對映的唯一 ID
  * `saved`：傳回 _SAVED_ 或 _UNSAVED_ 的清除

* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 的集合

----
### saveOrUnsavePurgePath
更新清除路徑項目的狀態，以指出是否應該儲存該清除路徑。如果儲存清除路徑，則請建立新的 `saved` 清除。如果路徑是 `unsaved`，則請刪除已儲存的清除記錄。

* **參數**：
  * `uniqueId`：「清除」所屬對映的唯一 ID
  * `path`：要儲存或取消儲存的「清除」路徑
  * `saved`：_SAVED_ 或 _UNSAVED_

* **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge` 的集合

----
## 存活時間的 API  
### TimeToLive 類別變數：  
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

 ----
## 度量值的 API
[檢視度量值容器](metrics-container.html)
### getCustomerUsageMetrics
傳回客戶帳戶在一段指定期間的直接顯示畫面（沒有圖形）的預定統計資料總數。

 * **參數**：
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingUsageMetrics
傳回給定對映的直接顯示畫面的預定統計資料總數。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingHitsMetrics
傳回每個網域對映在給定時間範圍內依特定頻率的命中總數。頻率可以是 'day'、'week' 及 'month'，而每一個間隔都是圖形的一個圖點。根據 `startDate`、`endDate` 及 `frequency`，排序傳回資料。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingHitsByTypeMetrics
傳回給定時間範圍內依特定頻率的命中總數。頻率可以是 'day'、'week' 及 'month'，而每一個間隔都是圖形的一個圖點。根據 `startDate`、`endDate` 及 `frequency`，必須排序傳回資料。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingBandwidthMetrics
傳回個別 CDN 的邊緣命中數。每一個供應商的地區可能會不同。每個對映。

 * **參數**：  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
___
### getMappingBandwidthByRegionMetrics
傳回給定對映在一段指定期間的直接顯示畫面（沒有圖形）的預定統計資料總數。依預設，`frequency` 的值是 'aggregate'。

 * **參數**：
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **傳回**：類型 `SoftLayer_Container_Network_CdnMarketplace_Metrics` 的物件集合
