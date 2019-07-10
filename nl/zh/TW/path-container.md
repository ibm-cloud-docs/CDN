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

# 路徑（原點）容器
{: #path-origin-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 集合包含了我們的（原點）路徑 API 所使用的屬性。每個路徑 API 都會傳回此類型的集合。

**類別** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`：此原點路徑所屬對映的唯一 ID。  
* `path`：相對於可用來聯繫此原點之網域的路徑。  
* `originType`：原點主機的類型，目前為 'HOST\_SERVER' 或 'OBJECT\_STORAGE'。  
* `httpPort`：用於 HTTP 通訊協定的埠號。  
* `httpsPort`：用於 HTTPS 通訊協定的埠號。  
* `status`：（原點）路徑的狀態。有效的狀態是：_CREATING_、_STARTING_、_RUNNING_、_UPDATING_、_STOPPING_ 及 _DELETING_。
* `fileExtension`：允許快取的副檔名。  
* `header`：指定原點伺服器所使用的主機標頭資訊。
* `cacheKeyQueryRule`：下列選項可用來配置快取金鑰行為：
  * `include-all`：包含所有查詢引數（**預設**）
  * `ignore-all`：忽略所有查詢引數
  * `ignore: 以空格區隔的查詢引數`：忽略那些特定的查詢引數。例如 "ignore: query1 query2"
  * `include: 以空格區隔的查詢引數`：包含那些特定的查詢引數。例如 "include: query1 query2"
