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

# Anbietercontainer
Die Sammlung `SoftLayer_Container_Network_CdnMarketplace_Vendor` enthält die Attribute, die von den Anbieter-APIs (Vendor-APIs) genutzt werden. 


Klasse `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: Der Name eines aktuellen IBM Cloud CDN-Anbieters.  
* `featureSummary`: Eine kurze Zusammenfassung der Anbieterfunktionen.  
* `features`: Eine Liste der Funktionen, die vom Anbieter unterstützt werden.  
* `status`: Gibt an, ob ein Anbieter eine über IBM Cloud verfügbare Option ist.


Zeigen Sie eine Liste der verfügbaren Anbieter und ihrer Funktionen mit einem Aufruf der API `listVendors` an:

```php
$vendors = $client->listVendors();
``` 
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
