---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Referência da API CDN
{: #cdn-api-reference}

A {{site.data.keyword.cloud}} Infrastructure Application Programming Interface (comumente chamada de SLAPI), fornecida pelo {{site.data.keyword.cloud}}, é a interface de desenvolvimento que fornece aos desenvolvedores e administradores do sistema a interação direta com o sistema back-end de infraestrutura do {{site.data.keyword.cloud_notm}}.

O SLAPI implementa muitos dos recursos no Portal do Cliente: se uma interação é possível no Portal do Cliente, ela também pode ser realizada no SLAPI. Como é possível interagir com todas as partes do ambiente de Infraestrutura do {{site.data.keyword.cloud_notm}} programaticamente, dentro do SLAPI, é possível usar a API para automatizar tarefas.

O SLAPI é um sistema de Chamada de Procedimento Remoto (RPC). Cada chamada envolve enviar dados para um terminal de API e receber dados estruturados em troca. O formato usado para enviar e receber dados com a SLAPI depende de qual implementação da API você escolhe. O SLAPI atualmente usa SOAP, XML-RPC ou REST para transmissão de dados.

Para obter mais informações sobre a SLAPI ou sobre as APIs de serviço do {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN), consulte os recursos a seguir na {{site.data.keyword.cloud_notm}} Development Network:

* [Visão geral do SLAPI](https://softlayer.github.io/ )
* [Introdução ao SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [Guia da API PHP Soap](https://softlayer.github.io/article/php/ )

----

Para começar, aqui está uma sequência de chamada API recomendada para seguir:
* `listVendors` - Fornece a lista de fornecedores suportados
* `verifyOrder` - Verifica se o pedido pode ser feito
* `placeOrder`  - Cria a conta do CDN com um determinado fornecedor. Até 10 Mapeamentos de CDN podem ser
criados após uma chamada placeOrder bem-sucedida.
* `createDomainMapping` - Cria os mapeamentos de CDN
* `verifyDomainMapping` - Muda o status do CDN para _EM EXECUÇÃO_

É possível usar as outras APIs depois de ter seguido a sequência anterior.

[O Código de Exemplo está disponível para cada etapa nesta sequência de chamada.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

**Deve-se** usar o nome de usuário da API e a chave de API de um usuário com a permissão `CDN_ACCOUNT_MANAGE` para a maioria das chamadas API mostradas neste documento. Verifique o usuário
principal da sua conta se você precisar que essa permissão seja ativada para você. (Cada conta do cliente IBM Cloud é fornecida com
um usuário principal.)
{: note}

----
## API para Fornecedor
{: #api-for-vendor}

### listVendors
Esta API permite que o usuário liste os Fornecedores de CDN suportados. O `vendorName` é necessário para
criar uma conta CDN e começar o pedido do seu CDN.

* **Parâmetros necessários**: nenhum
* **Retorno**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Vendor`

  O contêiner Fornecedor e um exemplo de uso podem ser visualizados aqui: [contêiner
Fornecedor](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## API para conta
{: #api-for-account}

### verifyCdnAccountExists
Verifica se existe uma conta CDN para o usuário que está chamando a API, para o `vendorName` especificado.

* **Parâmetros necessários**: `vendorName`: forneça o nome de um provedor CDN válido.
* **Retornar**: `true` se uma conta existir, caso contrário, `false`.

----
## API para mapeamento de domínio
{: #api-for-domain-mapping}

### createDomainMapping
Usando as entradas fornecidas, essa função cria um mapeamento de domínio para o fornecedor especificado e o associa ao ID da conta de infraestrutura do {{site.data.keyword.cloud_notm}} do usuário. A conta CDN deve primeiro ser criada usando `placeOrder` para que essa API funcione (consulte um exemplo da chamada API `placeOrder` nos [Exemplos de código](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api). Depois de criar o CDN com sucesso, um `defaultTTL` será criado com um valor de 3.600 segundos.

* **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  É possível visualizar todos os atributos no Contêiner de entrada aqui:

  [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

  Os atributos a seguir são parte do contêiner de entrada e podem ser fornecidos ao criar um mapeamento de domínio (os
atributos são opcionais, a menos que indicado de outra forma):
    * `vendorName`: **necessário** forneça o nome de um provedor válido do IBM Cloud CDN.
    * `origin`: **necessário** forneça o endereço do Servidor de origem como uma sequência.
    * `originType`: **necessário** O tipo de origem pode ser `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `domain`: **necessário** forneça o seu nome do host como uma sequência.
    * `protocol`: **necessário** Os protocolos suportados são `HTTP`, `HTTPS` ou `HTTP_AND_HTTPS`.
    * ` certificateType `:  ** obrigatório **  para o protocolo HTTPS. ` SHARED_SAN_CERT  ` ou  ` WILDCARD_CERT `
    * `path`: caminho por meio do qual o conteúdo em cache será entregue. O caminho padrão é  ` / * `
    * `httpPort` e/ou `httpsPort`: (**necessário** para servidor host)
Essas duas opções devem corresponder ao protocolo desejado. Se o protocolo for `HTTP`, então `httpPort` deverá ser configurado e `httpsPort` _não_ deverá ser configurado. Da mesma forma, se o protocolo for `HTTPS`, então `httpsPort` deverá ser configurado e `httpPort` _não_ deverá ser configurado. Se o protocolo for `HTTP_AND_HTTPS`, então _ambos_ `httpPort` e `httpsPort` _deverão_ ser configurados. A Akamai tem certas limitações em números de porta. Veja as [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números de porta permitidos.
    * `header`: especifica as informações do cabeçalho do host usadas pelo Servidor de origem
    * `respectHeader`: um valor booleano que, se configurado como `true`, faz as configurações do TTL na Origem substituam as configurações do TTL do CDN.
    * `cname`: forneça um alias para o nome do host. Será gerado se um não for fornecido.
    * `bucketName`: Nome do bucket (**necessário** apenas para o Object Storage) para o seu Object Storage do S3.
    * `fileExtension`: extensões do arquivo (opcionais para o Object Storage) que podem ser armazenadas em cache.
    * `cacheKeyQueryRule`: as opções a seguir estão disponíveis para configurar o comportamento da chave do
cache. Se nenhum argumento `cacheKeyQueryRule` for fornecido, ele será padronizado como "include-all"
      * `include-all` - inclui todos os argumentos de consulta **padrão**
      * `ignore-all` - ignora todos os argumentos de consulta
      * `ignore: space separated query-args` - ignora esses argumentos de consulta específicos. Por exemplo, `ignore: query1 query2`
      * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, `include: query1 query2`

* **Retorno**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  **NOTA**: a coleção fornece um valor `uniqueId`, que precisa ser enviado como
entrada para as chamadas API subsequentes relacionadas ao mapeamento e ao caminho de origem.

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Exclui o mapeamento de domínio com base no `uniqueId`. O mapeamento de domínio deverá estar em um dos
seguintes estados: _RUNNING_, _STOPPED_, _EXCLUÍDO_, _ERROR_,
_CNAME_CONFIGURATION_ ou _SSL_CONFIGURATION_.

* **Parâmetros necessários**: `uniqueId`: o ID exclusivo do mapeamento a ser
excluído
* **Retorna**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
  [Visualizar o contêiner de mapeamento](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Verifica o status do CDN e atualiza o `status` do mapeamento CDN se ele mudar. Quando um mapeamento
CDN é criado inicialmente, seu status é mostrado como _CNAME_CONFIGURATION_. Neste ponto, deve-se atualizar o registro
DNS para que o mapeamento CDN aponte o nome do host para o CNAME. Verifique com seu provedor DNS se você tiver perguntas sobre como a atualização é feita e quanto tempo pode levar para a mudança se propagar na Internet. Geralmente, deve levar de 15 a 30 minutos. Após esse tempo, essa API `verifyDomainMapping` deverá ser chamada para verificar se a cadeia CNAME foi concluída. Se
a cadeia CNAME estiver concluída, o status do mapeamento CDN mudará para _EM EXECUÇÃO_.

Esta API pode ser chamada a qualquer momento para obter o status de mapeamento do CDN mais recente. O mapeamento de domínio
deverá estar em um dos seguintes estados: _EM EXECUÇÃO_ ou _CNAME_CONFIGURATION_.

* **Parâmetros necessários**: `uniqueId`: ID exclusivo do mapeamento que você deseja
verificar
* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Inicia um mapeamento de domínio do CDN com base no `uniqueId`. Para ser iniciado, o mapeamento de domínio deverá estar em um estado _STOPPED_.

* **Parâmetros necessários**: `uniqueId`: ID exclusivo do mapeamento a ser
iniciado
* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Para um mapeamento de domínio do CDN com base no `uniqueId`. Para iniciar a parada, o mapeamento de domínio deverá estar em um estado _RUNNING_.

* **Parâmetros necessários**: `uniqueId`: ID exclusivo do mapeamento a ser parado
* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
Permite que o usuário atualize propriedades do mapeamento identificado pelo `uniqueId`. É possível mudar os campos a seguir: argumentos `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header`, `cacheKeyQueryRule` e, se o seu tipo de origem for Object Storage, o `bucketName` e o `fileExtension` também poderão ser mudados. Para que uma atualização ocorra, o mapeamento de domínio deverá estar em um estado _RUNNING_.

* **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  É possível visualizar todos os atributos no Contêiner de entrada aqui: [Visualizar o Contêiner de entrada](/docs/infrastructure/CDN?topic=CDN-input-container)

  Os atributos a seguir são parte do contêiner de entrada e **devem** ser fornecidos ao atualizar
um mapeamento de domínio:
    * `vendorName`: forneça o nome do provedor CDN para esse mapeamento.
    * `path`: forneça o caminho atual para esse mapeamento
    * `origin`: forneça o endereço do servidor de origem como uma sequência.
    * `originType`: tipo de origem pode ser `HOST_SERVER` ou
`OBJECT_STORAGE`.
    * `domain`: forneça o nome do host.
    * `protocol`: protocolos suportados são `HTTP`, `HTTPS` ou
`HTTP_AND_HTTPS`.
    * `httpPort` e/ou `httpsPort`: essas duas opções devem corresponder ao protocolo
desejado. Se o protocolo for `HTTP`, então `httpPort` deverá ser configurado e `httpsPort` _não_ deverá ser configurado. Da mesma forma, se o protocolo for `HTTPS`, então `httpsPort` deverá ser configurado e `httpPort` _não_ deverá ser configurado. Se o protocolo for `HTTP_AND_HTTPS`, então _ambos_ `httpPort` e `httpsPort` _deverão_ ser configurados. A Akamai tem certas limitações em números de porta. Veja as [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números de porta permitidos.
    * `header`: especifica as informações do cabeçalho do host usadas pelo Servidor de origem
    * `respectHeader`: um valor booleano que, se configurado como
`true`, faz com que as configurações de TTL na Origem substituam as configurações de TTL do CDN.
    * `uniqueId`: gerado após o mapeamento ser criado.
    * `cname`: forneça o cname. Um foi gerado quando o mapeamento foi criado se você não forneceu um.
    * `bucketName`: Nome do bucket (**necessário** apenas para o Object Storage) para o seu Object Storage do S3.
    * `fileExtension`: extensões do arquivo (**necessárias** apenas para o Object Storage) que podem ser armazenadas em cache.
    * `cacheKeyQueryRule`: regras de comportamento de chave de cache somente podem ser atualizadas para
mapeamentos CDN criados _após_ 16/11/2017. As opções a seguir estão disponíveis para configurar o comportamento da
chave de cache:
      * `include-all` - inclui todos os argumentos de consulta **padrão**
      * `ignore-all` - ignora todos os argumentos de consulta
      * `ignore: space separated query-args` - ignora esses argumentos de consulta específicos. Por exemplo, `ignore: query1 query2`
      * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, `include: query1 query2`
* ** Retornar **  uma coleta do tipo  ` SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping `

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Retorna uma coleção de todos os mapeamentos de domínio para o cliente atual.

* **Parâmetros necessários**: nenhum
* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Retorna uma coleção com um único objeto de domínio com base no `uniqueId` de um CDN.

* **Parâmetros necessários**: `uniqueId`: ID exclusivo do mapeamento a ser
retornado
* **Retornar**: uma coleta de objeto único do tipo
`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizar o Mapeamento de contêiner](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## APIs para origem
{: #apis-for-origin}

### createOriginPath
Cria um Caminho de origem para um CDN existente e para um cliente específico. O caminho de origem pode se basear em um
servidor host ou Object Storage. Para criar o caminho de origem, o mapeamento de domínio deverá estar em um
estado _EM EXECUÇÃO_ ou _CNAME_CONFIGURATION_.

* **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  É possível visualizar todos os atributos no Contêiner de entrada aqui:

  [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

  Os atributos a seguir são parte do contêiner de entrada e podem ser fornecidos ao criar um caminho de origem (atributos são opcionais, a menos que indicado de outra forma):
    * `vendorName`: **necessário** forneça o nome de um provedor válido do IBM Cloud CDN.
    * `origin`: **necessário** forneça o endereço do Servidor de origem como uma sequência.
    * `originType`: **necessário** O tipo de origem pode ser `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `domain`: **necessário** forneça o seu nome do host como uma sequência.
    * `protocol`: **necessário** Os protocolos suportados são `HTTP`, `HTTPS` ou `HTTP_AND_HTTPS`.
    * `path`: caminho por meio do qual o conteúdo em cache será entregue. Deve começar com o caminho de
mapeamento. Por exemplo, se o caminho de mapeamento for `/test`, então, o seu caminho de origem poderá ser `/test/media`
    * `httpPort` e/ou `httpsPort`: **necessário** Essas duas opções devem corresponder ao protocolo desejado. Se o protocolo for `HTTP`, então `httpPort` deverá ser configurado e `httpsPort` _não_ deverá ser configurado. Da mesma forma, se o protocolo for `HTTPS`, então `httpsPort` deverá ser configurado e `httpPort` _não_ deverá ser configurado. Se o protocolo for `HTTP_AND_HTTPS`, então _ambos_ `httpPort` e `httpsPort` _deverão_ ser configurados. A Akamai tem certas limitações em números de porta. Veja as [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números de porta permitidos.
    * `header`: especifica as informações do cabeçalho do host usadas pelo Servidor de origem
    * `uniqueId`: **necessário** gerado após o mapeamento ser criado.
    * `cname`: forneça um alias para o nome do host. Se você não forneceu um cname exclusivo, um foi gerado para você quando o mapeamento foi criado.
    * `bucketName`: (**necessário** para Object Storage) nome do depósito para seu Object Storage S3.
    * `fileExtension`: extensões do arquivo (opcionais para o Object Storage) que podem ser armazenadas em cache.
    * `cacheKeyQueryRule`: as opções a seguir estão disponíveis para configurar o comportamento da Chave de cache:
      * `include-all` - inclui todos os argumentos de consulta **padrão**
      * `ignore-all` - ignora todos os argumentos de consulta
      * `ignore: space separated query-args` - ignora esses argumentos de consulta específicos. Por exemplo, `ignore: query1 query2`
      * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, `include: query1 query2`

* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Visualizar o Contêiner do caminho de origem](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Atualiza um Caminho de origem existente para um mapeamento existente e para um cliente específico. Não é possível mudar o tipo de origem com essa API. As propriedades a seguir podem mudar: `path`, `origin`, `httpPort` e os argumentos `httpsPort`, `header` e `cacheKeyQueryRule`. Para ser atualizado, o mapeamento de domínio deve estar em um estado _EM
EXECUÇÃO_ ou _CNAME_CONFIGURATION_.

* **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  É possível visualizar todos os atributos no Contêiner de entrada aqui:

  [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

  Os atributos a seguir são parte do contêiner de entrada e podem ser fornecidos ao atualizar um caminho de origem (atributos são opcionais, a menos que indicado de outra forma):
    * `oldPath`: **necessário** caminho atual para ser mudado
    * `origin`: (**necessário** se estiver sendo atualizado) Forneça o endereço do servidor de origem como uma sequência.
    * `originType`: **necessário** O tipo de origem pode ser `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `path`: **necessário** Novo caminho para ser incluído. Relativo ao caminho de
mapeamento.
    * `httpPort` e/ou `httpsPort`: (**necessário** para servidor host, se estiver sendo atualizado) Essas duas opções devem corresponder ao protocolo desejado. Se o protocolo for `HTTP`, então `httpPort` deverá ser configurado e `httpsPort` _não_ deverá ser configurado. Da mesma forma, se o protocolo for `HTTPS`, então `httpsPort` deverá ser configurado e `httpPort` _não_ deverá ser configurado. Se o protocolo for `HTTP_AND_HTTPS`, então _ambos_ `httpPort` e `httpsPort` _deverão_ ser configurados. A Akamai tem certas limitações em números de porta. Veja as [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números de porta permitidos.
    * `uniqueId`: **necessário** ID exclusivo do mapeamento ao qual essa origem pertence
    * `bucketName`: Nome do bucket (**necessário** apenas para o Object Storage) para o seu Object Storage do S3.
    * `fileExtension`: extensões do arquivo (**necessárias** apenas para o Object Storage) que podem ser armazenadas em cache.
    * `cacheKeyQueryRule`: (**necessário** se estiver sendo atualizado) Regras de comportamento da chave do cache somente podem ser atualizadas para caminhos de origem criados _após_ 16/11/2017. As seguintes opções estão disponíveis para configurar o comportamento da chave do cache:
      * `include-all` - inclui todos os argumentos de consulta **padrão**
      * `ignore-all` - ignora todos os argumentos de consulta
      * `ignore: space separated query-args` - ignora esses argumentos de consulta específicos. Por exemplo, `ignore: query1 query2`
      * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, `include: query1 query2`

* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Visualizar o Contêiner do caminho de origem](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Exclui um caminho de origem existente para um CDN existente e para um cliente específico. Para ser excluído, o mapeamento de domínio deve estar em um estado _EM EXECUÇÃO_ ou _CNAME_CONFIGURATION_.

* **Parâmetros necessários**:
  * `uniqueId`: O ID exclusivo do mapeamento ao qual esse caminho pertence
  * `path`: caminho para ser excluído

* **Retornar**: uma mensagem de status se a exclusão for bem-sucedida, caso contrário, uma exceção será
lançada.

----
### listOriginPath
Lista os caminhos de origem para um mapeamento existente com base no `uniqueId`.

* **Parâmetros necessários**:
  * `uniqueId`: forneça o uniqueId do mapeamento para o qual você deseja listar Caminhos de origem.
* **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Visualizar o Contêiner do caminho de origem](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## API para limpeza
{: #api-for-purge}

### Classe do contêiner para limpeza:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /** * @var string */ public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
Cria um registro de limpeza e insere-o no banco de dados.

* **Parâmetros**:
  * `uniqueId`: ID exclusivo do mapeamento para o qual a limpeza será criada
  * `path`: caminho da limpeza a ser criado

* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
Retorna o histórico de limpeza para um CDN com base no status `uniqueId` e `salved`. (Por padrão, o valor de `saved` é configurado como _UNSAVED_.)

* **Parâmetros**:
  * `uniqueId`: ID exclusivo do mapeamento para o qual recuperar o histórico de limpeza
  * `saved`: retornar limpezas que foram _SALVAS_ ou _NÃO SALVAS_

* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
Atualiza o status da entrada do caminho de limpeza para indicar se esse caminho de limpeza deve ser salvo. Cria uma nova limpeza `saved` se um caminho de limpeza estiver salvo. Exclui um registro de limpeza salvo se o caminho for `unsaved`.

* **Parâmetros**:
  * `uniqueId`: ID exclusivo do mapeamento ao qual a limpeza pertence
  * `path`: caminho da limpeza a ser salvo ou não salvo
  * `saved`: _SALVO_ ou _NÃO SALVO_

* **Retornar**: uma coleção de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
## API para tempo de vida
{: #api-for-time-to-live}

### Variáveis de classe TimeToLive:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /** * @var string */ public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
Cria um novo objeto `TimeToLive` e insere-o no banco de dados.

 * **Parâmetros**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Retorno**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Atualiza um objeto `TimeToLive` existente. Se as entradas _oldTtl_ e _newTtl_ forem iguais, ele sairá antecipadamente.

 * **Parâmetros**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Retorno**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Exclui um objeto `TimeToLive` existente do banco de dados.

 * **Parâmetros**: `string` `uniqueId`, `string` `pathName`
 * **Retorno**: uma sequência com o status da exclusão
___
### listTtl
Lista objetos `TimeToLive` existentes com base no `uniqueId` de um CDN.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma matriz de objetos do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`

 ----
## API para métricas
{: #api-for-metrics}

[ Visualizar o Contêiner de Métricas ](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
Retorna o número total de estatísticas predeterminadas para exibição direta (sem gráfico) para a conta de um cliente, durante um período de tempo especificado.

 * **Parâmetros**:
   * ` vendorName `
   * ` startDate `
   * ` endDate `
   * ` frequência `

 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingUsageMetrics
Retorna o número total de estatísticas predeterminadas para exibição direta para o mapeamento especificado. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**:
   * ` mappingUniqueId `
   * ` startDate `
   * ` endDate `
   * ` frequência `

 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsMetrics
Retorna o número total de ocorrências em uma certa frequência durante um intervalo de tempo especificado, por mapeamento de domínio. A frequência pode ser 'dia', 'semana' e 'mês', em que cada intervalo é um ponto de plot para um gráfico. Os dados de retorno são ordenados com base em `startDate`, `endDate` e `frequency`. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**:
   * ` mappingUniqueId `
   * ` startDate `
   * ` endDate `
   * ` frequência `

 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Retorna o número total de ocorrências em uma certa frequência durante um intervalo de tempo especificado. A frequência pode ser 'dia', 'semana' e 'mês', em que cada intervalo é um ponto de plot para um gráfico. Os dados de retorno devem ser ordenados com base em `startDate`, `endDate` e `frequency`. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**:
   * ` mappingUniqueId `
   * ` startDate `
   * ` endDate `
   * ` frequência `

 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Retorna o número de ocorrências da borda para um CDN individual. As regiões podem ser diferentes para cada fornecedor. Por
mapeamento.

 * **Parâmetros**:  
   * ` mappingUniqueId `
   * ` startDate `
   * ` endDate `
   * ` frequência `

 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Retorna o número total de estatísticas pré-determinadas para exibição direta (sem gráfico) para um determinado mapeamento, durante um período de tempo especificado. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**:
   * ` mappingUniqueId `
   * ` startDate `
   * ` endDate `
   * ` frequência `

 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`

----
## API para o Controle de acesso geográfico
{: #api-for-geographical-access-control}

### createGeoblocking
Cria uma nova regra de Controle de acesso geográfico e retorna a regra recém-criada.

  * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    É possível visualizar todos os atributos no Contêiner de entrada aqui:

    [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

    Os atributos a seguir fazem parte do Contêiner de entrada e são **necessários** ao criar uma nova regra de Controle de acesso geográfico:
    * `uniqueId`: uniqueId do mapeamento que designará a regra
    * `accessType`: especifica se a regra usará `ALLOW` ou `DENY` para permitir ou negar tráfego para a região especificada
    * `regionType`: o tipo de região ao qual aplicar a regra de Controle de acesso geográfico, seja `CONTINENT` ou `COUNTRY_OR_REGION`
    * `regions`: uma matriz que lista os locais aos quais o `accessType` se aplicará

      Consulte a página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) para ver uma lista de possíveis regiões.

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizar a classe de Bloqueio geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Atualiza uma regra de Controle de acesso geográfico existente para um mapeamento de domínio existente e retorna a regra atualizada.

  * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    É possível visualizar todos os atributos no Contêiner de entrada aqui:

    [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

    Os atributos a seguir fazem parte do Contêiner de entrada e podem ser fornecidos ao atualizar uma regra de Controle de acesso geográfico (os parâmetros são opcionais, a menos que indicado de outra forma):
    * `uniqueId`: **necessário** uniqueId do mapeamento ao qual a regra a ser atualizada pertence
    * `accessType`: especifica se a regra usará `ALLOW` ou `DENY` para permitir ou negar tráfego para a região especificada.
    * `regionType`: o tipo de região ao qual aplicar a regra, seja `CONTINENT` ou `COUNTRY_OR_REGION`
    * `regions`: uma matriz que lista os locais aos quais o `accessType` se aplicará

      Consulte a página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) para ver uma lista de possíveis regiões.

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizar a classe de Bloqueio geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Remove uma regra de Controle de acesso geográfico existente de um mapeamento de domínio existente.

  * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    É possível visualizar todos os atributos no Contêiner de entrada aqui:

    [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

    O atributo a seguir faz parte do Contêiner de entrada e é **necessário** ao excluir uma regra de Controle de acesso geográfico:
    * `uniqueId`: forneça o uniqueId do mapeamento ao qual a regra a ser excluída pertence.

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizar a classe de Bloqueio geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Recupera o comportamento de Controle de acesso geográfico de um mapeamento por meio do banco de dados.

  * **Parâmetros**:
    * `uniqueId`: o uniqueId do mapeamento ao qual a regra pertence.

  * **Retorna**: um objeto do tipo
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizar a classe de Bloqueio geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Retorna uma lista dos tipos e regiões que têm permissão para criar regras de Controle de acesso geográfico.

  * **Parâmetros**:
    * `uniqueId`: o uniqueId de seu mapeamento de domínio.

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`

    [Visualizar a classe de Bloqueio geográfico](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## API para proteção de hotlink
{: #api-for-hotlink-protection}

### createHotlinkProtection
Cria uma nova Proteção de hotlink e retorna o comportamento recém-criado.

  * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    É possível visualizar todos os atributos no Contêiner de entrada aqui:

    [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

    Os atributos a seguir fazem parte do Contêiner de entrada e são **necessários** ao criar uma nova Proteção de hotlink:
    * `uniqueId`: uniqueId do mapeamento para o qual designar o comportamento
    * `protectionType`: especifica se o acesso ao seu conteúdo será "ALLOW" (permitido) ou "DENY" (negado) quando uma página da web fizer uma solicitação de conteúdo com um valor do cabeçalho Referente que corresponde a um dos termos nos refererValues especificados
    * `refererValues`: uma lista separada por espaço único de termos de correspondência de URL referentes para os quais o comportamento `protectionType` entrará em vigor

      Consulte a página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) para ver uma lista de valores válidos de Proteção de hotlink.

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Visualizar a classe de Proteção do hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Atualiza um comportamento de Proteção do hotlink existente para um mapeamento de domínio existente e retorna o comportamento atualizado.

  * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    É possível visualizar todos os atributos no Contêiner de entrada aqui:

    [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

    Os atributos a seguir fazem parte do Contêiner de entrada e são **necessários** ao atualizar uma Proteção de hotlink existente:
    * `uniqueId`: uniqueId do mapeamento para o qual o comportamento existente pertence
    * `protectionType`: especifica se o acesso ao seu conteúdo será "ALLOW" (permitido) ou "DENY" (negado) quando uma página da web fizer uma solicitação de conteúdo com um valor do cabeçalho Referente que corresponde a um dos termos nos refererValues especificados 
    * `refererValues`: uma lista separada por espaço único de termos de correspondência de URL referentes para os quais o comportamento `protectionType` entrará em vigor

      Consulte a página [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) para ver uma lista de valores válidos de Proteção de hotlink.

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Visualizar a classe de Proteção do hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Remove um comportamento de Proteção do hotlink existente de um mapeamento de domínio existente.

  * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    É possível visualizar todos os atributos no Contêiner de entrada aqui:

    [ Visualizar o Contêiner de Entrada ](/docs/infrastructure/CDN?topic=CDN-input-container)

    Os atributos a seguir fazem parte do Contêiner de entrada e são **necessários** ao criar uma nova Proteção de hotlink:
    * `uniqueId`: uniqueId do mapeamento do qual remover o comportamento

  * **Retorna**: nulo

----
### getHotlinkProtection
Recupera o comportamento atual de Proteção do hotlink de um mapeamento.

  * **Parâmetros**:
    * `uniqueId`: o uniqueId do mapeamento ao qual o comportamento pertence

  * **Retorna**: um objeto do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Visualizar a classe de Proteção do hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
