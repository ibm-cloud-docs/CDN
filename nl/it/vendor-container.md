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

# Contenitore fornitore
La raccolta `SoftLayer_Container_Network_CdnMarketplace_Vendor` contiene gli attributi utilizzati dalle nostre API Vendor. 


class `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: il nome di un provider di CDN IBM Cloud corrente.  
* `featureSummary`: un breve riepilogo delle funzioni del fornitore.  
* `features`: un elenco delle funzioni supportate dal fornitore.  
* `status`: indica se un fornitore è un'opzione disponibile tramite IBM Cloud.


Visualizza un elenco dei fornitori disponibili nonché le loro funzioni con una chiamata alla API `listVendors`:

```php
$vendors = $client->listVendors();
``` 
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
