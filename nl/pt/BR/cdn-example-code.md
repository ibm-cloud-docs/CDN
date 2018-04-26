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

# Exemplos de Códigos Usando a API CDN

## Código de Exemplo de Listagem de Fornecedores

O pré-requisito é fazer download e instalar o Soap Client de https://github.com/softlayer/softlayer-api-php-client

```
// Código de exemplo para demonstrar a chamada API de CDN Soap
// Este código imprime os fornecedores suportados pelo IBM CDN Service no terminal do qual é executado

// Etapa 1: Obter acesso ao SoapClient por meio do fornecedor/carregamento automático. O caminho é relativo a de onde
// o script é executado e pode precisar ser modificado apropriadamente
require_once './../vendor/autoload.php';

// Etapa 2: Fornecer Nome de Usuário da API e Chave API válidos
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Etapa 3: Inicializar SoapClient para a classe apropriada. Nesse caso, é a classe
// SoftLayer_Network_CdnMarketplace_Vendor que define o método listVendors
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);

// Etapa 4: Chamar o método listVendors e imprimir os fornecedores
// Quando bem-sucedido, o script exibirá os fornecedores no terminal
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

## Código de exemplo para verificar o pedido 

```
<?php

// Este código verifica se um pedido CDN pode ser feito com êxito

// Etapa 1: Obter acesso ao SoapClient por meio do fornecedor/carregamento automático. O caminho é relativo a de onde
// o script é executado e pode precisar ser modificado apropriadamente
require_once './../vendor/autoload.php';

// Etapa 2: Fornecer Nome de Usuário da API e Chave API válidos
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// Etapa 3: Inicializar um cliente da API para a classe SoftLayer_Product_Package
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Etapa 4: Configurar Filtro e Máscara para obter o objeto de pacote certo
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

    // Etapa 5: Inicializar o objeto OrderData
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Etapa 6: Criar SoapVar orderObject
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Etapa 7: Obter Pedido do Produto do Cliente e chamar verifyOrder
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


## Código de exemplo para fazer pedido

```
<?php

// Este código é idêntico a VerifyOrder, exceto a Etapa 7, que chama placeOrder. 
// PlaceOrder deve ser chamado apenas após um verifyOrder bem-sucedido 

// Etapa 1: Obter acesso ao SoapClient por meio do fornecedor/carregamento automático. O caminho é relativo a de onde
// o script é executado e pode precisar ser modificado apropriadamente
require_once './../vendor/autoload.php';

// Etapa 2: Fornecer Nome de Usuário da API e Chave API válidos
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// Etapa 3: Inicializar um cliente da API para a classe SoftLayer_Product_Package
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Etapa 4: Configurar Filtro e Máscara para obter o objeto de pacote certo
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

    // Etapa 5: Inicializar o objeto OrderData
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Etapa 6: Criar SoapVar orderObject
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Etapa 7: Obter Pedido do Produto do Cliente e chamar placeOrder
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


## Código de exemplo para criar CDN ou Criar Mapeamento de Domínio

```
<?php

// Este é o código de exemplo para mostrar como chamar createDomainMapping para criar um novo CDN

// Etapa 1: Obter acesso ao SoapClient por meio do fornecedor/carregamento automático. O caminho é relativo a de onde
// o script é executado e pode precisar ser modificado apropriadamente
require_once './../vendor/autoload.php';

// Etapa 2: Fornecer Nome de Usuário da API e Chave API válidos
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Etapa 3: Inicializar SoapClient para a classe apropriada. Nesse caso, é a classe
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping que define o método createDomainMapping
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Etapa 4: Inicializar o objeto $input e chamar createDomainMapping
try {
    $input = new stdClass();

    // Fornecer o nome do fornecedor
    $input->vendorName = "akamai";

    // Especificar o Nome do Host e Cname
    $input->domain = "beta.testingcdn.net";
    $input->cname = "beta.cdnedge.bluemix.net";

    // Especificar o tipo de origem. Aqui, "HOST_SERVER" é escolhido
    $input->originType = "HOST_SERVER";

    // Especificar o endereço de origem, o protocolo e o número da porta
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

## Código de Exemplo para Verificar Mapeamento de Domínio

```
<?php

// Antes de chamar verifyDomainMapping, o cliente deve incluir um registro CNAME do Nome do Host customizado para o servidor DNS.
// Esse código chama verifyDomainMapping com o UniqueId que foi retornado como parte de createDomainMapping.
// VerifyDomainMapping verifica se a configuração CNAME está concluída e, se estiver, muda o status CDN para o status RUNNING

// Etapa 1: Obter acesso ao SoapClient por meio do fornecedor/carregamento automático. O caminho é relativo a de onde
// o script é executado e pode precisar ser modificado apropriadamente
require_once './../vendor/autoload.php';

// Etapa 2: Fornecer Nome de Usuário da API e Chave API válidos
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Etapa 3: Inicializar SoapClient para a classe apropriada. Nesse caso, é a classe
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping que define o método verifyDomainMapping
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Etapa 4: Inicializar o $uniqueId e chamar verifyDomainMapping
try {
    $uniqueId = 907987176256747;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}
