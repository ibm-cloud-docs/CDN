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

# Anbietercontainer
{: #vendor-container}

Die Sammlung `SoftLayer_Container_Network_CdnMarketplace_Vendor` enthält die Attribute, die von den Anbieter-APIs (Vendor-APIs) genutzt werden.


**Klasse** `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: Der Name eines aktuellen {{site.data.keyword.cloud}} CDN-Anbieters.  
* `featureSummary`: Eine kurze Zusammenfassung der Anbieterfunktionen.  
* `features`: Eine Liste der Funktionen, die vom Anbieter unterstützt werden.  
* `status`: Gibt an, ob ein Anbieter eine über IBM Cloud verfügbare Option ist.


Zeigen Sie eine Liste der verfügbaren Anbieter und ihrer Funktionen mit einem Aufruf der API `listVendors` an:

```php
require_once __DIR__.'/vendor/autoload.php';

$apiUsername = '<Ihr Benutzername>' ;
$apiKey = '<Ihr API-Schlüssel>' ;

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

Der obige API-Aufruf führt zu einer Ausgabe wie der folgenden:

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
