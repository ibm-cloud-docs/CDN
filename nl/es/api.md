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

# Referencia de API

Visite https://sldn.softlayer.com/article/PHP para obtener más información sobre PHP y SOAP en relación con la configuración. 
 
## API para una cuenta
### verifyCdnAccountExists
Comprueba si existe una cuenta CDN del usuario que llama a la API para el `vendorName` dado

 * **Parámetros**: `vendorName`
 * **Devuelve**: `true` si existe la cuenta o `false`
___

## API para la correlación de dominios
### createDomainMapping
Usando las entradas proporcionadas, esta función crea una correlación de dominios para el proveedor dado y la asocia con el ID de cuenta de {{site.data.keyword.BluSoftlayer_notm}} del usuario. La cuenta CDN debe crearse utilizando `createCustomerSubAccount` para que funcione esta API. Después de crear correctamente la CDN, se crea `defaultTTL` con un valor de 3600 segundos.

 * **Parámetros**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. La recopilación debería incluir los elementos siguientes: `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType`, `path`
 * **Devuelve**: una recopilación de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`. La recopilación proporciona un valor `uniqueId` que debe enviarse como entrada para las llamadas API posteriores relacionadas con la correlación. 
___ 
### deleteDomainMapping
Suprime la correlación de dominios basada en `uniqueId`. El estado de correlación de dominios debe ser uno de los siguientes: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME\_CONFIGURATION_ o _SSL\_CONFIGURATION_.

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### verifyDomainMapping
Verifica el estado de la CDN y actualiza los valores de `status`, `cname` y/o `vendorCname`, si es necesario. Devuelve los valores actualizados donde corresponda. La correlación de dominios debe encontrarse en uno de los estados siguientes: _RUNNING_, _CNAME\_CONFIGURATION_ o _SSL\_CONFIGURATION_.

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### startDomainMapping
Inicia una correlación de dominios de CDN basada en `uniqueId`. Para que se inicie, el estado de la correlación de dominios debe ser _STOPPED_. 

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### stopDomainMapping
Detiene una correlación de dominios de CDN basada en `uniqueId`. Para que se inicie la detención, el estado de la correlación de dominios debe ser _RUNNING_. 

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### updateDomainMapping
Habilita al usuario para que actualice las propiedades de la correlación identificadas con `uniqueId`. Pueden modificarse los campos siguientes: `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path` y, si el tipo de origen de la vía de acceso es Object Storage, también pueden modificarse `bucketName` y `fileExtension`. Para que se produzca una actualización, el estado de la correlación de dominios debe ser _RUNNING_. 

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### listDomainMappings
Devuelve una recopilación de todos los dominios para un cliente particular.

 * **Parámetros**: _ninguno_ 
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### listDomainMappingByUniqueId
Devuelve una recopilación con un objeto de dominio único basado en el `uniqueId` de una CDN.

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de objetos únicos de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___

## API para la depuración
### createPurge
Crea un registro de depuración y lo inserta en la base de datos. 

 * **Parámetros**: `string` `uniqueId`, `string` `path`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### getPurgeHistoryPerMapping
Devuelve el historial de depuraciones de una CDN basado en los estados `uniqueId` y `saved`. (De forma predeterminada, el valor de `saved` se establece en _unsaved_).

 * **Parámetros**: `string` `uniqueId`, `int` `saved`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### saveOrUnsavePurgePath
Actualiza el estado de la entrada de vía de acceso de depuración para indicar si debe guardarse esta vía de acceso. Crea una nueva depuración `saved` si se guarda la vía de acceso de depuración. Suprime un registro de depuración guardado si la vía de acceso es `unsaved`.

 * **Parámetros**: `string` `uniqueId`, `string` `path`, `int` `saved`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___

## API para el tiempo de duración
### createTimeToLive
Crea un nuevo objeto `TimeToLive` y lo inserta en la base de datos.

 * **Parámetros**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Devuelve**: un objeto de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Actualiza un objeto `TimeToLive` existente. Si las entradas de _oldTtl_ y _newTtl_ son iguales, sale pronto.

 * **Parámetros**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Devuelve**: un objeto de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Suprime un objeto `TimeToLive` existente de la base de datos.

 * **Parámetros**: `string` `uniqueId`, `string` `pathName`
 * **Devuelve**: una cadena con el estado de la supresión
___
### listTtl
Lista los objetos `TimeToLive` existentes según un valor de `uniqueId` de la CDN.

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una matriz de objetos de tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___

## API para el origen
### createOriginPath
Crea una vía de acceso de origen para una CDN existente y un cliente determinado. La vía de acceso de origen puede basarse en un servidor de host o en Object Storage. Para crear la vía de acceso de origen, el estado de la correlación de dominios debe ser _RUNNING_ o _CNAME\_CONFIGURATION_.   

 * **Parámetros**: un objeto `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` que espera la definición de las propiedades siguientes: `domainName`, `vendorName`, `path`, `originType` y `origin`. Si el tipo de origen es servidor, también deben establecerse `httpPort` y/o `httpsPort`. Si el tipo de origen es Object Storage, también debe proporcionarse `bucketName` junto con un valor de `fileExtension` opcional.  
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

___ 
### updateOrigin
Actualiza una vía de acceso de origen para una correlación existente y un cliente determinado. `originType` no se puede cambiar. Solo se pueden cambiar las propiedades siguientes: `path`, `origin`, `httpPort` y `httpsPort`. Para que se actualice, el estado de la correlación de dominios debe ser _RUNNING_ o _CNAME\_CONFIGURATION_. 

 * **Parámetros**: un objeto `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` que espera la definición de las propiedades siguientes: `domainName`, `vendorName`, `path`, `originType` y `origin`. Si el tipo de origen es un servidor, también deben establecerse `httpPort` y/o `httpsPort`. Si el tipo de origen de la vía de acceso es Object Storage, deben proporcionarse `bucketName` y, opcionalmente, `fileExtension`.   
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### deleteOriginPath
Suprime una vía de acceso de origen para una CDN existente y un cliente determinado. Para que se suprima, el estado de la correlación de dominios debe ser _RUNNING_ o _CNAME\_CONFIGURATION_.   

 * **Parámetros**: `string` `uniqueId`, `string` `path`
 * **Devuelve**: un mensaje de estado si la supresión se ha realizado correctamente; en caso contrario, se devuelve una excepción

___
### listOriginPath
Lista la vía de acceso de origen para una correlación existente y un cliente determinado.

 * **Parámetros**: `string` `uniqueId`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___

## API para métricas
### getCustomerUsageMetrics
Devuelve el número total de estadísticas predeterminadas para su visualización directa (sin gráficos), correspondientes a la cuenta de un cliente durante un periodo de tiempo determinado.

 * **Parámetros**: `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingUsageMetrics
Devuelve el número total de estadísticas predeterminadas para su visualización directa, correspondientes a la correlación determinada. El valor de `frequency` es "aggregate" de forma predeterminada. 

 * **Parámetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingHitsMetrics
Devuelve el número total de aciertos en una frecuencia determinada durante un intervalo de tiempo dado, por correlación de dominios. La frecuencia puede ser diaria, semanal o mensual, en la que cada intervalo es un punto de gráfico. Los datos devueltos se ordenan según los valores de `startDate`, `endDate` y `frequency`. El valor de `frequency` es "aggregate" de forma predeterminada. 

 * **Parámetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Devuelve el número total de aciertos en una frecuencia determinada durante un intervalo de tiempo dado. La frecuencia puede ser por horas, diaria, semanal o mensual, en la que cada intervalo es un punto de gráfico. Los datos devueltos deben ordenarse según los valores de `startDate`, `endDate` y `frequency`. El valor de `frequency` es "aggregate" de forma predeterminada. 

 * **Parámetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Devuelve el número de aciertos en el perímetro por cada región en una CDN individual. Las regiones pueden diferir según el proveedor. Por correlación. 

 * **Parámetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Devuelve el número total de estadísticas predeterminadas para su visualización directa (sin gráficos), correspondientes a la cuenta de un cliente durante un periodo de tiempo determinado.El valor de `frequency` es "aggregate" de forma predeterminada. 

 * **Parámetros**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Devuelve**: una recopilación de objetos de tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
