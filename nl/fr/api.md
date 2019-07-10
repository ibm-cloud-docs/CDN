---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}


# Références de l'API CDN
{: #cdn-api-reference}

L'interface de programme d'application (API) d'infrastructure {{site.data.keyword.cloud}} (généralement appelée SLAPI), fournie par {{site.data.keyword.cloud}}, est l'interface de développement qui permet aux développeurs et administrateurs système d'interagir directement avec le système back end de l'infrastructure {{site.data.keyword.cloud_notm}}.

SLAPI implémente un grand nombre de fonctionnalités du portail client : si une interaction est possible dans le portail client, elle peut également être exécutée dans SLAPI. Puisque vous pouvez interagir à l'aide d'un programme avec toutes les portions de l'environnement d'infrastructure {{site.data.keyword.cloud_notm}} au sein de SLAPI, vous pouvez utiliser l'API pour automatiser les tâches.

SLAPI est un système RPC (Remote Procedure Call, appel de procédure distante). Chaque appel implique l'envoi de données vers un noeud final d'API et la réception de données structurées en retour. Le format utilisé pour l'envoi et la réception de données à l'aide de SLAPI dépend de l'implémentation que vous avez choisie. SLAPI utilise actuellement SOAP, XML-RPC ou REST pour la transmission de données.

Pour en savoir plus sur SLAPI ou sur les API du service {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN), veuillez consulter les ressources suivantes dans le réseau de développement d'{{site.data.keyword.cloud_notm}} :

* [SLAPI Overview](https://softlayer.github.io/ )
* [Getting Started with SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API Guide](https://softlayer.github.io/article/php/ )

----

Pour commencer, vous trouverez ci-dessous la séquence d'appels API recommandée :
* `listVendors` - Fournit une liste des fournisseurs pris en charge.
* `verifyOrder` - Vérifie si la commande peut-être placée.
* `placeOrder`  - Crée le compte CDN avec un fournisseur donné. Vous pouvez créer jusqu'à 10 mappages de CDN après un appel placeOrder réussi.
* `createDomainMapping` - Crée les mappages du CDN
* `verifyDomainMapping` - Change le statut du CDN en _RUNNING_ (en cours d'exécution)

Vous pouvez utiliser les autres API après avoir suivi la séquence précédente.

[Un exemple de code est disponible pour chaque étape de cette séquence d'appels.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

Vous **devez** utiliser le nom d'utilisateur d'API et la clé d'API d'un utilisateur possédant le droit `CDN_ACCOUNT_MANAGE` pour la plupart des appels d'API affichés dans ce document. Contactez l'utilisateur principal de votre compte si vous avez besoin que ce droit soit activé pour vous. (Chaque compte client IBM Cloud possède un utilisateur principal)
{: note}

----
## API pour les fournisseurs
{: #api-for-vendor}

### listVendors
Cette API permet aux utilisateurs de dresser la liste des fournisseurs CDN pris en charge. Le `vendorName` est nécessaire pour créer un compte CDN et commencer à commander votre CDN.

* **Paramètres requis** : Aucun
* **Retour** : collection de type `SoftLayer_Container_Network_CdnMarketplace_Vendor`

  Vous trouverez le conteneur des fournisseurs et un exemple d'utilisation ici : [Conteneur des fournisseurs](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## API de compte
{: #api-for-account}

### verifyCdnAccountExists
Vérifie si un compte CDN existe pour l'utilisateur appelant l'API, pour le nom de fournisseur donné (`vendorName`).

* **Paramètres requis** : `vendorName` : fournissez le nom d'un fournisseur de CDN valide.
* **Retour** : `true` si un compte existe, sinon `false`.

----
## API pour le mappage de domaine
{: #api-for-domain-mapping}

### createDomainMapping
A l'aide des entrées fournies, cette fonction crée un mappage de domaine pour le fournisseur donné et l'associe à l'{{site.data.keyword.cloud_notm}}ID de compte d'infrastructure de l'utilisateur. Le compte CDN doit d'abord être créé avec `placeOrder` pour que cette API fonctionne (voir un exemple de l'appel d'API `placeOrder` dans les [Exemples de codes](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)). Une fois le compte CDN créé, le paramètre `defaultTTL` est créé avec la valeur 3 600 secondes.

* **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

  [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

  Les attributs suivants appartiennent au conteneur d'entrée et peuvent être fournis lors de la création d'un mappage de domaine (les attributs sont facultatifs sauf indication contraire) :
    * `vendorName` : **obligatoire** Fournissez le nom d'un fournisseur IBM Cloud CDN valide.
    * `origin` : **obligatoire** Fournissez l'adresse du serveur d'origine sous forme de chaîne.
    * `originType` : **obligatoire** Le type du serveur d'origine peut être `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `domain` : **obligatoire** Fournissez votre nom d'hôte sous forme de chaîne.
    * `protocol` : **obligatoire** Les protocoles pris en charge sont `HTTP`, `HTTPS` ou `HTTP_AND_HTTPS`.
    * `certificateType` : **obligatoire** pour le protocole HTTPS. `SHARED_SAN_CERT` ou `WILDCARD_CERT`
    * `path` : Chemin d'accès à partir duquel le contenu mis en cache sera distribué. Le chemin d'accès par défaut est `/*`
    * `httpPort` et/ou `httpsPort` : (**obligatoire** pour le serveur hôte) ces deux options doivent correspondre au protocole souhaité. Si le protocole est `HTTP`, alors `httpPort` doit être défini et `httpsPort` ne doit _pas_ être défini. De même, si le protocole est `HTTPS`, alors `httpsPort` doit être défini et `httpPort` ne doit _pas_ être défini. Si le protocole est `HTTP_AND_HTTPS`, alors  `httpPort` et `httpsPort` _doivent_ être définis _à la fois_. Akamai possède certaines limitations au niveau des numéros de port. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports autorisés.
    * `header` : Spécifie les informations relatives aux en-têtes d'hôte utilisés par le serveur d'origine
    * `respectHeader` : Valeur booléenne qui, si elle est définie sur `true`, entraînera le remplacement des paramètres TTL du CDN par les paramètres TTL du serveur d'origine.
    * `cname` : Fournissez un alias pour le nom d'hôte. Il sera généré si aucune valeur n'est fournie.
    * `bucketName` : (**obligatoire** pour le stockage d'objet uniquement) Nom de compartiment de votre stockage d'objet S3.
    * `fileExtension` : (facultatif pour le stockage d'objet) Extensions de fichiers autorisées à être mises en cache.
    * `cacheKeyQueryRule` : Les options suivantes sont disponibles pour configurer le comportement de la clé de cache. Si aucun argument `cacheKeyQueryRule` n'est fourni, la valeur par défaut "include-all" sera utilisée
      * `include-all` - inclut tous les arguments de requête **default**
      * `ignore-all` - ignore tous les arguments de requête
      * `ignore: arguments de requête séparés par des espaces` - ignore ces arguments de requête spécifiques. Par exemple, `ignore: query1 query2`
      * `include: arguments de requête séparés par des espaces` : inclut ces arguments de requête spécifiques. Par exemple, `include: query1 query2`

* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  **REMARQUE** : La collection fournit une valeur `uniqueId` qui doit être envoyée sous forme d'entrée pour les appels API suivants en relation avec le mappage ou le chemin d'origine.

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Supprime le mappage de domaine sur la base du paramètre `uniqueId`. Le mappage de domaine doit se trouver dans l'un des états suivants : _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME_CONFIGURATION_ ou _SSL_CONFIGURATION_.

* **Paramètres obligatoires** : `uniqueId` : ID unique du mappage à supprimer
* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Vérifie l'état du CDN et met à jour la valeur `status` du mappage CDN si elle a changé. Lorsqu'un mappage de CDN est créé initialement, son statut est _CNAME_CONFIGURATION_. A ce stade, vous devez mettre à jour l'enregistrement DNS de sorte que le nom d'hôte du mappage CDN pointe vers le CNAME. Contactez votre fournisseur DNS si vous avez des questions relatives à la procédure de mise à jour et à la durée nécessaire pour propager les modifications sur Internet. La durée moyenne est comprise entre 15 et 30 minutes. Au-delà de ce délai, l'API `verifyDomainMapping` doit être appelée afin de vérifier si la chaîne CNAME est terminée. Si tel est le cas, le statut du mappage CDN devient _RUNNING_.

Vous pouvez appeler cette API à tout moment afin d'obtenir le statut du mappage CDN le plus récent. Le mappage de domaine doit se trouver dans l'un des états suivants : _RUNNING_ ou _CNAME_CONFIGURATION_.

* **Paramètres obligatoires** : `uniqueId` : ID unique du mappage que vous souhaitez vérifier
* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Démarre un mappage de domaine CDN sur la base du paramètre `uniqueId`. Pour démarrer, le mappage de domaine doit se trouver à l'état _STOPPED_.

* **Paramètres obligatoires** : `uniqueId` : ID unique du mappage devant être démarré
* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Met fin à un mappage de domaine CDN sur la base du paramètre `uniqueId`. Pour s'arrêter, le mappage de domaine doit se trouver à l'état _RUNNING_.

* **Paramètres obligatoires** : `uniqueId` : ID unique du mappage devant être arrêté
* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
Permet à l'utilisateur de mettre à jour les propriétés du mappage identifiées par le paramètre `uniqueId`. Les zones suivantes peuvent être modifiées : les arguments `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header`, `cacheKeyQueryRule` et si votre type d'origine est Stockage d'objet, `bucketName` et `fileExtension` peuvent également être modifiés. Pour être mis à jour, le mappage de domaine doit se trouver à l'état _RUNNING_.

* **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Vous pouvez afficher tous les attributs du conteneur d'entrée ici :
  [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

  Les attributs suivants appartiennent au conteneur d'entrée et **doivent** être fournis lors de la mise à jour d'un mappage de domaine :
    * `vendorName` : Fournissez le nom du fournisseur de CDN de ce mappage.
    * `path` : Fournissez le chemin en cours de ce mappage
    * `origin` : Fournissez l'adresse du serveur d'origine sous forme de chaîne.
    * `originType` : Le type d'origine peut être `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `domain` : Fournissez votre nom d'hôte.
    * `protocol` : Les protocoles pris en charge sont `HTTP`, `HTTPS` ou `HTTP_AND_HTTPS`.
    * `httpPort` et/ou `httpsPort` : Ces deux options doivent correspondre au protocole souhaité. Si le protocole est `HTTP`, alors `httpPort` doit être défini et `httpsPort` ne doit _pas_ être défini. De même, si le protocole est `HTTPS`, alors `httpsPort` doit être défini et `httpPort` ne doit _pas_ être défini. Si le protocole est `HTTP_AND_HTTPS`, alors  `httpPort` et `httpsPort` _doivent_ être définis _à la fois_. Akamai possède certaines limitations au niveau des numéros de port. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports autorisés.
    * `header` : Spécifie les informations relatives aux en-têtes d'hôte utilisés par le serveur d'origine
    * `respectHeader` : Valeur booléenne qui, si elle définie sur `true`, entraîne le remplacement des paramètres TTL du CDN par les paramètres TTL du serveur d'origine.
    * `uniqueId` : Généré après la création du mappage.
    * `cname` : Fournissez le cname. Si vous ne l'avez pas fourni lors de la création du mappage, il a été créé automatiquement.
    * `bucketName` : (**obligatoire** pour le stockage d'objet uniquement) Nom de compartiment de votre stockage d'objet S3.
    * `fileExtension` : (**obligatoire** pour le stockage d'objet uniquement) Extensions de fichiers pouvant être mises en cache.
    * `cacheKeyQueryRule` : Les règles de comportement des clés de cache peuvent uniquement être mises à jour pour les mappages créés _après_ le 16/11/17. Les options suivantes sont disponibles pour configurer le comportement des clés de cache :
      * `include-all` - inclut tous les arguments de requête **default**
      * `ignore-all` - ignore tous les arguments de requête
      * `ignore: arguments de requête séparés par des espaces` - ignore ces arguments de requête spécifiques. Par exemple, `ignore: query1 query2`
      * `include: arguments de requête séparés par des espaces` : inclut ces arguments de requête spécifiques. Par exemple, `include: query1 query2`
* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Renvoie une collection de tous les mappages de domaines pour le client en cours.

* **Paramètres requis** : Aucun
* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Renvoie une collection avec un objet de domaine unique sur la base du paramètre `uniqueId` associé au CDN.

* **Paramètres obligatoires** : `uniqueId`: ID unique du mappage à renvoyer
* **Retour** : Collection d'objets uniques de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Afficher le conteneur de mappage](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## API du serveur d'origine
{: #apis-for-origin}

### createOriginPath
Crée un chemin d'origine pour un CDN existant et un client particulier. Le chemin d'origine peut être basé sur un serveur hôte ou sur un stockage d'objet. Pour créer le chemin d'origine, le mappage de domaine doit être dans un état _RUNNING_ ou _CNAME_CONFIGURATION_.

* **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

  [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

  Les attributs suivants appartiennent au conteneur d'entrée et peuvent être fournis lors de la création d'un chemin d'origine (les attributs sont facultatifs sauf indication contraire) :
    * `vendorName` : **obligatoire** Fournissez le nom d'un fournisseur IBM Cloud CDN valide.
    * `origin` : **obligatoire** Fournissez l'adresse du serveur d'origine sous forme de chaîne.
    * `originType` : **obligatoire** Le type du serveur d'origine peut être `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `domain` : **obligatoire** Fournissez votre nom d'hôte sous forme de chaîne.
    * `protocol` : **obligatoire** Les protocoles pris en charge sont `HTTP`, `HTTPS` ou `HTTP_AND_HTTPS`.
    * `path` : Chemin d'accès à partir duquel le contenu mis en cache sera distribué. Doit commencer par le chemin de mappage. Par exemple, si le chemin de mappage est `/test`, votre chemin d'origine peut être `/test/media`
    * `httpPort` et/ou `httpsPort` : **obligatoire** Ces deux options doivent correspondre au protocole souhaité. Si le protocole est `HTTP`, alors `httpPort` doit être défini et `httpsPort` ne doit _pas_ être défini. De même, si le protocole est `HTTPS`, alors `httpsPort` doit être défini et `httpPort` ne doit _pas_ être défini. Si le protocole est `HTTP_AND_HTTPS`, alors  `httpPort` et `httpsPort` _doivent_ être définis _à la fois_. Akamai possède certaines limitations au niveau des numéros de port. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports autorisés.
    * `header` : Spécifie les informations relatives aux en-têtes d'hôte utilisés par le serveur d'origine
    * `uniqueId` : **obligatoire** Généré après la création du mappage.
    * `cname` : Fournissez un alias pour le nom d'hôte. Si vous n'avez pas fourni, un cname unique a été généré pour vous lors de la création du mappage.
    * `bucketName` : (**obligatoire** pour le stockage d'objet) Nom de compartiment de votre stockage d'objet S3.
    * `fileExtension` : (facultatif pour le stockage d'objet) Extensions de fichiers autorisées à être mises en cache.
    * `cacheKeyQueryRule` : Les options suivantes sont disponibles pour configurer le comportement de la clé de cache :
      * `include-all` - inclut tous les arguments de requête **default**
      * `ignore-all` - ignore tous les arguments de requête
      * `ignore: arguments de requête séparés par des espaces` - ignore ces arguments de requête spécifiques. Par exemple, `ignore: query1 query2`
      * `include: arguments de requête séparés par des espaces` : inclut ces arguments de requête spécifiques. Par exemple, `include: query1 query2`

* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Afficher le conteneur de chemin d'origine](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Met à jour un chemin d'origine existant pour un mappage existant et un client particulier. Le type d'origine ne peut pas être changé avec cette API. Les propriétés suivantes peuvent être modifiées : `path`, `origin`, `httpPort` et `httpsPort`, `header` and `cacheKeyQueryRule` arguments. Pour être mis à jour, le mappage de domaine doit se trouver à l'état _RUNNING_ ou _CNAME_CONFIGURATION_.

* **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

  [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

  Les attributs suivants appartiennent au conteneur d'entrée et peuvent être fournis lors de la mise à jour d'un chemin d'origine (les attributs sont facultatifs sauf indication contraire) :
    * `oldPath` : **obligatoire** Chemin en cours à modifier
    * `origin` : (**obligatoire** en cas de mise à jour) Fournissez l'adresse du serveur d'origine sous forme de chaîne.
    * `originType` : **obligatoire** Le type du serveur d'origine peut être `HOST_SERVER` ou `OBJECT_STORAGE`.
    * `path` : **obligatoire** Nouveau chemin à ajouter. Relatif au chemin de mappage.
    * `httpPort` et/ou `httpsPort` : (**obligatoire** pour le serveur hôte en cas de mise à jour) Ces deux options doivent correspondre au protocole souhaité. Si le protocole est `HTTP`, alors `httpPort` doit être défini et `httpsPort` ne doit _pas_ être défini. De même, si le protocole est `HTTPS`, alors `httpsPort` doit être défini et `httpPort` ne doit _pas_ être défini. Si le protocole est `HTTP_AND_HTTPS`, alors  `httpPort` et `httpsPort` _doivent_ être définis _à la fois_. Akamai possède certaines limitations au niveau des numéros de port. Consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports autorisés.
    * `uniqueId` : **obligatoire** ID unique du mappage auquel appartient cette origine
    * `bucketName` : (**obligatoire** pour le stockage d'objet uniquement) Nom de compartiment de votre stockage d'objet S3.
    * `fileExtension` : (**obligatoire** pour le stockage d'objet uniquement) Extensions de fichiers pouvant être mises en cache.
    * `cacheKeyQueryRule` : (**obligatoire** en cas de mise à jour) Les règles de comportement de la clé de cache peuvent uniquement être mises à jour dans le cas de chemins d'origine créés _après_ le 16/11/17. Les options suivantes sont disponibles pour configurer le comportement de la clé de cache :
      * `include-all` - inclut tous les arguments de requête **default**
      * `ignore-all` - ignore tous les arguments de requête
      * `ignore: arguments de requête séparés par des espaces` - ignore ces arguments de requête spécifiques. Par exemple, `ignore: query1 query2`
      * `include: arguments de requête séparés par des espaces` : inclut ces arguments de requête spécifiques. Par exemple, `include: query1 query2`

* **Retour** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Afficher le conteneur de chemin d'origine](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Supprime un chemin d'origine existant d'un CDN existant, et pour un client particulier. Pour être supprimé, le mappage de domaine doit se trouver à l'état _RUNNING_ ou _CNAME_CONFIGURATION_.

* **Paramètres obligatoires**:
  * `uniqueId` : ID unique du mappage auquel appartient ce chemin d'origine
  * `path` : chemin à supprimer

* **Retour** : Message d'état si la suppression a réussi, sinon message d'exception.

----
### listOriginPath
Répertorie les chemins d'origine d'un mappage existant sur la base du paramètre `uniqueId`.

* **Paramètres obligatoires**:
  * `uniqueId` : Fournissez l'ID unique du mappage pour lequel vous souhaitez répertorier les chemins d'origine.
* **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Afficher le conteneur de chemin d'origine](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## API pour la purge
{: #api-for-purge}

### Classe de conteneur pour la purge :
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
Crée un enregistrement de purge et l'insère dans la base de données.

* **Paramètres** :
  * `uniqueId` : ID unique du mappage vers lequel la purge sera créée
  * `path` : chemin d'accès à la purge devant être créée

* **Retour** : collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
Renvoie l'historique de purge d'un CDN sur la base du paramètre `uniqueId` et du statut `saved`. (La valeur par défaut `saved` est définie sur _UNSAVED_.)

* **Paramètres** :
  * `uniqueId` : ID unique du mappage pour lequel l'historique de purge doit être extrait
  * `saved` : renvoie les purges sauvegardées (_SAVED_) ou non sauvegardées (_UNSAVED_)

* **Retour** : collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
Met à jour le statut de l'entrée du chemin de purge pour indiquer si la purge doit être sauvegardée ou non. Crée une nouvelle purge `saved` si un chemin de purge est sauvegardé. Supprime un enregistrement de purge sauvegardé si le chemin est défini sur `unsaved`.

* **Paramètres** :
  * `uniqueId`: ID unique du mappage auquel appartient la purge
  * `path` : chemin d'accès à la purge à sauvegarder ou non
  * `saved` : sauvegardé (_SAVED_) ou non sauvegardé (_UNSAVED_)

* **Retour** : collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
## API pour la durée de vie
{: #api-for-time-to-live}

### Variables de la classe TimeToLive :  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
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

 ----
## API pour Metrics
{: #api-for-metrics}

[Afficher le conteneur Metrics](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
Renvoie le nombre total de statistiques prédéterminées avec affichage direct (non graphique) d'un compte client, au cours d'une période donnée.

 * **Paramètres** :
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingUsageMetrics
Renvoie le nombre total de statistiques avec affichage direct du mappage concerné. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** :
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsMetrics
Renvoie le nombre total de correspondances à une certaine fréquence au cours d'un laps de temps donné, par mappage de domaine. La fréquence peut être au format 'jour', 'semaine' et 'mois', où chaque intervalle correspond à un point de tracé sur le graphique. Les données renvoyées sont ordonnées en fonction des valeurs `startDate`, `endDate` et `frequency`. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** :
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Renvoie le nombre total de correspondances à une certaine fréquence au cours d'un laps de temps donné. La fréquence peut être au format 'jour', 'semaine' et 'mois', où chaque intervalle correspond à un point de tracé sur le graphique. Les données renvoyées doivent être ordonnées en fonction des valeurs `startDate`, `endDate` et `frequency`. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** :
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Renvoie le nombre de correspondances vers le serveur d'équilibrage des charges pour un CDN individuel. Les régions peuvent être différentes d'un fournisseur à l'autre. Par mappage.

 * **Paramètres** :  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Renvoie le nombre total de statistiques prédéterminées avec affichage direct (non graphique) d'un mappage donné, au cours d'une période donnée. La valeur `frequency` est agrégée par défaut.

 * **Paramètres** :
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Retour** : Collection d'objets de type `SoftLayer_Container_Network_CdnMarketplace_Metrics`

----
## API pour le contrôle d'accès géographique
{: #api-for-geographical-access-control}

### createGeoblocking
Crée une nouvelle règle de contrôle d'accès géographique et renvoie la règle nouvellement créée.

  * **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

    [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

    Les attributs suivants appartiennent au conteneur d'entrée et sont **requis** lors de la création d'une nouvelle règle de contrôle d'accès géographique :
    * `uniqueId` : ID unique du mappage pour affecter la règle
    * `accessType` : indique si la règle autorisera (`ALLOW`) ou refusera (`DENY`) le trafic vers la région concernée.
    * `regionType` : type de région à laquelle appliquer la règle de contrôle d'accès géographique, à savoir `CONTINENT` ou `COUNTRY_OR_REGION`
    * `regions`: tableau répertoriant les emplacements auxquels le type d'accès (`accessType`) s'appliquera

      Pour obtenir une liste de régions possibles, voir la page [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class).

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Afficher la classe Geo-blocking](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Met à jour une règle de contrôle d'accès géographique existante pour un mappage de domaine existant et renvoie la règle mise à jour.

  * **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

    [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

    Les attributs suivants appartiennent au conteneur d'entrée et peuvent être fournis lors de la mise à jour d'une règle de contrôle d'accès géographique (les paramètres sont facultatifs sauf indication contraire) :
    * `uniqueId` : ID unique **requis** du mappage auquel appartient la règle à mettre à jour
    * `accessType` : indique si la règle autorisera (`ALLOW`) ou refusera (`DENY`) le trafic vers la région concernée.
    * `regionType` : type de région à laquelle appliquer la règle, à savoir `CONTINENT` ou `COUNTRY_OR_REGION`
    * `regions`: tableau répertoriant les emplacements auxquels le type d'accès (`accessType`) s'appliquera

      Pour obtenir une liste de régions possibles, voir la page [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class).

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Afficher la classe Geo-blocking](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Retirer une règle de contrôle d'accès géographique d'un mappage de domaine existant.

  * **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

    [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

    Les attributs suivants appartiennent au conteneur d'entrée et sont **requis** lors de la suppression d'une règle de contrôle d'accès géographique :
    * `uniqueId` : fournissez l'ID unique du mappage auquel appartient la règle à supprimer.

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Afficher la classe Geo-blocking](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Extrait de la base de données le comportement de contrôle d'accès géographique d'un mappage.

  * **Paramètres** :
    * `uniqueId` : ID unique du mappage auquel appartient la règle.

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Afficher la classe Geo-blocking](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Renvoie une liste des types et des régions admis pour la création de règles de contrôle d'accès géographique.

  * **Paramètres** :
    * `uniqueId` : ID unique de votre mappage de domaine.

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`

    [Afficher la classe Geo-blocking](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## API pour la protection des liens dynamiques
{: #api-for-hotlink-protection}

### createHotlinkProtection
Crée une nouvelle protection des liens dynamiques et renvoie le comportement nouvellement créé.

  * **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

    [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

    Les attributs suivants appartiennent au conteneur d'entrée et sont **requis** lors de la création d'une nouvelle protection des liens dynamiques :
    * `uniqueId` : ID unique du mappage auquel affecter le comportement
    * `protectionType` : indique s'il convient d'autoriser (ALLOW) ou de refuser (DENY) l'accès à votre contenu lorsqu'une page Web émet une demande de contenu avec une valeur d'en-tête Referer qui correspond à l'un des termes indiqués dans refererValues
    * `refererValues` : liste de termes de correspondance d'URL de référence séparés par un unique espace auxquels le comportement `protectionType` sera appliqué

      Pour la liste des valeurs de protection des liens dynamiques valides, voir la page [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class).

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Afficher la classe de protection des liens dynamiques](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Met à jour un comportement de protection des liens dynamiques existante pour un mappage de domaine existant et renvoie le comportement mis à jour.

  * **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

    [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

    Les attributs suivants appartiennent au conteneur d'entrée et sont **requis** lors de la mise à jour d'une protection des liens dynamiques existante :
    * `uniqueId` : ID unique du mappage auquel appartient le comportement existant
    * `protectionType` : indique s'il convient d'autoriser (ALLOW) ou de refuser (DENY) l'accès à votre contenu lorsqu'une page Web émet une demande de contenu avec une valeur d'en-tête Referer qui correspond à l'un des termes indiqués dans refererValues 
    * `refererValues` : liste de termes de correspondance d'URL de référence séparés par un unique espace auxquels le comportement `protectionType` sera appliqué

      Pour la liste des valeurs de protection des liens dynamiques valides, voir la page [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class).

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Afficher la classe de protection des liens dynamiques](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Supprime un comportement de protection des liens dynamiques existante d'un mappage de domaine existant.

  * **Paramètres** : Collection de type `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Vous pouvez afficher tous les attributs du conteneur d'entrée ici :

    [Afficher le conteneur d'entrée](/docs/infrastructure/CDN?topic=CDN-input-container)

    Les attributs suivants appartiennent au conteneur d'entrée et sont **requis** lors de la création d'une nouvelle protection des liens dynamiques :
    * `uniqueId` : ID unique du mappage à partir duquel supprimer le comportement

  * **Retour** : null

----
### getHotlinkProtection
Extrait un comportement de protection des liens dynamiques en cours d'un mappage.

  * **Paramètres** :
    * `uniqueId` : ID unique du mappage auquel appartient le comportement

  * **Retour** : objet de type `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Afficher la classe de protection des liens dynamiques](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
