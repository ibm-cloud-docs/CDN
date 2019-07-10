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

# Contenitore fornitore
{: #vendor-container}

La raccolta `SoftLayer_Container_Network_CdnMarketplace_Vendor` contiene gli attributi utilizzati dalle nostre API Vendor.


**class** `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: il nome di un provider {{site.data.keyword.cloud}} CDN corrente.  
* `featureSummary`: un breve riepilogo delle funzioni del fornitore.  
* `features`: un elenco delle funzioni supportate dal fornitore.  
* `status`: indica se un fornitore è un'opzione disponibile tramite IBM Cloud.


Visualizza un elenco dei fornitori disponibili nonché le loro funzioni con una chiamata alla API `listVendors`:

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

La chiamata API precedente genera un output simile al seguente:

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
