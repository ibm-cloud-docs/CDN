---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: code examples, example API calls, CDN API, Soap, client, apiKey

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Exemples de code basé sur l'API du CDN
{: #code-examples-using-the-cdn-api}

Ce document contient des appels API et la sortie résultante pour de nombreuses API CDN.

## Etapes générales nécessaires pour tous les appels API
{: #general-steps-needed-for-all-api-calls}

Vous devez au préalable avoir téléchargé et installé le client SOAP depuis la page [https://github.com/softlayer/softlayer-api-php-client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/softlayer/softlayer-api-php-client)

  * Vous devez accéder à SoapClient via `vendor/autoload`. Le chemin est relatif par rapport à l'emplacement depuis lequel le script est exécuté et il peut donc être nécessaire de le modifier comme il convient. Dans PHP, l'instruction ressemblera à ceci : `require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * Tous les appels API sont authentifiés avec votre nom d'utilisateur et une clé apiKey. Vous trouverez plus d'informations sur la façon de générer une clé apiKey sur la page [Softlayer API Getting Started page ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/article/getting-started/), under "Getting Your API Key".

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * Initialisez SoapClient pour la classe appropriée.

## Exemple de code permettant de répertorier les fournisseurs
{: #example-code-for-listing-vendors}

Dans ce cas, c'est la classe SoftLayer_Network_CdnMarketplace_Vendor qui définit l'API `listVendors` API ; elle doit être passée en tant que paramètre à `\SoftLayer\SoapClient::getClient()`. Vous aurez besoin du nom d'un fournisseur actif plus tard, quand vous créerez un mappage de domaine.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

Le code `listVendor` affiche un tableau des fournisseurs, ainsi que leur statut et fonctions. L'exemple de sortie montre un fournisseur actif, Akamai.

```php
Array
(
    [0] => stdClass Object
        (
            [featureSummary] => Performance, Reliability and Scale
            [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
            [status] => ACTIVE
            [vendorName] => akamai
        )

)
```

{: codeblock}

## Exemple de code permettant de vérifier la commande
{: #example-code-to-verify-order}

L'appel vers `verifyOrder` n'est pas obligatoire avant de passer une commande mais il est conseillé. Il peut être utilisé pour vérifier qu'un autre appel vers `placeOrder` aboutira. Vous trouverez plus d'informations sur `verifyOrder` dans la [documentation SoftLayer API![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/).

Dans ce cas, c'est la classe `SoftLayer_Product_Order` qui définit la méthode verifyOrder ; elle doit être passée en tant que paramètre à `\SoftLayer\SoapClient::getClient()`. Avant l'appel vers `verifyOrder`, vous devez générer `$orderObject` en utilisant `SoftLayer_Product_Package`.

```php
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

try {
    $filter = new stdClass();
    $filter->keyName = new stdClass();
    $filter->keyName->operation = 'CONTENT_DELIVERY_NETWORK_SERVICE';

    $client->setObjectFilter($filter);
    $client->setObjectMask(
        "mask[
            itemPrices.item[
                attributes.attributeType,
                bundleItems.activeUsagePrices[
                    priceType,
                    pricingLocationGroup.locations
                ]
            ]
        ]"
    );

    $package = $client->getAllObjects()[0];

    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    $productOrderClient = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Order', null, $apiUsername, $apiKey);

    $result = $productOrderClient->verifyOrder($orderObject);
    print_r($result);
    print_r("\n");
}

catch (\Exception $e) {
    die('Verify Order failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}


## Exemple de code permettant de passer la commande
{: #example-code-to-place-order}

Cet appel API est identique à l'exemple de code précédent, sauf qu'il appelle `placeOrder` plutôt que `verifyOrder.` Vous trouverez plus d'informations sur `placeOrder` dans la [documentation SoftLayer API![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/).

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

try {
    $filter = new stdClass();
    $filter->keyName = new stdClass();
    $filter->keyName->operation = 'CONTENT_DELIVERY_NETWORK_SERVICE';

    $client->setObjectFilter($filter);
    $client->setObjectMask(
        "mask[
            itemPrices.item[
                attributes.attributeType,
                bundleItems.activeUsagePrices[
                    priceType,
                    pricingLocationGroup.locations
                ]
            ]
        ]"
    );

    $package = $client->getAllObjects()[0];

    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    $productOrderClient = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Order', null, $apiUsername, $apiKey);

    $result = $productOrderClient->placeOrder($orderObject);
    print_r($result);
    print_r("\n");
}

catch (\Exception $e) {
    die('Place order failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}


## Exemple de code permettant de créer un CDN ou un mappage de domaine
{: #example-code-to-create-cdn-or-create-domain-mapping}

Cet exemple vous montre comment créer un nouveau mappage CDN en utilisant l'API `createDomainMapping`. Il accepte un paramètre unique de l'objet `stdClass`. SoapClient doit être initialisé via la classe `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`, comme illustré dans l'exemple.

Si vous choisissez de fournir un nom CNAME personnalisé, il **doit** se terminer par `.cdnedge.bluemix.net` ou une erreur est émise.  Voir [cette description](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-) pour savoir comment fournir votre propre nom CNAME.
{: important}

```php

$client = \SoftLayer\SoapClient::getClient(
    'SoftLayer_Network_CdnMarketplace_Configuration_Mapping',
    null,
    $apiUsername,
    $apiKey
  );


try {
    $inputObject = new stdClass();

    // The following values are required
    $inputObject->vendorName = "akamai";
    $inputObject->origin = "origin.cdntesting.net";
    $inputObject->originType = "HOST_SERVER";
    $inputObject->domain = "api-testing.cdntesting.net";
    $inputObject->protocol = "HTTP";
    $inputObject->httpPort = 80;

    // The following value is required only for HTTPS protocol
    $inputObject->certificateType = "SHARED_SAN_CERT";

    // The following values are optional
    $inputObject->cname = "api-testing.cdnedge.bluemix.net";
    $inputObject->path = "/media";
    $inputObject->header = '';
    $inputObject->respectHeader = true;
    $inputObject->bucketName = 'mybucket';
    $inputObject->fileExtension = "txt, jpeg";
    $inputObject->cacheKeyQueryRule = "include-all";

    $cdnMapping = $client->createDomainMapping($inputObject);
    print_r($cdnMapping);

} catch (\Exception $e) {
    die('createDomainMapping failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}

L'exemple `createDomainMapping` affiche les attributs du CDN nouvellement créé. Prenez note du code `uniqueId`, car vous aurez à le fournir pour de nombreuses autres API. La sortie doit ressembler à ceci :

```php
Array
(
    [0] => stdClass Object
        (
            [bucketName] => mybucket
            [cacheKeyQueryRule] => include-all
            [certificateType] => SHARED_SAN_CERT
            [cname] => api-testing.cdnedge.bluemix.net
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
            [performanceConfiguration] => General web delivery
            [protocol] => HTTP
            [respectHeaders] => 1
            [serveStale] => 1
            [status] => CNAME_CONFIGURATION
            [uniqueId] => 610345992629xxx
            [vendorName] => akamai
        )

)
```
{: codeblock}

## Exemple de code permettant de vérifier le mappage de domaine
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping vérifie si la configuration CNAME est terminée et, si c'est la cas, change le statut CDN en RUNNING. Avant d'appeler `verifyDomainMapping`, vous devez ajouter un enregistrement CNAME du nom d'hôte Hostname personnalisé à votre serveur DNS.

Cet exemple appelle `verifyDomainMapping` avec l'ID UniqueId qui a été retourné en tant qu'élément de `createDomainMapping`. SoapClient doit être initialisé en utilisant la classe `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`, comme illustré dans l'exemple suivant.

```php

$client = \SoftLayer\SoapClient::getClient(
    'SoftLayer_Network_CdnMarketplace_Configuration_Mapping',
    null,
    $apiUsername,
    $apiKey
  );

try {
    $uniqueId = 610345992629xxx;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}

Si votre enregistrement CNAME a été ajouté à votre serveur DNS, le statut `status` de votre CDN passe à RUNNING après l'appel vers `verifyDomainMapping`,  comme illustré dans l'exemple suivant.

```php

    [0] => stdClass Object
        (
          ...

            [status] => RUNNING
            [uniqueId] => 610345992629xxx

          ...
        )
```
{: codeblock}


Si votre enregistrement CNAME n'a pas été ajouté à votre serveur DNS ou que votre serveur n'a pas encore été mis à jour, le statut `status` de votre CDN passe à CNAME_CONFIGURATION, comme illustré dans l'exemple suivant.

L'exécution du chaînage CNAME peut prendre plusieurs minutes (jusqu'à 30).
{: note}

```php
Array
(
    [0] => stdClass Object
        (
          ...

            [status] => CNAME_CONFIGURATION
            [uniqueId] => 610345992629xxx

          ...
        )

)
```
{: codeblock}

Pour être sûr que votre enregistrement CNAME est configuré correctement, exécutez `dig <your domain>` sur la ligne de commande. La sortie devrait être similaire à la suivante :

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

Le nom de domaine est mappé correctement à l'enregistrement CNAME.
