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

# 供应商容器
{: #vendor-container}

`SoftLayer_Container_Network_CdnMarketplace_Vendor` 集合包含供应商 API 利用的属性。


`SoftLayer_Container_Network_CdnMarketplace_Vendor` **类**  
* `vendorName`：当前 {{site.data.keyword.cloud}} CDN 提供者的名称。  
* `featureSummary`：供应商功能的简单摘要。  
* `features`：供应商支持的功能列表。  
* `status`：指示供应商是否为通过 IBM Cloud 提供的可用选项。


通过调用 `listVendors` API 显示可用供应商及其功能的列表：

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

上面的 API 调用生成的输出类似以下内容：


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
