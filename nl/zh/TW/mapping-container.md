---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# 對映容器
{: #mapping-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 集合包含了我們的「對映 API」所使用的屬性。每個「對映 API」都會傳回此類型的集合。

**類別** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`：

* `vendorName`：有效 {{site.data.keyword.cloud}} CDN 提供者的名稱。
* `uniqueId`：由系統產生且是每個對映獨有的 10 位數 ID。在對映建立時產生。
* `domain`：主要的 CDN 名稱。也稱為 `hostname`。
* `protocol`：用來設定服務的通訊協定。它可以是 HTTP、HTTPS，或兩者的組合 - HTTP_AND_HTTPS。
* `originType`：原點主機的類型，目前為 'HOST_SERVER' 或 'OBJECT_STORAGE'。
* `originHost`：原點伺服器位址（原點伺服器的主機名稱或 IPv4 位址），這是要從該處提取內容的端點，或是內容儲存所在儲存區的名稱。它必須少於 511 個字元。
* `httpPort`：用於 HTTP 通訊協定的埠號。Akamai 對於 HTTP 和 HTTPS 埠的埠號有特定的限制。如需允許的埠號，請參閱[常見問題](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
* `httpsPort`：用於 HTTPS 通訊協定的埠號。Akamai 對於 HTTP 和 HTTPS 埠的埠號有特定的限制。如需允許的埠號，請參閱[常見問題](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)。
* `certificateType`：對映所使用的憑證類型。可能是 `WILDCARD_CERT` 或 `SHARED_SAN_CERT`。對於 HTTP 對映，值將為 0。
* `cname`：主機名稱別名的標準名稱記錄。它可以由使用者提供，也可以由系統產生。如果由使用者提供，則應該少於 64 個英數字元，且必須是唯一的。如果未提供，將會在建立對映時產生一個。
* `respectHeaders`：布林值，如果設為 'true' 會導致原點的 TTL 設定置換 CDN TTL 設定。
* `header`：指定原點伺服器所使用的主機標頭資訊。
* `status`：對映的狀態。狀態可以是 CNAME_CONFIGURATION、SSL_CONFIGURATION、RUNNING、STOPPED、DELETED 或 ERROR。
* `bucketName`：S3 Object Storage 的儲存區名稱。
* `fileExtension`：允許快取的副檔名。
* `path`：S3 Object Storage 的路徑。通常位於服務的物件儲存庫 URL 或 API 區段中。
* `cacheKeyQueryRule`：下列選項可用來配置快取金鑰行為：
  * `include-all`：包含所有查詢引數（**預設**）
  * `ignore-all`：忽略所有查詢引數
  * `ignore: 以空格區隔的查詢引數`：忽略那些特定的查詢引數。例如 "ignore: query1 query2"
  * `include: 以空格區隔的查詢引數`：包含那些特定的查詢引數。例如 "include: query1 query2"

特別值得注意的是 `uniqueId`，它是在建立對映時產生，且會用來作為後續 API 呼叫的參數。

例如，這個 `listDomainMappingByUniqueid` 呼叫  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

會傳回類似如下的物件：

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
            [cname] => api-testing.cdnedge.bluemix.net
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
            [performanceConfiguration] => General web delivery
            [protocol] => HTTP
            [respectHeaders] => 1
            [serveStale] => 1
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

`stopDomainMapping` 及 `startDomainMapping` 呼叫會傳回相同的「對映」物件，但 `status` 會不同。

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
