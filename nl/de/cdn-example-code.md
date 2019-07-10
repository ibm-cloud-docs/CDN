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

# Codebeispiele mit der CDN-API
{: #code-examples-using-the-cdn-api}

Dieses Dokument enthält API-Beispielaufrufe und die sich daraus ergebende Ausgabe für eine Vielzahl an CDN-APIs.

## Für alle API-Aufrufe erforderliche allgemeine Schritte
{: #general-steps-needed-for-all-api-calls}

Als Voraussetzung muss der SOAP-Client von [https://github.com/softlayer/softlayer-api-php-client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/softlayer/softlayer-api-php-client) heruntergeladen und installiert werden.

  * Sie müssen über `vendor/autoload` Zugriff auf den SOAP-Client erhalten. Der Pfad ist relativ zu der Position, an der das Script ausgeführt wird; es kann erforderlich sein, ihn entsprechend zu ändern. In PHP ähnelt die Anweisung der folgenden Zeichenfolge: `require_once './../vendor/autoload.php';`

      ```php
      require_once __VERZEICHNIS__.'/vendor/autoload.php';
      ```

  * Alle API-Aufrufe werden mit dem Benutzernamen und einem API-Schlüssel authentifiziert. Weitere Informationen zum Generieren eines API-Schlüssels finden Sie auf der Seite [Softlayer-API - Einführung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/article/getting-started/) unter "API-Schlüssel abrufen".

      ```php
      $apiUsername = '<Ihr Benutzername>' ;
      $apiKey = '<Ihr API-Schlüssel>' ;
      ```
  * Initialisieren Sie den SOAP-Client für die entsprechende Klasse.

## Beispielcode zum Auflisten von Anbietern
{: #example-code-for-listing-vendors}

In diesem Fall definiert die Klasse 'SoftLayer_Network_CdnMarketplace_Vendor' die API `listVendors` und muss als Parameter an `\SoftLayer\SoapClient::getClient()` übergeben werden. Sie benötigen den Namen eines aktiven Anbieters später, wenn Sie eine Domänenzuordnung durchführen.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

Bei Verwendung des Codes `listVendor` wird eine Reihe von Anbietern zusammen mit ihren Statusangaben und Funktionen angezeigt. Als Beispielausgabe wird ein aktiver Anbieter mit dem Namen Akamai angezeigt.

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

## Beispielcode zum Prüfen der Bestellung
{: #example-code-to-verify-order}

Der Aufruf für `verifyOrder` muss nicht verbindlich vor der Erteilung eines Auftrags ausgeführt werden, diese Reihenfolge wird jedoch empfohlen. So kann überprüft werden, ob ein nachfolgender Aufruf für `placeOrder` erfolgreich ist. Weitere Informationen zu `verifyOrder` finden Sie in der [Dokumentation zur SoftLayer-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/).

In diesem Fall definiert die Klasse `SoftLayer_Product_Order` die Methode 'verifyOrder' und muss als Parameter an `\SoftLayer\SoapClient::getClient()` übergeben werden. Vor dem Aufruf für `verifyOrder` müssen Sie `$orderObject` mithilfe von `SoftLayer_Product_Package` erstellen.

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


## Beispielcode zum Aufgeben einer Bestellung
{: #example-code-to-place-order}

Abgesehen davon, dass `placeOrder` anstatt `verifyOrder` aufgerufen wird, ist dieser API-Aufruf mit dem vorherigen Codebeispiel identisch. Weitere Informationen zu `placeOrder` finden Sie in der [Dokumentation zur SoftLayer-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/).

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


## Beispielcode zum Erstellen eines CDN oder einer Domänenzuordnung
{: #example-code-to-create-cdn-or-create-domain-mapping}

Im folgenden Beispiel wird dargestellt, wie mithilfe der API `createDomainMapping` eine neue CDN-Zuordnung erstellt wird. Hierbei wird ein einzelner Parameter des Objekts `stdClass` verwendet. Der SOAP-Client muss mithilfe der Klasse `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` wie im Beispiel beschrieben initialisiert werden.

Falls Sie einen angepassten CNAME angeben, **muss** dieser mit `.cdnedge.bluemix.net` enden; andernfalls wird ein Fehler ausgelöst. Informationen zu den Regeln zum Angeben eines eigenen CNAME finden Sie in [dieser Beschreibung](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-).
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

    // Folgende Werte sind erforderlich
    $inputObject->vendorName = "akamai";
    $inputObject->origin = "origin.cdntesting.net";
    $inputObject->originType = "HOST_SERVER";
    $inputObject->domain = "api-testing.cdntesting.net";
    $inputObject->protocol = "HTTP";
    $inputObject->httpPort = 80;

    // Der folgende Wert ist nur für das HTTPS-Protokoll erforderlich
    $inputObject->certificateType = "SHARED_SAN_CERT";

    // Die folgenden Werte sind optional
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

Im Beispiel `createDomainMapping` werden die Attribute der neu erstellten CDN-Instanz angezeigt. Notieren Sie sich den Wert für `uniqueId`, da Sie ihn als Parameter für viele andere APIs angeben müssen. Die Ausgabe sollte der nachfolgenden ähnlich sein:

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

## Beispielcode zum Überprüfen der Domänenzuordnung
{: #example-code-to-verify-domain-mapping}

Von 'VerifyDomainMapping' wird überprüft, ob die CNAME-Konfiguration vollständig ist; falls dies der Fall ist, wechselt der CDN-Status in den Status AKTIV. Vor dem Aufrufen von `verifyDomainMapping` müssen Sie einen CNAME-Datensatz des angepassten Hostnamens zu Ihrem DNS-Server hinzufügen.

Im folgenden Beispiel wird `verifyDomainMapping` mit dem Wert für 'UniqueId' aufgerufen, der als Teil von `createDomainMapping` zurückgegeben wurde. Der SOAP-Client muss mithilfe der Klasse `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` wie im folgenden Beispiel beschrieben initialisiert werden.

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

Falls der CNAME-Datensatz zum DNS-Server hinzugefügt wurde, ändert sich der Wert für `status` der CDN-Instanz nach dem Aufruf für `verifyDomainMapping` wie im folgenden Beispiel dargestellt in AKTIV.

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


Falls der CNAME-Datensatz nicht zum DNS-Server hinzugefügt wurde oder der Server noch nicht aktualisiert wurde, lautet der Wert für `status` für die CDN-Instanz wie im folgenden Beispiel angegeben 'CNAME_CONFIGURATION'.

Es kann einige Minuten dauern (bis zu 30), bis die CNAME-Verkettung abgeschlossen ist.
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

Wenn Sie sicherstellen möchten, dass der CNAME-Datensatz ordnungsgemäß konfiguriert ist, führen Sie `dig <your domain>` in der Befehlszeile aus. Die Ausgabe sollte der nachfolgenden ähnlich sein:

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

Nachfolgend wird der Domänenname angezeigt, der dem CNAME ordnungsgemäß zugeordnet ist.
