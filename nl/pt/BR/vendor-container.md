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

# Contêiner do fornecedor
A coleção `SoftLayer_Container_Network_CdnMarketplace_Vendor` contém os atributos utilizados pelas nossas
APIs do fornecedor. 


classe `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: o nome de um provedor CDN atual do IBM Cloud.  
* `featureSummary`: um breve resumo dos recursos do fornecedor.  
* `features`: uma lista de recursos que são suportados pelo fornecedor.  
* `status`: indica se um fornecedor é uma opção disponível por meio do IBM Cloud.


Exibir uma lista dos fornecedores disponíveis, assim como seus recursos, com uma chamada para a API
`listVendors`:

```php
$vendors = $client->listVendors();
``` 
A chamada API anterior resulta em saída semelhante a essa:
```php
SoftLayer_Container_Network_CdnMarketplace_Vendor Object
(
    [vendorName] => akamai
    [featureSummary] => Performance, Reliability and Scale
    [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
    [status] => ACTIVE
)

```
