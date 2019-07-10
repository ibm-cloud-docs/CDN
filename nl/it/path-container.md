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

# Contenitore percorso (origine)
{: #path-origin-container}

La raccolta `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` contiene gli attributi utilizzati dalle nostre API Path (Origin). Ciascuna delle API Path restituisce una raccolta di questo tipo.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: l'ID univoco dell'associazione a cui appartiene questo Origin Path.  
* `path`:  il percorso relativo al dominio che può essere utilizzato per raggiungere questa origine.  
* `originType`: il tipo dell'host di origine, attualmente ‘HOST\_SERVER’ oppure ‘OBJECT\_STORAGE’.  
* `httpPort`: il numero della porta utilizzata per il protocollo HTTP.  
* `httpsPort`: il numero della porta utilizzata per il protocollo HTTPS.  
* `status`: lo stato del Path (Origin (Percorso (origine)). Gli stati validi sono: _CREATING_, _STARTING_, _RUNNING_, _UPDATING_, _STOPPING_ e _DELETING_.
* `fileExtension`: le estensioni file di cui è consentita la memorizzazione in cache.  
* `header`: specifica le informazioni sull'intestazione host utilizzate dal server di origine.
* `cacheKeyQueryRule`: per configurare la modalità di funzionamento della chiave della cache sono disponibili le seguenti opzioni:
  * `include-all`: include tutti gli argomenti della query **Valore predefinito**
  * `ignore-all`: ignora tutti gli argomenti della query
  * `ignore: space separated query-args`: ignora questi specifici argomenti della query. Ad esempio, "ignore: query1 query2"
  * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, "include: query1 query2"
