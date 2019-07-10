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

# 輸入容器
{: #input-container}

輸入容器是對映和（原點）路徑類別都使用的集合。它為這兩個類別提供了一組一致的輸入屬性。

* `vendorName`：有效 {{site.data.keyword.cloud}} CDN 提供者的名稱。
* `oldPath`：由 updateOriginPath() 使用。這個內容會儲存現行（或舊的）路徑的名稱。

下列屬性是對映和（原點）路徑類別共同的屬性：
* `originType`：原點主機的類型，目前為 'HOST_SERVER' 或 'OBJECT_STORAGE'。
* `origin`：原點伺服器位址（原點伺服器的主機名稱或 IPv4 位址），這是要從該處提取內容的端點，或是內容儲存所在儲存區的名稱。它必須少於 511 個字元。
* `httpPort`：用於 HTTP 通訊協定的埠號。Akamai 對於 HTTP 和 HTTPS 埠的埠號有特定的限制。如需允許的埠號清單，請參閱[常見問題](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
* `httpsPort`：用於 HTTPS 通訊協定的埠號。Akamai 對於 HTTP 和 HTTPS 埠的埠號有特定的限制。如需允許的埠號清單，請參閱[常見問題](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
* `status`：對映或路徑的狀態。狀態可以是 CNAME_CONFIGURATION、SSL_CONFIGURATION、RUNNING、STOPPED、DELETED 或 ERROR。
* `path`：將從該處提供快取內容的路徑。預設路徑是 /\*。由 `updateOriginPath` API 使用時，這個屬性是指要新增的新路徑。
* `performanceConfiguration`：對映效能配置的規格。
* `cacheKeyQueryRule`：下列選項可用來配置快取金鑰行為：
  * `include-all`：包含所有查詢引數
  * `ignore-all`：忽略所有查詢引數
  * `ignore: 以空格區隔的查詢引數`：忽略那些特定的查詢引數。例如 `ignore: query1 query2`
  * `include: 以空格區隔的查詢引數`：包含那些特定的查詢引數。例如 `include: query1 query2`
* `geoblockingRule`

下列屬性是對映類別所特有：

* `uniqueId`：由系統產生且是每個對映獨有的 10 位數 ID。在對映建立時產生。
* `domain`：主要的 CDN 名稱。也稱為主機名稱。
* `protocol`：用來設定服務的通訊協定。它可以是 HTTP、HTTPS，或兩者的組合 - HTTP_AND_HTTPS。
* `cname`：主機名稱別名的標準名稱記錄。它可以由使用者提供，也可以由系統產生。如果由使用者提供，則應該少於 64 個英數字元，且必須是唯一的。如果未提供，將會在建立對映時產生一個。
* `certificateType`：對映所使用的憑證類型。可能是 `WILDCARD_CERT` 或 `SHARED_SAN_CERT`。對於 HTTP 對映，值將為 0。
* `respectHeaders`：布林值，如果設為 'true' 會導致原點的 TTL 設定置換 CDN TTL 設定。
* `header`：指定原點所使用的主機標頭資訊。

下列屬性與 Cloud Object Storage (COS) 相關：  
* `bucketName`：S3 Object Storage 的儲存區唯一名稱。  
* `fileExtension`：允許的副檔名。

下列屬性與配置「快速鏈結保護」相關：
* `hotlinkProtection`：如需詳細資料，請參閱[快速鏈結保護類別](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)。
