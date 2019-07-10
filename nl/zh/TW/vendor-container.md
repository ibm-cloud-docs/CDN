---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-25"

keywords: vendor, container, collection, class, API, attributes, provider

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 供應商容器
{: #vendor-container}

`SoftLayer_Container_Network_CdnMarketplace_Vendor` 集合包含了我們的供應商 API 所使用的屬性。


**類別** `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`：現行 {{site.data.keyword.cloud}} CDN 提供者的名稱。  
* `featureSummary`：供應商特性的簡短摘要。  
* `features`：供應商支援的特性清單。  
* `status`：指出供應商是否是可透過 IBM Cloud 使用的選項。


呼叫 `listVendors` API，顯示可用供應商及其特性的清單：

```php
require_once __DIR__.'/vendor/autoload.php';

$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

前一個 API 呼叫會導致類似如下的輸出：


```php
SoftLayer_Container_Network_CdnMarketplace_Vendor Object
(
    [vendorName] => akamai
    [featureSummary] => Performance, Reliability and Scale
    [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
    [status] => ACTIVE
)

```
{: codeblock}
