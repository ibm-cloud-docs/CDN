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

# Contenedor de vías de acceso (origen)
{: #path-origin-container}

La recopilación `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` contiene los atributos utilizados por nuestras API de vía de acceso (origen). Cada una de las API de vía de acceso devuelve una recopilación de este tipo.

**clase** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: ID exclusivo de la correlación a la cual pertenece esta vía de acceso de origen.  
* `path`: vía de acceso relativa al dominio que puede utilizarse para alcanzar este origen.  
* `originType`: tipo del host de origen, actualmente ‘HOST\_SERVER’ u ‘OBJECT\_STORAGE’.  
* `httpPort`: número del puerto utilizado para el protocolo HTTP.  
* `httpsPort`: número del puerto utilizado para el protocolo HTTPS.  
* `status`: estado de la vía de acceso (origen). Los estados válidos son: _CREATING_, _STARTING_, _RUNNING_, _UPDATING_, _STOPPING_ y _DELETING_.
* `fileExtension`: extensiones de archivo que se pueden almacenar en memoria caché.  
* `header`: especifica la información de cabecera de host utilizada por el servidor de origen.
* `cacheKeyQueryRule`: están disponibles las siguientes opciones para configurar el comportamiento de la clave de caché:
  * `include-all`: incluye todos los argumentos de consulta **predeterminados**
  * `ignore-all`: ignora todos los argumentos de consulta
  * `ignore: space separated query-args`: ignora argumentos de consulta específicos. Por ejemplo, "ignore: query1 query2"
  * `include: space separated query-args`: incluye argumentos de consulta específicos. Por ejemplo, "include: query1 query2"
