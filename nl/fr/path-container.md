---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: origin, path, container, collection, attributes, query, arguments, class, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Conteneur Path (Origin)
{: #path-origin-container}

La collection `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` contient les attributs utilisés par nos API Path (Origin). Chacun des API Path renvoie une collection de ce type.

**Classe** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` :  

* `mappingUniqueId` : ID unique du mappage auquel appartient ce chemin d'origine.  
* `path` :  Chemin relatif au domaine qui peut être utilisé pour atteindre cette origine.  
* `originType` : Type de l'hôte d'origine, actuellement ‘HOST\_SERVER’ ou ‘OBJECT\_STORAGE’.  
* `httpPort` :  Numéro du port utilisé pour le protocole HTTP.  
* `httpsPort` :  Numéro du port utilisé pour le protocole HTTPS.  
* `status` : Statut de Path (Origin). Les statuts valides sont : _CREATING_, _STARTING_, _RUNNING_, _UPDATING_, _STOPPING_ et _DELETING_.
* `fileExtension` : Extensions de fichier pouvant être mises en cache.  
* `header` : Spécifie les informations relatives aux en-têtes d'hôte utilisés par le serveur d'origine
* `cacheKeyQueryRule` : Les options suivantes sont disponibles pour configurer le comportement de la clé de cache :
  * `include-all` : Inclure tous les arguments de requête. **Valeur par défaut**
  * `ignore-all`: Ignorer tous les arguments de requête
  * `ignore: arguments de requête séparés par des espaces` : Ignore ces arguments de requête spécifiques. Par exemple, "ignore: query1 query2"
  * `include: arguments de requête séparés par des espaces` : Inclut ces arguments de requête spécifiques. Par exemple, "include: query1 query2"
