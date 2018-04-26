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

# Conteneur des fournisseurs
La collection `SoftLayer_Container_Network_CdnMarketplace_Vendor` contient les attributs utilisés par les API de nos fournisseurs. 


Classe `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName` : Nom du fournisseur IBM Cloud CDN en cours.  
* `featureSummary` : Récapitulatif des fonctions du fournisseur.  
* `features` : Liste des fonctions prises en charge par le fournisseur.  
* `status` : Indique si un fournisseur est une option disponible via IBM Cloud.


Pour afficher une liste des fournisseurs disponibles ainsi que de leurs fonctions, appelez l'API `listVendors` :

```php
$vendors = $client->listVendors();
``` 
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
