---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Conteneur d'entrée
{: #input-container}

Le conteneur d'entrée est une collection utilisée par les classes Mapping et (Origin) Path. Il offre à ces classes un ensemble cohérent d'attributs d'entrée.

* `vendorName` : Nom d'un fournisseur {{site.data.keyword.cloud}} CDN valide.
* `oldPath` : Utilisé par updateOriginPath(). Cette propriété stocke le nom du chemin en cours (old).

Les attributs suivants sont communs aux classes Mapping et (Origin) Path :
* `originType` : Type de l'hôte d'origine, actuellement 'HOST_SERVER' ou 'OBJECT_STORAGE'.
* `origin` : Adresse du serveur d'origine (nom d'hôte ou adresse IPv4 du serveur d'origine), point de terminaison à partir duquel le contenu est extrait ou nom du compartiment dans lequel le contenu est stocké. La longueur doit être inférieure à 511 caractères.
* `httpPort` :  Numéro du port utilisé pour le protocole HTTP. Akamai possède certaines limitations au niveau des numéros de ports pour les ports HTTP et HTTPS. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour obtenir une liste des numéros de ports autorisés.
* `httpsPort` :  Numéro du port utilisé pour le protocole HTTPS. Akamai possède certaines limitations au niveau des numéros de ports pour les ports HTTP et HTTPS. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour obtenir une liste des numéros de ports autorisés.
* `status` :  Statut du mappage ou du chemin. Le statut peut être CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED ou ERROR.
* `path` : Chemin d'accès à partir duquel le contenu mis en cache sera distribué. Le chemin par défaut est /\* . Lorsqu'il est utilisé par l'API `updateOriginPath`, cet attribut se réfère au nouveau chemin devant être ajouté.
* `performanceConfiguration` : Spécifications pour la configuration des performances du mappage.
* `cacheKeyQueryRule` : Les options suivantes sont disponibles pour configurer le comportement de la clé de cache :
  * `include-all`: Inclure tous les arguments de requête
  * `ignore-all` : Ignorer tous les arguments de requête
  * `ignore: arguments de requête séparés par des espaces` : Ignore ces arguments de requête spécifiques. Par exemple, `ignore: query1 query2`
  * `include: arguments de requête séparés par des espaces` : Inclut ces arguments de requête spécifiques. Par exemple, `include: query1 query2`
* `geoblockingRule`

Les attributs suivants sont spécifiques à la classe Mapping :

* `uniqueId` : Identificateur à 10 chiffres généré par le système et unique à chaque mappage. Il est généré lors de la création d'un mappage.
* `domain` : Nom du CDN principal. Egalement appelé "nom d'hôte".
* `protocol` : Protocole utilisé pour configurer les services. Il peut être HTTP, HTTPS ou une combinaison des deux, HTTP_AND_HTTPS.
* `cname` : Enregistrement du nom canonique qui sert d'alias au nom d'hôte. Il peut être fourni par l'utilisateur ou généré par le système. S'il est fourni par l'utilisateur, il doit comprendre moins de 64 caractères alphanumériques et doit être unique. S'il n'est pas fourni, il sera généré lors de la création du mappage.
* `certificateType` : Type de certificat utilisé par un mappage, `WILDCARD_CERT` ou `SHARED_SAN_CERT`. La valeur sera 0 pour les mappages HTTP.
* `respectHeaders` : Valeur booléenne qui, si définie sur 'true', entraîne le remplacement des paramètres TTL du CDN par les paramètres TTL du serveur d'origine.
* `header` : Spécifie les informations relatives aux en-têtes d'hôte utilisés par le serveur d'origine.

Les attributs suivants sont liés au stockage d'objet dans le Cloud :  
* `bucketName` : Nom unique de votre compartiment pour le stockage d'objets S3.  
* `fileExtension` : Extensions de fichiers autorisées.

L'attribut suivant est associé à la configuration de la protection des liens dynamiques :
* `hotlinkProtection` : Pour plus d'informations, voir la [classe de protection des liens dynamiques](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class).
