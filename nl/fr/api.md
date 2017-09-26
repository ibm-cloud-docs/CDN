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

# Référence d'API

Visitez la page https://sldn.softlayer.com/article/PHP pour en savoir plus sur la configuration de PHP et SOAP. 
 
## API pour un compte
### verifyCdnAccountExists
Vérifie si un compte CDN existe pour l'utilisateur qui appelle l'API, `vendorName`

 * **Paramètres** : `vendorName`
 * **Retour** : `true` si le compte existe, sinon `false`
___

## API pour le mappage de domaine
### createDomainMapping
A l'aide des entrées fournies, cette fonction crée un mappage de domaine pour le fournisseur donné et l'associe à l'{{site.data.keyword.BluSoftlayer_notm}}ID de compte de l'utilisateur. Le compte CDN doit être créé à l'aide du paramètre `createCustomerSubAccount` pour que l'API fonctionne. Une fois le compte CDN créé, le paramètre `defaultTTL` est créé avec la valeur 3 600 secondes.

 * **Paramètres** :  Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. Cette collection doit comporter les éléments suivants :  `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType`, `path`
 * **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`. La collection inclut une valeur `uniqueId`, qui doit être envoyée sous forme d'entrée pour les appels API suivants liés au mappage.
___ 
### deleteDomainMapping
Supprime le mappage de domaine sur la base du paramètre `uniqueId`. Le mappage de domaine doit se trouver dans l'un des états suivants :  _RUNNING_, _STOPPED_, , _DELETED_, _ERROR_, _CNAME\_CONFIGURATION_ ou _SSL\_CONFIGURATION_.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### verifyDomainMapping
Vérifie le statut du CDN et met à jour les valeurs `status`, `cname` et/ou `vendorCname` si nécessaire. Renvoie les valeurs mises à jour le cas échéant. Le mappage de domaine doit se trouver dans l'un des états suivants : _RUNNING_, _CNAME\_CONFIGURATION_ ou _SSL\_CONFIGURATION_.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### startDomainMapping
Démarre un mappage de domaine CDN sur la base du paramètre `uniqueId`. Pour démarrer, le mappage de domaine doit se trouver à l'état _STOPPED_.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### stopDomainMapping
Met fin à un mappage de domaine CDN sur la base du paramètre `uniqueId`. Pour s'arrêter, le mappage de domaine doit se trouver à l'état _RUNNING_.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### updateDomainMapping
Permet à l'utilisateur de mettre à jour les propriétés du mappage identifiées par le paramètre `uniqueId`. Les zones pouvant être modifiées sont les suivantes : `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path`, et lorsque le type d'origine du chemin est Object Storage, les zones `bucketName` et `fileExtension` peuvent également être éditées. Pour être mis à jour, le mappage de domaine doit se trouver à l'état _RUNNING_. 

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### listDomainMappings
Renvoie une collection de tous les domaines d'un client particulier.

 * **Paramètres** : _none_ 
 * **Retour** : Collection de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### listDomainMappingByUniqueId
Renvoie une collection avec un objet de domaine unique sur la base du paramètre `uniqueId` associé au CDN.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection d'objet unique pour des objets de type `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___

## API pour la purge
### createPurge
Crée un enregistrement de purge et l'insère dans la base de données.

 * **Paramètres** : `string` `uniqueId`, `string` `path`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### getPurgeHistoryPerMapping
Renvoie l'historique de purge d'un CDN sur la base du paramètre `uniqueId` et du statut `saved`. La valeur par défaut de `saved` est _unsaved_.)

 * **Paramètres** : `string` `uniqueId`, `int` `saved`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### saveOrUnsavePurgePath
Met à jour le statut de l'entrée du chemin de purge pour indiquer si la purge doit être sauvegardée ou non. Crée une nouvelle purge `saved` si un chemin de purge est sauvegardé. Supprime un enregistrement de purge sauvegardé si le chemin est défini sur `unsaved`.

 * **Paramètres** : `string` `uniqueId`, `string` `path`, `int` `saved`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___

## API pour la durée de vie
### createTimeToLive
Crée un nouvel objet `TimeToLive` et l'insère dans la base de données.

 * **Paramètres** : `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Retour** : Objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Met à jour un objet `TimeToLive` existant. Si les entrées _oldTtl_ et _newTtl_ sont identiques, il s'arrête prématurément.

 * **Paramètres** : `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Retours** : Objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Supprime un objet `TimeToLive` existant de la base de données.

 * **Paramètres** : `string` `uniqueId`, `string` `pathName`
 * **Retour** : Chaîne avec le statut de suppression
___
### listTtl
Répertorie les objets `TimeToLive` sur la base du paramètre `uniqueId` du CDN.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Tableau d'objets de type `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___

## API pour l'origine
### createOriginPath
Crée un chemin d'origine pour un CDN existant et un client particulier. Le chemin d'origine peut porter sur un serveur hôte ou sur un stockage d'objets. Pour créer le chemin d'origine, le mappage de domaine doit se trouver à l'état _RUNNING_ ou _CNAME\_CONFIGURATION_.  

 * **Paramètres** : Un objet `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` pour lequel doivent être définies les propriétés suivantes : `domainName`, `vendorName`, `path`, `originType` et `origin`. Si le type d'origine est un serveur, les paramètres `httpPort` et/ou `httpsPort` doivent également être définis. Si le type d'origine est un stockage d'objets, le paramètre `bucketName` doit également être renseigné, ainsi que le paramètre `fileExtension` en option.  
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

___ 
### updateOrigin
Met à jour un chemin d'origine existant pour un mappage extistant et un client particulier. La valeur `originType` n'est pas modifiable. Seules les propriétés suivantes le sont : `path`, `origin`, `httpPort` et `httpsPort`. Pour être mis à jour, le mappage de domaine doit être à l'état _RUNNING_ ou _CNAME\_CONFIGURATION_.

 * **Paramètres** : Un objet `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` pour lequel doivent être définies les propriétés suivantes : `domainName`, `vendorName`, `path`, `originType` et `origin`. Si le type d'origine est un serveur, les paramètres `httpPort` et/ou `httpsPort` doivent également être définis. Si le type d'origine du chemin est un stockage d'objets, le paramètre `bucketName` doit être renseigné, ainsi que le paramètre `fileExtension` en option.  
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### deleteOriginPath
Supprime un chemin d'origine pour un CDN existant et un client particulier. Pour être supprimé, le mappage de domaine doit être à l'état _RUNNING_ ou _CNAME\_CONFIGURATION_.  

 * **Paramètres** : `string` `uniqueId`, `string` `path`
 * **Retour** : Message d'état si la suppression aboutit, sinon message d'exception

___
### listOriginPath
Répertorie le chemin d'origine d'un mappage existant et d'un client particulier.

 * **Paramètres** : `string` `uniqueId`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___

## API pour les métriques
### getCustomerUsageMetrics
Renvoie le nombre total de statistiques prédéterminées avec affichage direct (non graphique) d'un compte client, au cours d'une période donnée.

 * **Paramètres** : `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingUsageMetrics
Renvoie le nombre total de statistiques avec affichage direct du mappage concerné. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** : `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingHitsMetrics
Renvoie le nombre total de correspondances à une certaine fréquence au cours d'un laps de temps donné, par mappage de domaine. La fréquence peut être au format 'jour', 'semaine' et 'mois', où chaque intervalle correspond à un point de tracé sur le graphique. Les données renvoyées sont ordonnées en fonction des valeurs `startDate`, `endDate` et `frequency`. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** : `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Renvoie le nombre total de correspondances à une certaine fréquence au cours d'un laps de temps donné. La fréquence peut être au format 'heure', 'jour', 'semaine' et 'mois', où chaque intervalle correspond à un point de tracé sur le graphique. Les données renvoyées doivent être ordonnées en fonction des valeurs `startDate`, `endDate` et `frequency`. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** : `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Renvoie le nombre maximum de correspondances par région d'un CDN individuel. Les régions peuvent être différentes d'un fournisseur à l'autre. Par mappage.

 * **Paramètres** : `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Renvoie le nombre total de statistiques prédéterminées avec affichage direct (non graphique) d'un compte client, au cours d'une période donnée. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** : `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
