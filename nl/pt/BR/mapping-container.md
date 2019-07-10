---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Contêiner de mapeamento
{: #mapping-container}

A coleção `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` contém os atributos usados por nossas APIs de Mapeamento. Cada uma das APIs de Mapeamento retorna uma coleção desse tipo.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: o nome de um provedor do {{site.data.keyword.cloud}} CDN válido.
* `uniqueId`: um identificador de 10 dígitos, gerado pelo sistema, que é exclusivo para cada mapeamento. Gerada quando um mapeamento é criado.
* `domain`: nome do CDN primário. Também referido como `nome do host`.
* `protocol`: protocolo usado para configurar os serviços. Ele pode ser HTTP, HTTPS ou uma combinação dos
dois, HTTP_AND_HTTPS.
* `originType`: O tipo de Host de origem, atualmente 'HOST_SERVER' ou 'OBJECT_STORAGE'.
* `originHost`: endereço do servidor de origem (ou o nome do host ou o endereço IPv4 do servidor de origem),
que é o terminal do qual buscar conteúdo ou o nome do depósito em que o conteúdo é armazenado. Ele deve ser menor que 511 caracteres.
* `httpPort`: número da porta usada para o protocolo HTTP. A Akamai tem certas limitações em números de porta para as portas de HTTP e HTTPS. Veja as [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números de porta permitidos.
* `httpsPort`: número da porta usada para o protocolo HTTPS. A Akamai tem certas limitações em números de porta para as portas de HTTP e HTTPS. Consulte as [Perguntas mais frequentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números de porta permitidos.
* `certificateType`: tipo de certificado que está sendo usado por um mapeamento. Pode ser `WILDCARD_CERT` ou `SHARED_SAN_CERT`. O valor
será 0 para mapeamentos de HTTP.
* `cname`: registro de nome canônico que cria um alias do nome do host. Ele pode ser fornecido pelo usuário ou gerado pelo sistema. Se for fornecido pelo usuário, ele deverá ter menos que 64 caracteres alfanuméricos e deve ser exclusivo. Se não fornecido, um será gerado quando o mapeamento for criado.
* `respectHeaders`: um Valor booleano que, se configurado como 'true', fará as configurações do TTL na Origem substituírem as configurações do TTL do CDN.
* `header`: especifica as informações do cabeçalho do Host usadas pelo Servidor de origem.
* `status`: o status do mapeamento. O status pode ser CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED ou ERROR.
* `bucketName`: nome do depósito para seu armazenamento de objetos S3.
* `fileExtension`: extensões do arquivo que podem ser armazenadas em cache.
* `path`: o caminho para o seu armazenamento de objetos S3. Normalmente localizado na seção de API ou URL do armazenamento de objetos do seu serviço.
* `cacheKeyQueryRule`: as opções a seguir estão disponíveis para configurar o comportamento da Chave de cache:
  * `include-all`: inclui todos os argumentos de consulta **Padrão**
  * `ignore-all`: ignore todos os argumentos de consulta
  * `ignore: space separated query-args`: ignora esses argumentos de consulta específicos. Por exemplo, "ignore: query1 query2"
  * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, "include: query1 query2"

De nota em particular é o `uniqueId`, que é gerado quando um mapeamento é criado e é usado como um parâmetro
para chamadas API subsequentes.

Por exemplo, essa chamada para `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client-> listDomainMappingByUniqueid ('750352919747xxx');  
```

retornará um objeto semelhante a esse:

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
            [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
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
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

As chamadas para `stopDomainMapping` e `startDomainMapping`
retornarão o mesmo objeto de Mapeamento, com o `status` representando a diferença.

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
