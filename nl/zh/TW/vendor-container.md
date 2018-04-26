---

copyright:
  years: 2017
lastupdated: "2017-10-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 供應商容器
`SoftLayer_Container_Network_CdnMarketplace_Vendor` 集合包含了我們的供應商 API 所使用的屬性。 


類別 `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`：現行 IBM Cloud CDN 提供者的名稱。  
* `featureSummary`：供應商特性的簡短摘要。  
* `features`：供應商支援的特性清單。  
* `status`：指出供應商是否是可透過 IBM Cloud 使用的選項。


呼叫 `listVendors` API，顯示可用供應商及其特性的清單：

```php
$vendors = $client->listVendors();
``` 
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
