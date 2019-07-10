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

# Conteneur des fournisseurs
{: #vendor-container}

La collection `SoftLayer_Container_Network_CdnMarketplace_Vendor` contient les attributs utilisés par les API de nos fournisseurs.


**Classe** `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName` : Nom d'un fournisseur {{site.data.keyword.cloud}} CDN en cours.  
* `featureSummary` : Récapitulatif des fonctions du fournisseur.  
* `features` : Liste des fonctions prises en charge par le fournisseur.  
* `status` : Indique si un fournisseur est une option disponible via IBM Cloud.


Pour afficher une liste des fournisseurs disponibles ainsi que de leurs fonctions, appelez l'API `listVendors` :

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

L'appel d'API précédent donne un résultat similaire à celui-ci :

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
