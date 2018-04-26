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

# Codebeispiele mit der CDN-API

## Beispielcode zum Auflisten von Anbietern

Als Voraussetzung muss der SOAP-Client von 'https://github.com/softlayer/softlayer-api-php-client' heruntergeladen und installiert werden.

```
// Beispielcode zur Veranschaulichung eines SOAP-API-Aufrufs für CDN
// Mit diesem Code werden die vom IBM Service 'CDN' unterstützten Anbieter auf dem Terminal ausgedruckt, auf dem der Code ausgeführt wird.

// Schritt 1: Erhalten Sie Zugriff auf den SOAP-Client über 'vendor/autoload'. Der Pfad ist relativ zu der Position, an der
// das Script ausgeführt wird und muss möglicherweise entsprechend bearbeitet werden.
require_once './../vendor/autoload.php';

// Schritt 2: Geben Sie einen gültigen Benutzernamen und Schlüssel für die API an.
$apiUsername = '<Ihr Benutzername>' ;
$apiKey = '<Ihr API-Schlüssel>' ;

// Schritt 3: Initialisieren Sie den SOAP-Client für die entsprechende Klasse. In diesem Fall ist es die Klasse
// 'SoftLayer_Network_CdnMarketplace_Vendor', die die Methode 'listVendors' definiert.
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);

// Schritt 4: Rufen Sie die Methode 'listVendors' auf und drucken Sie die Anbieter aus.
// Ist der Vorgang erfolgreich, zeigt das Script die Anbieter auf dem Terminal an.
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

## Beispielcode zum Prüfen der Bestellung 

```
<?php

// Mit diesem Code wird geprüft, ob ein CDN erfolgreich platziert werden kann.

// Schritt 1: Erhalten Sie Zugriff auf den SOAP-Client über 'vendor/autoload'. Der Pfad ist relativ zu der Position, an der
// das Script ausgeführt wird und muss möglicherweise entsprechend bearbeitet werden.
require_once './../vendor/autoload.php';

// Schritt 2: Geben Sie einen gültigen Benutzernamen und Schlüssel für die API an.
$apiUsername = '<Ihr Benutzername>' ;
$apiKey = '<Ihr API-Schlüssel>' ;


// Schritt 3: Initialisieren Sie den API-Client für die Klasse 'SoftLayer_Product_Package'.
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Schritt 4: Legen Sie Filter und Maske fest, um das richtige Paketobjekt zu erhalten.
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

    // Schritt 5: Initialisieren Sie das Objekt 'OrderData'.
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Schritt 6: Erstellen Sie 'SoapVar orderObject'.
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Schritt 7: Rufen Sie den Client für den Produktauftrag ab und rufen Sie 'verifyOrder' auf.
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


## Beispielcode zum Aufgeben einer Bestellung

```
<?php

// Dieser Code ist identisch mit 'VerifyOrder', mit Ausnahme von Schritt 7, mit dem 'placeOrder' aufgerufen wird. 
// 'PlaceOrder' sollte nur nach erfolgreicher Ausführung von 'verifyOrder' aufgerufen werden.

// Schritt 1: Erhalten Sie Zugriff auf den SOAP-Client über 'vendor/autoload'. Der Pfad ist relativ zu der Position, an der
// das Script ausgeführt wird und muss möglicherweise entsprechend bearbeitet werden.
require_once './../vendor/autoload.php';

// Schritt 2: Geben Sie einen gültigen Benutzernamen und Schlüssel für die API an.
$apiUsername = '<Ihr Benutzername>' ;
$apiKey = '<Ihr API-Schlüssel>' ;


// Schritt 3: Initialisieren Sie den API-Client für die Klasse 'SoftLayer_Product_Package'.
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Schritt 4: Legen Sie Filter und Maske fest, um das richtige Paketobjekt zu erhalten.
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

    // Schritt 5: Initialisieren Sie das Objekt 'OrderData'.
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Schritt 6: Erstellen Sie 'SoapVar orderObject'.
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Schritt 7: Rufen Sie den Client für den Produktauftrag ab und rufen Sie 'placeOrder' auf.
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


## Beispielcode zum Erstellen eines CDN oder einer Domänenzuordnung

```
<?php

// Mit diesem Beispielcode wird gezeigt, wie 'createDomainMapping' zum Erstellen eines neuen CDN aufgerufen werden kann.

// Schritt 1: Erhalten Sie Zugriff auf den SOAP-Client über 'vendor/autoload'. Der Pfad ist relativ zu der Position, an der
// das Script ausgeführt wird und muss möglicherweise entsprechend bearbeitet werden.
require_once './../vendor/autoload.php';

// Schritt 2: Geben Sie einen gültigen Benutzernamen und Schlüssel für die API an.
$apiUsername = '<Ihr Benutzername>' ;
$apiKey = '<Ihr API-Schlüssel>' ;

// Schritt 3: Initialisieren Sie den SOAP-Client für die entsprechende Klasse. In diesem Fall ist es die Klasse
// 'SoftLayer_Network_CdnMarketplace_Configuration_Mapping', die die Methode 'createDomainMapping' definiert.
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Schritt 4: Initialisieren Sie das Objekt '$input' und rufen Sie 'createDomainMapping' auf.
try {
    $input = new stdClass();

    // Geben Sie den Anbietername an.
    $input->vendorName = "akamai";

    // Geben Sie den Hostnamen und einen Wert für 'Cname' an.
    $input->domain = "beta.testingcdn.net";
    $input->cname = "beta.cdnedge.bluemix.net";

    // Geben Sie den Ursprungstyp an. Hier wird "HOST_SERVER" ausgewählt.
    $input->originType = "HOST_SERVER";

    // Geben Sie den Ursprungsadresse, das Protokoll und die Portnummer an.
    $input->origin = "testserver.testingcdn.net";
    $input->protocol = "HTTP";
    $input->httpPort = 80;

    // Geben Sie Optionen an.
    $input->respectHeaders = true;

    $cdnMapping = $client->createDomainMapping($input);
    print_r($cdnMapping);
} catch (\Exception $e) {
    die('createDomainMapping failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}

## Beispielcode zum Überprüfen der Domänenzuordnung

```
<?php

// Vor dem Aufrufen von 'verifyDomainMapping' muss der Kunde einen CNAME-Datensatz des angepassten Hostnamens zum DNS-Server hinzufügen.
// Mit diesem Code wird 'verifyDomainMapping' mit der 'UniqueId' aufgerufen, die als Teil von 'createDomainMapping' zurückgegeben wurde.
// Mit 'VerifyDomainMapping' wird überprüft, ob die Konfiguration von 'CNAME' abgeschlossen ist. Ist dies der Fall, wir der CDN-Status auf 'RUNNING' gesetzt.

// Schritt 1: Erhalten Sie Zugriff auf den SOAP-Client über 'vendor/autoload'. Der Pfad ist relativ zu der Position, an der
// das Script ausgeführt wird und muss möglicherweise entsprechend bearbeitet werden.
require_once './../vendor/autoload.php';

// Schritt 2: Geben Sie einen gültigen Benutzernamen und Schlüssel für die API an.
$apiUsername = '<Ihr Benutzername>' ;
$apiKey = '<Ihr API-Schlüssel>' ;

// Schritt 3: Initialisieren Sie den SOAP-Client für die entsprechende Klasse. In diesem Fall ist es die Klasse
// 'SoftLayer_Network_CdnMarketplace_Configuration_Mapping', die die Methode 'verifyDomainMapping' definiert.
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Schritt 4: Initialisieren Sie das Objekt '$uniqueId' und rufen Sie 'verifyDomainMapping' auf.
try {
    $uniqueId = 907987176256747;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}
