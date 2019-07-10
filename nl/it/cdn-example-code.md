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

# Esempi di codice utilizzando l'API CDN
{: #code-examples-using-the-cdn-api}

Questo documento contiene chiamate API di esempio e l'output risultante per numerose API CDN.

## Passi generali necessari per tutte le chiamate API
{: #general-steps-needed-for-all-api-calls}

Il prerequisito è di scaricare e installare il client Soap da [https://github.com/softlayer/softlayer-api-php-client ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/softlayer/softlayer-api-php-client)

  * Devi accedere a SoapClient tramite `vendor/autoload`. Il percorso è relativo a dove viene eseguito lo script e potrebbe essere necessario modificarlo in modo appropriato. In PHP, la dichiarazione sarà simile a questa: `require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * Tutte le chiamate API vengono autenticate con il tuo nome utente e una apiKey. Ulteriori informazioni su come generare una apiKey sono disponibili nella [pagina Softlayer API Getting Started ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/article/getting-started/) nella sezione "Getting Your API Key".

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * Inizializza SoapClient per la classe appropriata.

## Codice di esempio per elencare i fornitori
{: #example-code-for-listing-vendors}

In questo caso, è la classe SoftLayer_Network_CdnMarketplace_Vendor che definisce l'API `listVendors` e deve essere passata come parametro a `\SoftLayer\SoapClient::getClient()`. Avrai bisogno del nome di un fornitore attivo in seguito, quando creerai un'Associazione dominio

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

Il codice `listVendor` visualizzerà un array di fornitori, così come lo stato e le funzioni. L'output di esempio mostra un fornitore attivo, Akamai.

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

## Esempio di codice per verificare l'ordine
{: #example-code-to-verify-order}

La chiamata a `verifyOrder` non è obbligatoria prima di effettuare un ordine, ma è comunque consigliata. Può essere utilizzata per verificare che una successiva chiamata a `placeOrder` avrà esito positivo. Ulteriori informazioni su `verifyOrder` sono disponibili nella [documentazione dell'API SoftLayer ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/).

In questo caso, è la classe `SoftLayer_Product_Order` che definisce il metodo verifyOrder e deve essere passata come parametro a `\SoftLayer\SoapClient::getClient()`. Prima della chiamata a `verifyOrder`, devi creare `$orderObject` utilizzando `SoftLayer_Product_Package`.

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


## Codice di esempio per inserire un ordine
{: #example-code-to-place-order}

Questa chiamata API è identica all'esempio di codice precedente, tranne per il fatto che chiama `placeOrder` anziché `verifyOrder.` Ulteriori informazioni su `placeOrder` sono disponibili nella [documentazione dell'API SoftLayer ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/).

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


## Esempio di codice per creare CDN o un'associazione del dominio
{: #example-code-to-create-cdn-or-create-domain-mapping}

Questo esempio ti mostra come creare una nuova associazione CDN utilizzando l'API `createDomainMapping`. Viene utilizzato un solo parametro di un oggetto `stdClass`. SoapClient deve essere inizializzato utilizzando la classe `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` come mostrato nell'esempio.

Se scegli di fornire un CNAME personalizzato, questo **deve** terminare con `.cdnedge.bluemix.net`, altrimenti verrà generato un errore. Vedi [questa descrizione](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-) per le regole su come fornire il tuo CNAME.
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

L'esempio `createDomainMapping` visualizzerà gli attributi della CDN appena creata. Prendi nota di `uniqueId`, poiché dovrai fornirlo come parametro per molte altre API. L'output avrà un aspetto simile al seguente:

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

## Codice di esempio per verificare l'associazione del dominio
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping controlla se la configurazione CNAME è completa e se lo è, cambia lo stato CDN in RUNNING. Prima di richiamare `verifyDomainMapping`, devi aggiungere un record CNAME del nome host personalizzato al tuo server DNS.

Questo esempio richiama `verifyDomainMapping` con l'UniqueId che era restituito come parte di `createDomainMapping`. SoapClient deve essere inizializzato utilizzando la classe `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` come mostrato nel seguente esempio.

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

Se il tuo record CNAME è stato aggiunto al server DNS, lo `stato` della tua CDN cambierà in RUNNING dopo la chiamata a `verifyDomainMapping`, come mostrato nel seguente esempio.

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


Se il tuo record CNAME non è stato aggiunto al tuo server DNS o se il tuo server non è stato aggiornato, lo `stato` della tua CDN sarà CNAME_CONFIGURATION, come mostrato nel seguente esempio.

Per il completamento del concatenamento CNAME potrebbero essere necessari diversi minuti (fino a 30).
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

Per essere sicuro che il tuo record CNAME sia configurato correttamente, esegui `dig <your domain>` nella riga di comando. L'output avrà un aspetto simile al seguente:

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

Qui vediamo il nome di dominio associato correttamente al CNAME.
