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

# Esempi di codice utilizzando l'API CDN

## Codici di esempio dell'elenco dei fornitori

Il pre-requisito è di scaricare e installare il client Soap da https://github.com/softlayer/softlayer-api-php-client

```
// Codice di esempio per illustrare la chiamata API Soap CDN
// Questo codice stampa i fornitori supportati dal servizio IBM CDN dal terminale da cui viene eseguito

// Passo 1: Ottieni l'accesso a SoapClient tramite vendor/autoload. Il percorso è relativo a dove viene
// eseguito lo script e potrebbe dover essere modificato appropriatamente
require_once './../vendor/autoload.php';

// Passo 2: Fornisci un nome utente e una chiave API validi
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Passo 3: Inizializza SoapClient per la classe appropriata. In questo caso, è la classe
// SoftLayer_Network_CdnMarketplace_Vendor che definisce il metodo listVendors
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);

// Passo 4: Richiama il metodo listVendors e stampa i fornitori
// Quando ha esito positivo lo script visualizzerà i fornitori nel terminale
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

## Esempio di codice per verificare l'ordine 

```
<?php

// Questo codice verifica se un ordine CDN può essere correttamente inserito

// Passo 1: Ottieni l'accesso a SoapClient tramite vendor/autoload. Il percorso è relativo a dove viene
// eseguito lo script e potrebbe dover essere modificato appropriatamente
require_once './../vendor/autoload.php';

// Passo 2: Fornisci un nome utente e una chiave API validi
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// Passo 3: Inizializza un client API per la classe SoftLayer_Product_Package
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Passo 4: Imposta il filtro e la maschera per ottenere l'oggetto pacchetto corretto
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

    // Passo 5: Inizializza l'oggetto OrderData
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Passo 6: Crea SoapVar orderObject
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Passo 7: Ottieni il client Product Order e richiama verifyOrder
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

```
<?php

// Questo codice è identico a VerifyOrder ad eccezione del passo 7 che richiama placeOrder. 
// Deve essere richiamato PlaceOrder solo dopo l'esito positivo di verifyOrder 

// Passo 1: Ottieni l'accesso a SoapClient tramite vendor/autoload. Il percorso è relativo a dove viene
// eseguito lo script e potrebbe dover essere modificato appropriatamente
require_once './../vendor/autoload.php';

// Passo 2: Fornisci un nome utente e una chiave API validi
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// Passo 3: Inizializza un client API per la classe SoftLayer_Product_Package
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Passo 4: Imposta il filtro e la maschera per ottenere l'oggetto pacchetto corretto
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

    // Passo 5: Inizializza l'oggetto OrderData
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Passo 6: Crea SoapVar orderObject
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Passo 7: Ottieni il client Product Order e richiama placeOrder
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

```
<?php

// Questo è il codice di esempio per visualizzare come richiamare createDomainMapping per creare una nuova CDN

// Passo 1: Ottieni l'accesso a SoapClient tramite vendor/autoload. Il percorso è relativo a dove viene
// eseguito lo script e potrebbe dover essere modificato appropriatamente
require_once './../vendor/autoload.php';

// Passo 2: Fornisci un nome utente e una chiave API validi
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Passo 3: Inizializza SoapClient per la classe appropriata. In questo caso, è la classe
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping che definisce il metodo createDomainMapping
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Passo 4: Inizializza l'oggetto $input e richiama createDomainMapping
try {
    $input = new stdClass();

    // Fornisci il nome fornitore
    $input->vendorName = "akamai";

    // Specifica il nome host e Cname
    $input->domain = "beta.testingcdn.net";
    $input->cname = "beta.cdnedge.bluemix.net";

    // Specifica il tipo di origine. Qui viene scelto "HOST_SERVER"
    $input->originType = "HOST_SERVER";

    // Specifica il numero porta, il protocollo e l'indirizzo di origine
    $input->origin = "testserver.testingcdn.net";
    $input->protocol = "HTTP";
    $input->httpPort = 80;

    // Specify Options
    $input->respectHeaders = true;

    $cdnMapping = $client->createDomainMapping($input);
    print_r($cdnMapping);
} catch (\Exception $e) {
    die('createDomainMapping failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}

## Codice di esempio per verificare l'associazione del dominio

```
<?php

// Prima di richiamare verifyDomainMapping, il cliente deve aggiungere un record CNAME del nome host personalizzato al server DNS.
// Questo codice richiama verifyDomainMapping con l'UniqueId restituito come parte di createDomainMapping.
// VerifyDomainMapping controlla se la configurazione CNAME è completa e se lo è, cambia lo stato CDN in RUNNING

// Passo 1: Ottieni l'accesso a SoapClient tramite vendor/autoload. Il percorso è relativo a dove viene
// eseguito lo script e potrebbe dover essere modificato appropriatamente
require_once './../vendor/autoload.php';

// Passo 2: Fornisci un nome utente e una chiave API validi
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Passo 3: Inizializza SoapClient per la classe appropriata. In questo caso, è la classe
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping che definisce il metodo verifyDomainMapping
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Passo 4: Inizializza $uniqueId e richiama verifyDomainMapping
try {
    $uniqueId = 907987176256747;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}
