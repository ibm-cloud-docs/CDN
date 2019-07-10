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

# Exemplos de Códigos Usando a API CDN
{: #code-examples-using-the-cdn-api}

Este documento contém chamadas API de exemplo e a saída resultante para várias APIs do CDN.

## Etapas gerais necessárias para todas as chamadas API
{: #general-steps-needed-for-all-api-calls}

O pré-requisito é fazer download e instalar o Soap Client por meio do [https://github.com/softlayer/softlayer-api-php-client ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/softlayer/softlayer-api-php-client)

  * Deve-se obter acesso ao SoapClient por meio de `vendor/autoload`. O caminho é
relativo ao local em que o script é executado e pode precisar ser modificado apropriadamente. Em PHP, a
instrução será semelhante a esta: `require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php ';
      ```

  * Todas as chamadas API são autenticadas com seu nome de usuário e com uma apiKey. Mais informações sobre como gerar uma apiKey podem ser localizadas na [Página de introdução da API do Softlayer ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/article/getting-started/), em "Obtendo sua chave de API".

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * Inicialize o SoapClient para a classe apropriada.

## Código de Exemplo para Vendors de Listagem
{: #example-code-for-listing-vendors}

Nesse caso, é a classe SoftLayer_Network_CdnMarketplace_Vendor que define a API
`listVendors` e deve ser transmitida como um parâmetro para
`\SoftLayer\SoapClient::getClient()`. Posteriormente, será necessário o nome de um fornecedor
ativo ao criar um Mapeamento de Domínio

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey); try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

O código `listVendor` exibirá uma matriz de fornecedores, bem como seus status e
recursos. A saída de exemplo mostra um fornecedor ativo, o Akamai.

```php
Matriz (
    [0] => stdClass Object (
            [featureSummary] => Performance, Reliability and Scale
            [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
            [status] => ACTIVE
            [vendorName] => akamai
        )

)
```

{: codeblock}

## Código de exemplo para verificar o pedido
{: #example-code-to-verify-order}

A chamada para `verifyOrder` não é obrigatória antes de fazer um pedido, mas é
recomendada. Ela pode ser usada para verificar se uma chamada subsequente para `placeOrder`
será bem-sucedida. Mais informações sobre `verifyOrder` podem ser localizadas na [Documentação da API do SoftLayer ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/).

Nesse caso, ela é a classe `SoftLayer_Product_Order`, que define o método
verifyOrder e deve ser transmitida como um parâmetro para `\SoftLayer\SoapClient::getClient()`. Antes da chamada para `verifyOrder`, é necessário construir o `$orderObject` usando
o `SoftLayer_Product_Package`.

```php
$client = \SoftLayer\SoapClient ::getClient ('SoftLayer_Product_Package', nulo, $apiUsername, $apiKey);

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

    $orderData = new stdClass(); $orderData->packageId = $package->id; $orderData->prices = $package->itemPrices;

    $orderObject = new SoapVar( $orderData, SOAP_ENC_OBJECT, 'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service', 'http://api.service.softlayer.com/soap/v3/'
    );

    $productOrderClient = \SoftLayer\SoapClient ::getClient ('SoftLayer_Product_Order', nulo, $apiUsername, $apiKey);

    $result = $productOrderClient->verifyOrder($orderObject);
    print_r($result);
    print_r("\n");
}

catch (\Exception $e) {
    die('Verify Order failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}


## Código de exemplo para fazer pedido
{: #example-code-to-place-order}

Esta chamada API é idêntica ao exemplo de código anterior, exceto que ela chama `placeOrder`,
em vez de `verifyOrder`. Mais informações sobre `placeOrder` podem ser localizadas na [Documentação da API do SoftLayer ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/).

```php

$client = \SoftLayer\SoapClient ::getClient ('SoftLayer_Product_Package', nulo, $apiUsername, $apiKey);

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

    $orderData = new stdClass(); $orderData->packageId = $package->id; $orderData->prices = $package->itemPrices;

    $orderObject = new SoapVar( $orderData, SOAP_ENC_OBJECT, 'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service', 'http://api.service.softlayer.com/soap/v3/'
    );

    $productOrderClient = \SoftLayer\SoapClient ::getClient ('SoftLayer_Product_Order', nulo, $apiUsername, $apiKey);

    $result = $productOrderClient->placeOrder($orderObject);
    print_r($result);
    print_r("\n");
}

catch (\Exception $e) {
    die('Place order failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}


## Código de exemplo para criar CDN ou Criar Mapeamento de Domínio
{: #example-code-to-create-cdn-or-create-domain-mapping}

Este exemplo mostra como criar um novo mapeamento de CDN utilizando a API `createDomainMapping`. Ele utiliza um único parâmetro de um objeto `stdClass`. O SoapClient deve ser inicializado
usando a classe `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`, conforme
mostrado no exemplo.

Se você optar por fornecer um CNAME customizado, ele **deverá** terminar com `.cdnedge.bluemix.net` ou um erro será lançado. Consulte [esta
descrição](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-) para obter as regras sobre como fornecer seu próprio CNAME.
{: important}

```php

$client = \SoftLayer\SoapClient::getClient( 'SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey );


try {
    $inputObject = new stdClass ();

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

O exemplo `createDomainMapping` exibirá os atributos do CDN recém-criado. Anote o `uniqueId`, já que será necessário fornecê-lo como um parâmetro para muitas outras
APIs. A saída deve ser semelhante a esta:

```php
Matriz (
    [0] => stdClass Object (
            [bucketName] => mybucket
            [cacheKeyQueryRule] => include-all
            [certificateType] => SHARED_SAN_CERT
            [cname] => api-testing.cdnedge.bluemix.net
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net [originType] => HOST_SERVER [path] => /media/
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

## Código de Exemplo para Verificar Mapeamento de Domínio
{: #example-code-to-verify-domain-mapping}

O VerifyDomainMapping verifica se a configuração do CNAME está concluída e, em caso afirmativo, muda
o status do CDN para RUNNING. Antes de chamar `verifyDomainMapping`, deve-se incluir um
registro do CNAME do Nome do host customizado em seu servidor DNS.

Este exemplo chama `verifyDomainMapping` com o UniqueId que foi retornado como
parte de `createDomainMapping`. O SoapClient deve ser inicializado usando a classe
`SoftLayer_Network_CdnMarketplace_Configuration_Mapping`, conforme mostrado no exemplo a seguir.

```php

$client = \SoftLayer\SoapClient::getClient( 'SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey );

try {
    $uniqueId = 610345992629xxx;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}

Se o registro CNAME tiver sido incluído em seu servidor DNS, o `status` de seu CDN
mudará para RUNNING após a chamada para `verifyDomainMapping`, conforme mostrado no
exemplo a seguir.

```php

    [0] => stdClass Object (
          ...

            [status] => RUNNING
            [uniqueId] => 610345992629xxx

          ...
        )
```
{: codeblock}


Se seu registro do CNAME não tiver sido incluído no servidor DNS ou seu servidor ainda não foi
atualizado, o `status` de seu CDN será CNAME_CONFIGURATION, conforme mostrado no exemplo
a seguir.

Pode levar vários minutos (até 30) para que o encadeamento de CNAME seja concluído.
{: note}

```php
Matriz (
    [0] => stdClass Object (
          ...

            [status] => CNAME_CONFIGURATION
            [uniqueId] => 610345992629xxx

          ...
        )

)
```
{: codeblock}

Para certificar-se de que o seu registro CNAME esteja configurado corretamente, execute `dig <your domain>` na linha de comandos. A saída deve ser semelhante a esta:

```
;; ANSWER SECTION: api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

Aqui é exibido o nome de domínio mapeado corretamente para o CNAME.
