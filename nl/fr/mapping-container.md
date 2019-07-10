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

# Conteneur Mapping
{: #mapping-container}

La collection `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` contient les attributs utilisés par nos API de mappage. Chaque API de mappage renvoie une collection de ce type.

**Classe** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` :

* `vendorName` : Nom d'un fournisseur {{site.data.keyword.cloud}} CDN valide.
* `uniqueId` : Identificateur à 10 chiffres généré par le système et unique à chaque mappage. Il est généré lors de la création d'un mappage.
* `domain` : Nom du CDN principal. Il est également appelé `hostname`.
* `protocol` : Protocole utilisé pour configurer les services. Il peut être HTTP, HTTPS ou une combinaison des deux, HTTP_AND_HTTPS.
* `originType` : Type de l'hôte d'origine, actuellement 'HOST_SERVER' ou 'OBJECT_STORAGE'.
* `originHost` : Adresse du serveur d'origine (le nom d'hôte ou l'adresse IPv4 du serveur d'origine), qui correspond au noeud final où le contenu doit être extrait ou au nom du compartiment où le contenu est stocké. Elle doit contenir moins de 511 caractères.
* `httpPort` :  Numéro du port utilisé pour le protocole HTTP. Akamai possède certaines limitations au niveau des numéros de ports pour les ports HTTP et HTTPS. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports autorisés.
* `httpsPort` :  Numéro du port utilisé pour le protocole HTTPS. Akamai possède certaines limitations au niveau des numéros de ports pour les ports HTTP et HTTPS. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports autorisés.
* `certificateType` : Type de certificat utilisé par un mappage, `WILDCARD_CERT` ou `SHARED_SAN_CERT`. La valeur sera 0 pour les mappages HTTP.
* `cname` : Enregistrement du nom canonique qui sert d'alias au nom d'hôte. Il peut être fourni par l'utilisateur ou généré par le système. S'il est fourni par l'utilisateur, il doit comprendre moins de 64 caractères alphanumériques et doit être unique. S'il n'est pas fourni, il sera généré lors de la création du mappage.
* `respectHeaders` : Valeur booléenne qui, si définie sur 'true', entraîne le remplacement des paramètres TTL du CDN par les paramètres TTL du serveur d'origine.
* `header` : Spécifie les informations relatives aux en-têtes d'hôte utilisés par le serveur d'origine
* `status` : Statut du mappage. Le statut peut être CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED ou ERROR.
* `bucketName` : Nom de compartiment de votre stockage d'objet S3.
* `fileExtension` : Extensions de fichier pouvant être mises en cache.
* `path` : Chemin d'accès à votre stockage d'objet S3. Il se trouve généralement sous l'URL du conteneur d'objets ou dans la section API de votre service.
* `cacheKeyQueryRule` : Les options suivantes sont disponibles pour configurer le comportement de la clé de cache :
  * `include-all` : Inclure tous les arguments de requête. **Valeur par défaut**
  * `ignore-all` : Ignorer tous les arguments de requête
  * `ignore: arguments de requête séparés par des espaces` : Ignore ces arguments de requête spécifiques. Par exemple, "ignore: query1 query2"
  * `include: arguments de requête séparés par des espaces` : Inclut ces arguments de requête spécifiques. Par exemple, "include: query1 query2"

Il est important de faire une remarque particulière pour `uniqueId` qui est généré lors de la création d'un mappage et qui est utilisé en tant que paramètre pour les appels d'API suivants.

Par exemple, cet appel vers `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

renverra un objet similaire à celui-ci :

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
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
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

Les appels vers `stopDomainMapping` et `startDomainMapping` retournent le même objet Mapping, seule la valeur de la ligne `status` faisant la différence.

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
