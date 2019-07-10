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

# ベンダー・コンテナー
{: #vendor-container}

`SoftLayer_Container_Network_CdnMarketplace_Vendor` コレクションには、ベンダー API によって使用される属性が含まれています。


**クラス** `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: 現行の {{site.data.keyword.cloud}} CDN プロバイダーの名前。  
* `featureSummary`: ベンダー機能の簡単な要約。  
* `features`: ベンダーによってサポートされる機能のリスト。  
* `status`: ベンダーが IBM Cloud から使用可能なオプションであるかどうかを示します。


`listVendors` API への呼び出しを使用して、利用可能なベンダーのリストをその機能とともに表示します。

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

上記の API 呼び出しによって、以下のような出力が得られます。

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
