---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gestion de votre CDN

Apprenez à gérer la configuration de votre CDN grâce aux consignes ci-dessous.

## Obtention du statut d'exécution du CDN

Une fois que vous avez créé un CDN, il apparaît sur votre tableau de bord des CDN. Vous voyez le nom du CDN, son origine, son fournisseur et son statut.  

 ![Capture d'écran de la liste des mappages](images/mapping_list_cname.png)

**Etape 1 :**

Une fois que vous avez commandé un CDN, vous devez configurer le **CNAME** avec votre fournisseur DNS. La plupart des fournisseurs DNS peuvent vous fournir des instructions sur la manière de définir ou de modifier le CNAME.

   * Pendant ce temps, le statut de votre CDN affichera **CNAME Configuration**. Contactez votre fournisseur DNS pour savoir quand seront prises en compte les modifications.

   ![CNAME config](images/cname-config.png)  

**Etape 2 :**  

Après avoir configuré le CNAME avec votre fournisseur DNS, vous pouvez vérifier à tout moment son statut en sélectionnant **Get Status** à partir du menu déroulant dynamique situé à droite du statut de votre CDN.

  ![CNAME getStatus](images/cname-getstatus.png)  

**Etape 3 :**  

Une fois le chaînage CNAME terminé, le statut du compte devient *RUNNING* et le CDN est prêt à être utilisé.

Félicitations ! Votre CDN est désormais actif.  

## Arrêt du CDN
Le CDN ne peut être arrêté que lorsque son statut est "En cours d'exécution".

**Etape 1 :**  

Cliquez sur "Stop CDN" à partir du menu déroulant dynamique (3 points verticaux en regard du statut CDN).
 ![Overflow menu](images/stop_cdn.png)

**Etape 2 :**  

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer l'arrêt du service. Sélectionnez **Confirm** pour poursuivre.

**Etape 3 :**  

Après environ 5-15 secondes, le statut devient "Arrêté".


## Démarrage du CDN
Le CDN ne peut être démarré que lorsque son statut est "Arrêté".  

**Etape 1 :**  

Cliquez sur "Start CDN" à partir du menu déroulant dynamique, à savoir les 3 points à droite de la ligne du CDN.
 ![Overflow menu](images/start_cdn.png)

**Etape 2 :**  

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer le démarrage du service. Sélectionnez **Confirm** pour poursuivre.

**Etape 3 :**  

Si l'action aboutit, une boîte de dialogue apparaît dans l'angle supérieur droit de l'écran, indiquant que l'action a été effectuée avec succès, avec l'heure.

**Etape 4 :**  

Cette étape change le statut par 'CNAME Configuration'

**Etape 5 :**  

Cliquez sur l'option "Get Status" du menu déroulant dynamique. Cette étape change le statut par 'Running'. Votre CDN devient opérationnel.

## Suppression du CDN

Pour supprimer un CDN, procédez comme suit :

**REMARQUE** : la sélection de `Delete` depuis le menu déroulant dynamique supprime uniquement le CDN, elle ne supprime pas votre compte.

**Etape 1 :**  

Cliquez sur "Delete" à partir du menu déroulant dynamique.

 ![Delete CDN Overflow menu](images/delete_cdn.png)

**Etape 2 :**  

Une boîte de dialogue plus grande s'ouvre, vous demandant de confirmer la suppression. Cliquez sur **Delete** pour poursuivre.

**Etape 3 :**  

Cette étape remplace le statut par 'Deleting'. Cliquez sur 'Get Status' dans le menu déroulant dynamique et supprimez la ligne de la liste des CDN.

## Configuration de l'heure de mise en cache du contenu à l'aide de l'option "Time To Live" (Durée de vie)

Une fois votre CDN en cours d'exécution, vous pouvez définir l'heure de mise en cache du contenu à l'aide de la fonction de durée de vie Time To Live (TTL). La durée de vie d'un chemin de fichier ou de répertoire spécifique indique la durée pendant laquelle le contenu doit être mis en cache. Lorsque vous avez créé le mappage CDN, une valeur TTL globale de 3 600 secondes par défaut a été créée.

**Etape 1 :**  

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.

**Etape 2 :**  

Vous pouvez ajuster l'heure en utilisant les flèches ou en saisissant une nouvelle valeur. Cette valeur est exprimée en secondes. Par exemple, 3 600 secondes représentent 1 heure. La plus petite valeur admise pour `timeToLive` est 30 secondes, tandis que la valeur la plus grande est 2 147 483 647 secondes. Cliquez sur le bouton **Save** pour définir l'heure de mise en cache du contenu.

  ![Adding ttl](images/adding-path.png)

**Etape 3 :**

Une fois la sauvegarde terminée, vous pouvez **modifier** ou **supprimer** le paramètre TTL à l'aide des options du menu déroulant dynamique. (**Remarque** : le chemin d'accès à TTL n'est pas modifiable. Si le chemin de mappage est changé, le chemin d'accès à TTL est mis à jour automatiquement.)

  ![Modification ou suppression du paramètre TTL](images/edit-delete-ttl-setting.png)  

  * Lorsque le contenu correspond à plusieurs règles, la dernière configuration ajoutée est prioritaire.

  * Les valeurs TTL peuvent être définies uniquement pour un nom de fichier ou un répertoire spécifique. Les expressions régulières ne sont pas prises en charge, car elles risquent d'entraîner un comportement imprévisible.

## Ajout de détails sur le chemin d'origine

Lorsque votre CDN a le statut *CNAME_Configuration* ou *Running*, vous pouvez ajouter des informations complémentaires sur le chemin d'origine. Vous pouvez décider de fournir du contenu à partir de plusieurs serveurs d'origine. Par exemple, les photos peuvent être basées sur un serveur différent de celui des vidéos. L'origine peut être un serveur hôte ou un stockage d'objets.

**Remarque :** Le CDN transforme l'URL du serveur d'origine. Par exemple, si l'origine `xyz.example.com` est ajoutée avec le chemin `/example/*` lorsqu'un utilisateur ouvre l'URL `www.example.com/example/*`, le serveur d'équilibrage des charges du CDN extrait le contenu depuis `xyz.example.com/*`.

**Etape 1 :**  

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.  

**Etape 2 :**  

Sélectionnez l'onglet **Origins**, puis cliquez sur **Add Origin**. Cette étape ouvre une nouvelle boîte de dialogue dans laquelle vous pouvez configurer votre origine.  

   ![Origins add origin](images/add-origins.png)

**Etape 3 :**  

Vous *devez* fournir un chemin d'accès. Celui-ci doit *obligatoirement* commencer par le chemin du CDN comme préfixe, si le CDN a été créé avec un chemin.  
Par exemple, si le CDN a été créé avec le chemin `/examplePath`, le chemin d'accès à l'origine doit commencer par le préfixe `/examplePath/`. Vous pouvez fournir un en-tête d'hôte si vous le souhaitez.  
  
   ![Origins add origin](images/add-origin-path.png)

**Etape 4 :**  

Sélectionnez soit **Server**, soit **Object Storage**.

  * Si vous avez choisi **Server**, entrez l'adresse du serveur d'origine comme adresse IPv4 ou son _nom d'hôte_. Il est recommandé de fournir le nom d'hôte et un nom de domaine complet (FQDN). Suivant le protocole qui a été sélectionné durant la création du CDN, fournissez également un port HTTP, un port HTTPS ou les deux. Si vous utilisez un port HTTPS, l'adresse du serveur d'origine **doit** être un _nom d'hôte_ et non une adresse IP.
  
       ![Add origin server](images/add-origin-server-default.png)

  * Si vous avez sélectionné **Object Storage**, indiquez le noeud final, le nom du compartiment et le port HTTPS. Vous pouvez également spécifier les extensions de fichiers pouvant être utilisées dans le service CDN. Si vous ne définissez pas d'extensions spécifiques, toutes les extensions de fichiers sont autorisées.
  
       ![Add origin object storage](images/add-origin-object-storage.png)

  * Les options d'**optimisation** et de **clé de cache** sont les mêmes pour la configuration du serveur et du stockage d'objet.
  
    * Sélectionnez les options d'**optimisation** dans le menu déroulant. La **livraison Web générale** est l'option par défaut, mais vous pouvez aussi sélectionner les **optimisations de fichiers volumineux** ou la **vidéo à la demande**. L'optimisation de la **livraison Web générale** permet au CDN de servir un contenu de jusqu'à 1,8 Go, alors que l'optimisation des **fichiers volumineux** permet des téléchargements de fichiers de 1,8 Go à 320 Go. La **vidéo à la demande** optimise votre CDN pour la livraison de formats de diffusion en flux segmentés. Pour tout renseignement additionnel, voir les descriptions des fonctions [Optimisation des fichiers volumineux](about.html#large-file-optimization-) et [Vidéo à la demande](about.html#video-on-demand-). 
    
        ![Options de configuration des performances](images/performance-config-options.png)
    
    * Sélectionnez les options de **clé de cache** dans le menu déroulant. L'option par défaut est **Include-all**. Si vous sélectionnez **Include specified** ou **Ignore specified**, vous **devez** entrer des chaînes de requête à inclure ou à ignorer, séparées par un espace. Par exemple, entrez `uuid=123456` pour une chaîne de requête unique, ou `uuid=123456 issue=important` pour deux chaînes de requête. Pour en savoir plus sur la fonction [Cache Key Query Args](about.html#ignore-query-args-in-cache-key-feature-), consultez la description de la fonction.
    
        ![Options de clés de cache](images/cache-key-options.png)
    
**REMARQUE** : les options relatives au protocole et au port affichées sur l'interface utilisateur correspondront aux sélections effectuées lors de la commande du CDN. Par exemple, si **HTTP port** a été sélectionné lors de la commande d'un CDN, seule l'option **HTTP port** apparaîtra comme choix de l'option d'ajout d'une origine.

**Etape 5 :**  

Cliquez sur le bouton **Add** pour ajouter votre chemin d'origine.

  **Remarque** : Lorsque vous indiquez des extensions de fichier pour le chemin d'origine du stockage des objets, le paramètre TTL dont l'adresse URL est identique au chemin d'origine est étendue de manière à inclure tous les fichiers comportant les extensions spécifiées. Par exemple, si vous créez le chemin d'origine `/example` et spécifiez les extensions de fichier "jpg png gif", la valeur TTL du chemin TTL `/example` inclura tous les fichiers JPG/PNG/GIF sous le répertoire `/example` et ses sous-répertoires.

**Etape 6 :**  

Après avoir ajouté une origine, vous pouvez la **modifier** ou la **supprimer** en utilisant les options du menu déroulant dynamique.

  ![Edit or delete Origin](images/edit-delete-origin.png)

## Purge du contenu mis en cache

Une fois votre CDN en cours d'exécution, vous pouvez purger le contenu mis en cache sur le serveur du fournisseur.  

**Etape 1 :**  

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.

**Etape 2 :**  

Sélectionnez l'onglet **Purge**.

   ![Purge page](images/purge_tab.png)

**Etape 3 :**  

Entrez le chemin d'accès du fichier à purger au format UNIX, puis cliquez sur le bouton **Purge**. Pour l'instant, seule la purge d'un fichier unique est prise en charge.

**Etape 4 :**  

Une fois la purge terminée, l'activité figure sous **Purge Activity**. Vous pouvez sélectionner l'option **Redo purge** ou **Favorite** du menu déroulant dynamique.

   ![Purge activity](images/purge-activity.png)

   **Remarque :** S'il existe plus de 15 purges, l'activité de purge est effacée automatiquement tous les 15 jours.

## Mise à jour des informations relatives à la configuration CDN

Lorsque votre CDN est en cours d'exécution, vous pouvez mettre à jour les détails de la configuration CDN.

**Etape 1 :**

Sur la page des CDN, sélectionnez votre CDN pour accéder à la page **Overview**.

**Etape 2 :**  

Sélectionnez l'onglet **Settings**. Les détails concernant votre configuration CDN sont affichés.

   ![Onglet Settings](images/settings-tab.png)

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


## Configuration d'IBM Cloud Object Storage pour CDN

Pour pouvoir utiliser des objets stockés dans IBM Cloud Object Storage, définissez la propriété "acl" (à savoir, la liste de contrôle d'accès) pour chacun des objets contenus dans votre compartiment sur la valeur d'accès "public-read".

Consultez la rubrique Tools dans IBM Cloud Object Storage Developer Center (https://developer.ibm.com/cloudobjectstorage/) pour installer tous les clients ou outils nécessaires. Ce guide suppose que vous avez installé l'interface de ligne de commande AWS officielle, compatible avec l'API IBM Cloud Object Storage S3.

L'exemple de code ci-dessous indique comment configurer l'accès "public-read" pour tous les objets de votre compartiment à l'aide de l'interface de ligne de commande.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```
