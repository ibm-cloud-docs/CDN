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

# Contenedor de proveedor
{: #vendor-container}

La recopilación `SoftLayer_Container_Network_CdnMarketplace_Vendor` contiene los atributos utilizados por las API de nuestros proveedores.


**clase** `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: el nombre de un proveedor actual de {{site.data.keyword.cloud}} CDN.  
* `featureSummary`: un breve resumen de las características del proveedor.  
* `features`: una lista de las características admitidas por el proveedor.  
* `status`: indica si un proveedor es una opción disponible a través de IBM Cloud.


Visualice una lista de los proveedores disponibles, así como sus características, con una llamada a la API `listVendors`:

```php
require_once __DIR__.'/vendor/autoload.php';

$apiUsername = '<Nombre de usuario>' ;
$apiKey = '<Clave de API>' ;

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

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
{: codeblock}
