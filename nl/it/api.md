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

# Riferimento API

Visita il sito https://sldn.softlayer.com/article/PHP per ulteriori informazioni su PHP e SOAP relativi alla configurazione. 
 
## API per un Account
### verifyCdnAccountExists
Verifica che esista un account CDN per l'utente che richiama l'API, relativo al `vendorName` fornito

 * **Parametri**: `vendorName`
 * **Restituzione**: `true` se l'account esiste, altrimenti `false`
___

## API per Associazione dominio
### createDomainMapping
Utilizzando gli input forniti, questa funzione crea un'associazione dominio per il fornitore indicato e la unisce all'ID account {{site.data.keyword.BluSoftlayer_notm}} dell'utente. Affinché questa API funzioni, l'account CDN deve essere creato utilizzando `createCustomerSubAccount`. Una volta creato il CDN, viene creato un `defaultTTL` con un valore di 3600 secondi.

 * **Parametri**:  una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. La raccolta deve includere i seguenti elementi:  `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType`, `path`
 * **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`. La raccolta fornisce un valore `uniqueId`, che deve essere inviato come input per le successive chiamate API correlate all'associazione.
___ 
### deleteDomainMapping
Elimina l'associazione dominio in base all'`uniqueId`. L'associazione dominio deve essere in uno dei seguenti stati:  _IN ESECUZIONE_, _ARRESTATO_, _ELIMINATO_, _ERRORE_, _CONFIGURAZIONE_CNAME\_ o _CONFIGURAZIONE_SSL\_.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### verifyDomainMapping
Verifica lo stato del CDN e, se necessario, aggiorna `status`, `cname` e/o `vendorCname`. Restituisce i valori aggiornati laddove applicabile. L'associazione dominio deve essere in uno dei seguenti stati: _IN ESECUZIONE_, _CONFIGURAZIONE_CNAME\_ o _CONFIGURAZIONE_SSL\_.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### startDomainMapping
Avvia un'associazione dominio CDN in base all'`uniqueId`. Per essere avviata, l'associazione dominio deve essere in uno stato _ARRESTATO_.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### stopDomainMapping
Arresta un'associazione dominio CDN in base all'`uniqueId`. Per iniziare l'arresto, l'associazione dominio deve essere in uno stato _IN ESECUZIONE_.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### updateDomainMapping
Abilita l'utente ad aggiornare le proprietà dell'associazione identificata dall'`uniqueId`. È possibile modificare i seguenti campi: `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path` e, se il tipo di origine del percorso è Archiviazione oggetti, è possibile modificare anche `bucketName` e `fileExtension`. Affinché un aggiornamento venga eseguito, l'associazione dominio deve essere in uno stato _IN ESECUZIONE_.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### listDomainMappings
Restituisce una raccolta di tutti i domini per un particolare cliente.

 * **Parametri**: _none_ 
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### listDomainMappingByUniqueId
Restituisce una raccolta con un singolo oggetto dominio in base all'`uniqueId` di un CDN.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta a oggetto singolo di oggetti di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___

## API per Eliminazione
### createPurge
Crea un record di eliminazione e lo inserisce nel database.

 * **Parametri**: `string` `uniqueId`, `string` `path`
 * **Restituzione**:  una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### getPurgeHistoryPerMapping
Restituisce la cronologia di eliminazioni per un CDN in base all'`uniqueId` e allo stato `saved`. (Per impostazione predefinita, il valore `saved` è impostato su _unsaved_.)

 * **Parametri**: `string` `uniqueId`, `int` `saved`
 * **Restituzione**:  una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### saveOrUnsavePurgePath
Aggiorna lo stato della voce di percorso di eliminazione per indicare se questo percorso di eliminazione deve essere salvato. Se un percorso di eliminazione viene salvato, crea una nuova eliminazione `saved`. Se il percorso è `unsaved`, elimina un record di eliminazione salvata.

 * **Parametri**: `string` `uniqueId`, `string` `path`, `int` `saved`
 * **Restituzione**:  una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___

## API per Time to Live
### createTimeToLive
Crea un nuovo oggetto `TimeToLive` e lo inserisce nel database.

 * **Parametri**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Restituzione**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Aggiorna un oggetto `TimeToLive` esistente. Se gli input  _oldTtl_ e _newTtl_ sono uguali, termina prima.

 * **Parametri**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Restituzione**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Elimina un oggetto `TimeToLive` esistente dal database.

 * **Parametri**: `string` `uniqueId`, `string` `pathName`
 * **Restituzione**: una stringa con lo stato dell'eliminazione
___
### listTtl
Elenca gli oggetti `TimeToLive` esistenti in base all'`uniqueId` di un CDN.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: un array di oggetti di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___

## API per Origine
### createOriginPath
Crea un percorso di origine per un CDN esistente e per un particolare cliente. Il percorso di origine può essere basato su un server host o su Archiviazione oggetti. Per creare il percorso di origine, l'associazione dominio deve essere in uno stato _IN ESECUZIONE_ o _CONFIGURAZIONE_CNAME_.  

 * **Parametri**: un oggetto `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` che prevede l'impostazione delle seguenti proprietà: `domainName`, `vendorName`, `path`, `originType` e `origin`. Se il tipo di origine è un server, è necessario impostare anche `httpPort` e/o `httpsPort`. Se il tipo di origine è Archiviazione oggetti, è necessario fornire anche `bucketName` e, facoltativamente, `fileExtension`.  
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

___ 
### updateOrigin
Aggiorna un percorso di origine per un'associazione esistente e per un particolare cliente. La proprietà `originType` non può essere modificata. È possibile modificare solo le seguenti proprietà: `path`, `origin`, `httpPort` e `httpsPort`. Per essere aggiornato, l'associazione dominio deve essere in uno stato _IN ESECUZIONE_ o _CONFIGURAZIONE_CNAME\_.

 * **Parametri**: un oggetto `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` che prevede l'impostazione delle seguenti proprietà: `domainName`, `vendorName`, `path`, `originType` e `origin`. Se il tipo di origine è un server, è necessario impostare anche `httpPort` e/o `httpsPort`. Se il tipo di origine del percorso è Archiviazione oggetti, è necessario fornire `bucketName` e, facoltativamente, `fileExtension`.   
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### deleteOriginPath
Elimina un percorso di origine per un CDN esistente e per un particolare cliente. Per essere eliminato, l'associazione dominio deve essere in uno stato _IN ESECUZIONE_ o _CONFIGURAZIONE_CNAME\_.  

 * **Parametri**: `string` `uniqueId`, `string` `path`
 * **Restituzione**: un messaggio di stato se l'eliminazione è stata eseguita correttamente; altrimenti, un'eccezione

___
### listOriginPath
Elenca il percorso di origine per un'associazione esistente e per un particolare cliente.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___

## API per Metriche
### getCustomerUsageMetrics
Restituisce il numero totale di statistiche predeterminate per la visualizzazione diretta (senza grafici) per l'account di un cliente, per un determinato periodo di tempo.

 * **Parametri**: `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingUsageMetrics
Restituisce il numero totale di statistiche predeterminate per la visualizzazione diretta per l'associazione fornita. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingHitsMetrics
Restituisce il numero totale di riscontri a una certa frequenza per un determinato intervallo di tempo, per associazione dominio. La frequenza può essere 'day', 'week' e 'month', dove ogni intervallo è un punto per il grafico. I dati di restituzione sono ordinati in base a `startDate`, `endDate` e `frequency`. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Restituisce il numero totale di riscontri a una certa frequenza per un determinato intervallo di tempo. La frequenza può essere 'hour', 'day', 'week' e 'month', dove ogni intervallo è un punto per il grafico. I dati di restituzione devono essere ordinati in base a `startDate`, `endDate` e `frequency`. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Restituisce il numero di riscontri perimetrali per regione per un singolo CDN. Le regioni possono differire per ogni fornitore. Per associazione.

 * **Parametri**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Restituisce il numero totale di statistiche predeterminate per la visualizzazione diretta (senza grafici) per l'account di un cliente, per un determinato periodo di tempo. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
