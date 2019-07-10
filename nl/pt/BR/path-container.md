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

# Contêiner de caminho (origem)
{: #path-origin-container}

A coleção `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` contém os atributos
utilizados pelas nossas APIs de caminho (origem). Cada uma das APIs de caminho retorna uma coleção desse tipo.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: o ID exclusivo do mapeamento ao qual esse caminho de origem pertence.  
* `path`: caminho relativo ao domínio que pode ser usado para alcançar essa origem.  
* `originType`: tipo do host de origem, atualmente 'HOST\_SERVER' ou 'OBJECT\_STORAGE'.  
* `httpPort`: número da porta usada para o protocolo HTTP.  
* `httpsPort`: número da porta usada para o protocolo HTTPS.  
* `status`: o status do caminho (origem). Os status válidos são: _CREATING_,
_STARTING_, _RUNNING_, _UPDATING_, _STOPPING_ e _DELETING_.
* `fileExtension`: extensões do arquivo que podem ser armazenadas em cache.  
* `header`: especifica as informações do cabeçalho do Host usadas pelo Servidor de origem.
* `cacheKeyQueryRule`: as opções a seguir estão disponíveis para configurar o comportamento da Chave de cache:
  * `include-all`: inclui todos os argumentos de consulta **Padrão**
  * `ignore-all`: ignora todos os argumentos de consulta
  * `ignore: space separated query-args`: ignora esses argumentos de consulta específicos. Por exemplo, "ignore: query1 query2"
  * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, "include: query1 query2"
