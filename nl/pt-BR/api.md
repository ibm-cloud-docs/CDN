---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Referência de API

Visite https://sldn.softlayer.com/article/PHP para obter mais informações sobre PHP e SOAP relacionados à configuração. 
 
## API para uma conta
### verifyCdnAccountExists
Verifica se existe uma conta CDN para o usuário que está chamando a API, para o `vendorName` especificado

 * **Parâmetros**: `vendorName`
 * **Retorno**: `true` se a conta existir, caso contrário, `false`
___

## API para mapeamento de domínio
### createDomainMapping
Usando as entradas fornecidas, essa função cria um mapeamento de domínio para o fornecedor especificado e o associa ao ID da conta do {{site.data.keyword.BluSoftlayer_notm}} do usuário. A conta CDN deve ser criada usando `createCustomerSubAccount` para que essa API funcione. Depois de criar o CDN com sucesso, um `defaultTTL` será criado com um valor de 3.600 segundos.

 * **Parâmetros**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. A coleção deverá incluir os itens a seguir: `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType`, `path`
 * **Retorno**: uma coleção do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`. A coleção fornece um valor `uniqueId`, que precisa ser enviado como entrada para as chamadas API subsequentes relacionadas ao mapeamento.
___ 
### deleteDomainMapping
Exclui o mapeamento de domínio com base no `uniqueId`. O mapeamento de domínio deverá estar em um dos estados a seguir: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME\_CONFIGURATION_ ou _SSL\_CONFIGURATION_.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### verifyDomainMapping
Verifica o status do CDN e atualizará o `status`, o `cname` e/ou o `vendorCname`, se necessário. Retorna os valores atualizados, quando aplicável. O mapeamento de domínio deverá estar em um dos estados a seguir: _RUNNING_, _CNAME\_CONFIGURATION_ ou _SSL\_CONFIGURATION_.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### startDomainMapping
Inicia um mapeamento de domínio do CDN com base no `uniqueId`. Para ser iniciado, o mapeamento de domínio deverá estar em um estado _STOPPED_.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### stopDomainMapping
Para um mapeamento de domínio do CDN com base no `uniqueId`. Para iniciar a parada, o mapeamento de domínio deverá estar em um estado _RUNNING_.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### updateDomainMapping
Permite que o usuário atualize propriedades do mapeamento identificado pelo `uniqueId`. Os campos a seguir poderão ser mudados: `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path` e se o tipo de origem de seu caminho for Object Storage, `bucketName` e `fileExtension` também poderão se mudados. Para que uma atualização ocorra, o mapeamento de domínio deverá estar em um estado _RUNNING_.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### listDomainMappings
Retorna uma coleção de todos os domínios para um cliente específico.

 * **Parâmetros**: _none_ 
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### listDomainMappingByUniqueId
Retorna uma coleção com um único objeto de domínio com base no `uniqueId` de um CDN.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção de único objeto de objetos do tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___

## API para limpeza
### createPurge
Cria um registro de limpeza e insere-o no banco de dados.

 * **Parâmetros**: `string` `uniqueId`, `string` `path`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### getPurgeHistoryPerMapping
Retorna o histórico de limpeza de um CDN com base nos status `uniqueId` e `saved`. (Por padrão, o valor de `saved` é configurado como _unsaved_.)

 * **Parâmetros**: `string` `uniqueId`, `int` `saved`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### saveOrUnsavePurgePath
Atualiza o status da entrada do caminho de limpeza para indicar se esse caminho de limpeza deverá ser salvo. Criará uma nova limpeza `saved` se um caminho de limpeza for salvo. Excluirá um registro de limpeza salvo se o caminho for `unsaved`.

 * **Parâmetros**: `string` `uniqueId`, `string` `path`, `int` `saved`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___

## API para tempo de vida
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
___

## API para origem
### createOriginPath
Cria um Caminho de origem para um CDN existente e para um cliente específico. O Caminho de origem pode basear-se em um servidor host ou em um Object Storage. Para criar o Caminho de origem, o mapeamento de domínio deverá estar em um estado _RUNNING_ ou _CNAME\_CONFIGURATION_.  

 * **Parâmetros**: um objeto `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` que espera que as propriedades a seguir sejam configuradas: `domainName`, `vendorName`, `path`, `originType` e `origin`. Se o tipo de origem for servidor, `httpPort` e/ou `httpsPort` também deverão ser configurados. Se o tipo de origem for Object Storage, o `bucketName` também deverá ser fornecido, junto a um `fileExtension` opcional.  
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

___ 
### updateOrigin
Atualiza um Caminho de origem existente para um mapeamento existente e para um cliente específico. O `originType` não pode ser mudado. Apenas as propriedades a seguir podem ser mudadas: `path`, `origin`, `httpPort` e `httpsPort`. Para ser atualizado, o mapeamento de domínio deverá estar em um estado _RUNNING_ ou _CNAME\_CONFIGURATION_.

 * **Parâmetros**: um objeto `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` que espera que as propriedades a seguir sejam configuradas: `domainName`, `vendorName`, `path`, `originType` e `origin`. Se o tipo de origem for um servidor, `httpPort` e/ou `httpsPort` também deverão ser configurados. Se o tipo de origem do caminho for Object Storage, `bucketName` deverá ser fornecido e, opcionalmente, `fileExtension` poderá ser fornecido.  
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### deleteOriginPath
Exclui um Caminho de origem existente para um CDN existente e para um cliente específico. Para ser excluído, o mapeamento de domínio deverá estar em um estado _RUNNING_ ou _CNAME\_CONFIGURATION_.  

 * **Parâmetros**: `string` `uniqueId`, `string` `path`
 * **Retorno**: uma mensagem de status se a exclusão tiver sido bem-sucedida; caso contrário, uma exceção

___
### listOriginPath
Lista o caminho de origem para um mapeamento existente e para um cliente específico.

 * **Parâmetros**: `string` `uniqueId`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___

## API para métricas
### getCustomerUsageMetrics
Retorna o número total de estatísticas predeterminadas para exibição direta (sem gráfico) para a conta de um cliente, durante um período de tempo especificado.

 * **Parâmetros**: `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingUsageMetrics
Retorna o número total de estatísticas predeterminadas para exibição direta para o mapeamento especificado. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingHitsMetrics
Retorna o número total de ocorrências em uma certa frequência durante um intervalo de tempo especificado, por mapeamento de domínio. A frequência pode ser 'dia', 'semana' e 'mês', em que cada intervalo é um ponto de plot para um gráfico. Os dados de retorno são ordenados com base em `startDate`, `endDate` e `frequency`. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Retorna o número total de ocorrências em uma certa frequência durante um intervalo de tempo especificado. A frequência pode ser 'hora', 'dia', 'semana' e 'mês', em que cada intervalo é um ponto de plot para um gráfico. Os dados de retorno devem ser ordenados com base em `startDate`, `endDate` e `frequency`. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Retorna o número de ocorrências de borda por região para uma CDN individual. As regiões podem ser diferentes para cada fornecedor. Por mapeamento.

 * **Parâmetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Retorna o número total de estatísticas predeterminadas para exibição direta (sem gráfico) para a conta de um cliente, durante um período de tempo especificado. O valor de `frequency` é 'aggregate', por padrão.

 * **Parâmetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retorno**: uma coleção de objetos do tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
