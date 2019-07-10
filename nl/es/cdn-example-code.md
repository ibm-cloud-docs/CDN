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

# Ejemplos de código que utilizan la API de la CDN
{: #code-examples-using-the-cdn-api}

Este documento contiene llamadas de API de ejemplo y la salida resultante para numerosas API de CDN.

## Pasos generales necesarios para todas las llamadas de API
{: #general-steps-needed-for-all-api-calls}

El requisito previo es descargar e instalar el cliente de Soap desde [https://github.com/softlayer/softlayer-api-php-client ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/softlayer/softlayer-api-php-client)

  * Debe obtener acceso al SoapClient a través de `vendor/autoload`. La vía de acceso es relativa a la ubicación desde la que se ejecuta el script y es posible que sea necesario modificarla adecuadamente. En PHP, la sentencia tendrá un aspecto similar al siguiente: `require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * Todas las llamadas de API se autentican con el nombre de usuario y con una clave de API. Encontrará más información sobre cómo generar una clave de API en la página [Cómo empezar con la API de Softlayer ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer.github.io/article/getting-started/) page, under "Getting Your API Key".

      ```php
      $apiUsername = '<Su nombre de usuario>' ;
      $apiKey = '<Your clave de API>' ;
      ```
  * Inicialice SoapClient para la clase adecuada.

## Código de ejemplo para Listar proveedores
{: #example-code-for-listing-vendors}

En este caso es la clase SoftLayer_Network_CdnMarketplace_Vendor la que define la API `listVendors` y debe pasarse como parámetro a `\SoftLayer\SoapClient::getClient()`. Necesitará el nombre de un proveedor activo más tarde, cuando cree una Correlación de dominio.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

El código `listVendor` mostrará una matriz de proveedores, así como su estado y sus características. La salida de ejemplo muestra un proveedor activo, Akamai.

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

## Código de ejemplo para verificar el pedido
{: #example-code-to-verify-order}

Antes de realizar un pedido, la llamada a `verifyOrder` no es obligatoria, pero se recomienda. Se puede utilizar para verificar que una llamada posterior a `placeOrder` tendrá éxito. Puede encontrar más información sobre `verifyOrder` en la [documentación de la API de SoftLayer ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/).

En este caso es la clase `SoftLayer_Product_Order` la que define el método verifyOrder y se debe pasar como un parámetro a `\SoftLayer\SoapClient::getClient()`. Antes de la llamada a `verifyOrder`, tiene que crear el `$orderObject` utilizando `SoftLayer_Product_Package`.

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


## Código de ejemplo para realizar el pedido
{: #example-code-to-place-order}

Esta llamada de API es idéntica a la del ejemplo de código anterior, excepto que llama a `placeOrder` en lugar de llamar a `verifyOrder`. Puede encontrar más información sobre `placeOrder` en la [documentación de la API de SoftLayer ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/).

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


## Código de ejemplo para crear una CDN o Crear una correlación de dominios
{: #example-code-to-create-cdn-or-create-domain-mapping}

En este ejemplo se muestra cómo crear una nueva correlación de CDN utilizando la API `createDomainMapping`. Necesita un solo parámetro, un objeto `stdClass`. El SoapClient debe inicializarse utilizando la clase `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`, tal como se muestra en el ejemplo.

Si decide proporcionar un CNAME personalizado, **debe** finalizar con `.cdnedge.bluemix.net` o se producirá un error. Consulte [esta descripción](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-) para obtener reglas sobre cómo proporcionar su propio CNAME.
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

    // Los valores siguientes son necesarios
    $inputObject->vendorName = "akamai";
    $inputObject->origin = "origin.cdntesting.net";
    $inputObject->originType = "HOST_SERVER";
    $inputObject->domain = "api-testing.cdntesting.net";
    $inputObject->protocol = "HTTP";
    $inputObject->httpPort = 80;

    // El valor siguiente se necesita solamente para el protocolo HTTPS
    $inputObject->certificateType = "SHARED_SAN_CERT";

    // Los valores siguientes son opcionales
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

El ejemplo `createDomainMapping` mostrará los atributos de la CDN recién creada. Anote el `uniqueId`, ya que tendrá que proporcionarlo como un parámetro para muchas otras API. El resultado debe ser similar a este:

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

## Código de ejemplo para verificar una correlación de dominios
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping comprueba si la configuración del CNAME está completa y, si es así, mueve el estado de la CDN a RUNNING. Antes de llamar a `verifyDomainMapping` debe añadir un registro CNAME del nombre de host personalizado a su servidor de DNS.

Este ejemplo llama a `verifyDomainMapping` con el UniqueId que se ha devuelto como parte de `createDomainMapping`. El SoapClient debe inicializarse utilizando la clase `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`, tal como se muestra en el siguiente ejemplo.

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

Si el registro CNAME se ha añadido al servidor DNS, el `estado` de la CDN cambiará a RUNNING después de la llamada a `verifyDomainMapping`, tal y como se muestra en el ejemplo siguiente.

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


Si el registro CNAME no se ha añadido al servidor DNS, o si el servidor aún no se ha actualizado, el `estado` de la CDN será CNAME_CONFIGURATION, tal y como se muestra en el ejemplo siguiente.

Pueden necesitarse varios minutos (hasta 30) para que el encadenamiento de CNAME finalice.
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

Para asegurarse de que el registro CNAME está configurado correctamente, ejecute `dig <your domain>` en la línea de mandatos. El resultado debe ser similar a éste:

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

Aquí vemos el nombre de dominio correlacionado correctamente con el CNAME.
