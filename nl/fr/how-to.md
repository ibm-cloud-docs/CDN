---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

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
{:DomainName: data-hd-keyref="DomainName"}

# Gestion de votre CDN
{: #manage-your-cdn}

Ce document décrit les tâches courantes de gestion de votre CDN.

## Configuration de l'heure de mise en cache du contenu à l'aide de l'option "Time To Live" (Durée de vie)
{: #setting-content-caching-time-using-time-to-live}

Une fois votre CDN en cours d'exécution, vous pouvez définir l'heure de mise en cache du contenu à l'aide de la fonction de durée de vie Time To Live (TTL). La durée de vie d'un chemin de fichier ou de répertoire spécifique indique la durée pendant laquelle le contenu doit être mis en cache. Lorsque vous avez créé le mappage CDN, une valeur TTL globale de 3 600 secondes (1 heure) par défaut a été créée.

**Etape 1 :**  

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.

**Etape 2 :**  

Vous pouvez ajuster l'heure en utilisant les flèches ou en saisissant une nouvelle valeur. Cette valeur est exprimée en secondes. Par exemple, 3 600 secondes représentent 1 heure. La plus petite valeur admise pour `timeToLive` est 0 seconde, tandis que la valeur la plus élevée est 2147483647 secondes (environ 24855 jours). Cliquez sur le bouton **Save** pour définir l'heure de mise en cache du contenu.

  ![Adding ttl](images/adding-path.png)

**Etape 3 :**

Une fois la sauvegarde terminée, vous pouvez **modifier** ou **supprimer** le paramètre TTL à l'aide des options du menu déroulant dynamique. (**Remarque** : le chemin d'accès à TTL n'est pas modifiable. Si le chemin de mappage est changé, le chemin d'accès à TTL est mis à jour automatiquement.)

  ![Modification ou suppression du paramètre TTL](images/edit-delete-ttl-setting.png)  

  * Lorsque le contenu correspond à plusieurs règles, la dernière configuration ajoutée est prioritaire.

  * Les valeurs TTL peuvent être définies uniquement pour un nom de fichier ou un répertoire spécifique. Les expressions régulières ne sont pas prises en charge, car elles risquent d'entraîner un comportement imprévisible.

## Ajout de détails sur le chemin d'origine
{: #adding-origin-path-details}

Lorsque votre CDN a le statut *CNAME_Configuration* ou *Running*, vous pouvez ajouter des informations complémentaires sur le chemin d'origine. Vous pouvez décider de fournir du contenu à partir de plusieurs serveurs d'origine. Par exemple, les photos peuvent être basées sur un serveur différent de celui des vidéos. L'origine peut être un serveur hôte ou un stockage d'objets.

Le CDN transforme l'URL du serveur d'origine. Par exemple, si l'origine `xyz.example.com` est ajoutée avec le chemin `/example/*` lorsqu'un utilisateur ouvre l'URL `www.example.com/example/*`, le serveur d'équilibrage des charges du CDN extrait le contenu depuis `xyz.example.com/*`.
{: note}

**Etape 1 :**

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.  

**Etape 2 :**

Sélectionnez l'onglet **Origins**, puis cliquez sur **Add Origin**. Cette étape ouvre une nouvelle boîte de dialogue dans laquelle vous pouvez configurer votre origine.  

   ![Origins add origin](images/add-origins.png)

**Etape 3 :**

Vous *devez* fournir un chemin d'accès. Vous pouvez fournir un en-tête d'hôte si vous le souhaitez.  

   ![Origins add origin](images/add-origin-path.png)

**Etape 4 :**

Sélectionnez soit **Server**, soit **Object Storage**.

  * Si vous avez choisi **Server**, entrez l'adresse du serveur d'origine comme adresse IPv4 ou son _nom d'hôte_. Il est recommandé de fournir le nom d'hôte et un nom de domaine complet (FQDN). Suivant le protocole qui a été sélectionné durant la création du CDN, fournissez également un port HTTP, un port HTTPS ou les deux. Si vous utilisez un port HTTPS, l'adresse du serveur d'origine **doit** être un _nom d'hôte_ et non une adresse IP.

       ![Add origin server](images/add-origin-server-default.png)

  * Si vous avez sélectionné **Object Storage**, indiquez le noeud final, le nom du compartiment et le port HTTPS. Vous pouvez également spécifier les extensions de fichiers pouvant être utilisées dans le service CDN. Si vous ne définissez pas d'extensions spécifiques, toutes les extensions de fichiers sont autorisées.

       ![Add origin object storage](images/add-origin-object-storage.png)

  * Les options d'**optimisation** et de **clé de cache** sont les mêmes pour la configuration du serveur et du stockage d'objet.

    * Sélectionnez les options d'**optimisation** dans le menu déroulant. La **livraison Web générale** est l'option par défaut, mais vous pouvez aussi sélectionner les **optimisations de fichiers volumineux** ou la **vidéo à la demande**. L'optimisation de la **livraison Web générale** permet au CDN de servir un contenu de jusqu'à 1,8 Go, alors que l'optimisation des **fichiers volumineux** permet des téléchargements de fichiers de 1,8 Go à 320 Go. La **vidéo à la demande** optimise votre CDN pour la livraison de formats de diffusion en flux segmentés. Pour tout renseignement additionnel, voir les descriptions des fonctions [Optimisation des fichiers volumineux](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization) et [Vidéo à la demande](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand).

        ![Options de configuration des performances](images/performance-config-options.png)

    * Sélectionnez les options de **clé de cache** dans le menu déroulant. L'option par défaut est **Include-all**. Si vous sélectionnez **Include specified** ou **Ignore specified**, vous **devez** entrer des chaînes de requête à inclure ou à ignorer, séparées par un espace. Par exemple, entrez `uuid=123456` pour une chaîne de requête unique, ou `uuid=123456 issue=important` pour deux chaînes de requête.  Pour en savoir plus sur la fonction [Cache Key Query Args](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args), consultez la description de la fonction.

        ![Options de clés de cache](images/cache-key-options.png)

Les options relatives au protocole et au port affichées sur l'interface utilisateur correspondront aux sélections effectuées lors de la commande du CDN. Par exemple, si **HTTP port** a été sélectionné lors de la commande d'un CDN, seule l'option **HTTP port** apparaîtra comme choix de l'option d'ajout d'une origine.
{: note}

**Etape 5 :**

Cliquez sur le bouton **Add** pour ajouter votre chemin d'origine.

Lorsque vous indiquez des extensions de fichier pour le chemin d'origine du stockage des objets, le paramètre TTL dont l'adresse URL est identique au chemin d'origine est étendue de manière à inclure tous les fichiers comportant les extensions spécifiées. Par exemple, si vous créez le chemin d'origine `/example` et spécifiez les extensions de fichier "jpg png gif", la valeur TTL du chemin TTL `/example` inclura tous les fichiers JPG/PNG/GIF sous le répertoire `/example` et ses sous-répertoires.
{: note}

**Etape 6 :**

Après avoir ajouté une origine, vous pouvez la **modifier** ou la **supprimer** en utilisant les options du menu déroulant dynamique.

  ![Edit or delete Origin](images/edit-delete-origin.png)

## Purge du contenu mis en cache
{: #purging-cached-content}

Une fois votre CDN en cours d'exécution, vous pouvez purger le contenu mis en cache sur le serveur du fournisseur.

**Etape 1 :**

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.

**Etape 2 :**

Sélectionnez l'onglet **Purge**.

   ![Purge page](images/purge_tab.png)

**Etape 3 :**

Entrez le chemin d'accès du fichier à purger au format UNIX, puis cliquez sur le bouton **Purge**. La purge est autorisée pour un seul fichier à la fois. Consultez la page [Règles et conventions de dénomination](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-) pour plus de détails sur la syntaxe autorisée pour le chemin de purge.

**Etape 4 :**

Une fois la purge terminée, l'activité figure sous **Purge Activity**. Vous pouvez sélectionner l'option **Redo purge** ou **Favorite** du menu déroulant dynamique.

   ![Purge activity](images/purge-activity.png)

S'il existe plus de 15 purges, l'activité de purge est effacée automatiquement tous les 15 jours.
{: note}

## Mise à jour des informations relatives à la configuration CDN
{: #updating-cdn-configuration-details}

Lorsque votre CDN est en cours d'exécution, vous pouvez mettre à jour les détails de la configuration CDN.

**Etape 1 :**

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.

**Etape 2 :**

Sélectionnez l'onglet **Settings**. Les détails concernant votre configuration CDN sont affichés.

   ![Onglet Settings](images/settings-tab.png)  

Vous verrez le certificat SSL uniquement si votre CDN a été configuré avec HTTPS.
{: note}

Les zones suivantes peuvent être modifiées sous **Server** :
  * Host header
  * Origin server address
  * HTTP/HTTPS Port
  * Serve Stale Content
  * Respect Headers
  * Optimization options
  * Cache-query    

Les zones suivantes peuvent être modifiées sous **Object Storage** :
  * Host header
  * Endpoint
  * Bucket name
  * HTTPS Port
  * Allowed file extensions
  * Serve Stale Content
  * Respect Headers
  * Optimization options
  * Cache-query

**Etape 3 :**

Mettez à jour les détails relatifs aux zones **Origin** ou **Other Options** le cas échéant, puis cliquez sur le bouton **Save** situé dans l'angle inférieur droit de l'écran pour mettre à jour les détails liés à votre configuration CDN.

   ![Save button](images/save-button.png)
