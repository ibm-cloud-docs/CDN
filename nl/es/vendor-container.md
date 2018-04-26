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

# Contenedor de proveedor
La recopilación `SoftLayer_Container_Network_CdnMarketplace_Vendor` contiene los atributos utilizados por las API de nuestros proveedores. 


clase `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: el nombre del proveedor actual de IBM Cloud CDN.  
* `featureSummary`: un breve resumen de las características del proveedor.  
* `features`: una lista de las características admitidas por el proveedor.  
* `status`: indica si un proveedor es una opción disponible a través de IBM Cloud.


Visualice una lista de los proveedores disponibles, así como sus características, con una llamada a la API `listVendors`:

```php
$vendors = $client->listVendors();
``` 
La llamada de API anterior genera un resultado similar a este:
```php
SoftLayer_Container_Network_CdnMarketplace_Vendor Object
(
    [vendorName] => akamai
    [featureSummary] => Performance, Reliability and Scale
    [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
    [status] => ACTIVE
)

```
