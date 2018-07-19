---

copyright:
  years: 2017,2018
lastupdated: "2018-06-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Commande d'un CDN

La présente section vous explique comment commander un réseau de distribution de contenu (CDN). Vous pouvez commander votre CDN soit sur le [portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/), soit sur le [portail Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## A partir du portail de contrôle :

**Etape 1 :**

Pour commencer, connectez-vous au [Portail client ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/) à l'aide de vos identifiants uniques.

**Etape 2 :**

Dans la barre de navigation située en haut de l'écran, sélectionnez **Network -> CDN**.

   ![Network menu options](images/network-cdn.png)

**Etape 3 :**

Sur la page **Content Delivery Networks**, cliquez sur le bouton **Order CDN** en haut à droite de la page.

   ![Select order CDN](images/order-cdn-button.png)

## Depuis le portail IBM Cloud

**Etape 1 :**

Connectez-vous au portail [IBM Cloud](https://www.ibm.com/cloud-computing/bluemix/)

**Etape 2 :**

Cliquez sur [Catalogue IBM Cloud](https://console.bluemix.net/catalog/). Dans la barre de navigation située dans la partie gauche de l'écran, sélectionnez **Network**.

   ![Bluemix CDN Navigation](images/bluemix_navigation.png)

**Etape 3 :**

Cliquez sur **CDN Tile** pour accéder à la page de sélection du fournisseur.

   ![Bluemix CDN Icon](images/bluemix_tile.png)


**Etape 4 :**

Dans l'écran **Select a CDN Provider**, choisissez un fournisseur de CDN parmi les options proposées. Cliquez sur le bouton **Select** pour confirmer les options sélectionnées puis cliquez sur **Next** en bas à droite de votre écran pour commencer le processus de mise à disposition.  
       ![Select CDN provider](images/Vendor_Select_And_Provision.png)

**Etape 5 :**

Renseignez la zone **Configure Name** :  

  * Spécifiez la zone **Hostname** (zone **obligatoire**), qui sert d'identificateur principal pour votre CDN (par exemple, _example.testingcdn.net_).  
  * Vous pouvez aussi fournir une valeur **CNAME** personnalisée (telle que _myfirstcdn.cdnedge.bluemix.net_). Si aucune valeur CNAME n'est saisie, une valeur est créée pour vous. <validation information to be included here>  

       ![Configure Name](images/configure-hostname-cname.png)  

    **Remarque** : L'utilisation d'un CNAME inapproprié peut provoquer l'arrêt des services.

**Etape 6 :**

Renseignez la zone **Configure Your Origin** : Pour configurer cette zone, sélectionnez soit **Server**, soit **Object Storage**.  

   * Spécifiez **Host header** (facultatif). Si aucun en-tête d'hôte n'est fourni, la valeur de **Hostname** sera utilisée par défaut. Pour en savoir plus sur l'en-tête d'hôte, consultez la description de la fonction de [prise en charge des en-têtes d'hôte](about.html#host-header-support-).  

   * Fournissez un chemin d'accès dans **Path** (facultatif). Le chemin d'accès doit être relatif au nom d'hôte spécifié dans **Hostname**.

      ![Configure Origin](images/configure-origin.png)  

  * Option **Server** : Si vous sélectionnez l'option **Server**, indiquez le nom d'hôte ou l'adresse IP du serveur d'origine à partir duquel les données sont mises en cache.
      * Renseignez la zone **Origin Server Address** (nom d'hôte ou adresse IPv4 du serveur d'origine) si vous avez sélectionné cette option. Si **HTTPS port** est sélectionné, l'adresse figurant sous **Origin Server Address** doit être celle du nom d'hôte et non l'adresse IP.
      * Vous pouvez également spécifier les zones **HTTP port** et/ou **HTTPS port**. Ces zones indiquent quels protocoles et numéros de port peuvent être utilisés pour contacter le serveur d'origine. Pour utiliser des numéros de port qui ne sont pas ceux définis par défaut, consultez la [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai) pour obtenir la liste des numéros de port autorisés.
      * **SSL Certificate** Cette option apparaît _uniquement_ quand HTTPS Port est sélectionné. \*Des informations supplémentaires relatives aux configurations de certificats SSL et HTTPS suivent la description de l'option Object Storage.

	     ![Configure origin server](images/configure-origin-server.png)

  * Option **Object Storage** : Si vous sélectionnez l'option **Object Storage**, fournissez les informations suivantes :
      * le noeud final (**Endpoint**) à partir duquel l'objet doit être récupéré,
      * le nom du compartiment (**Bucket**) dans lequel est stocké le contenu, et
      * le port HTTPS (**HTTPS port**).
      * **SSL Certificate** Cette option apparaît _uniquement_ quand HTTPS Port est sélectionné. \*Des informations supplémentaires relatives aux configurations de certificats SSL et HTTPS suivent la description de l'option Object Storage.
      * Vous pouvez également indiquer les extensions de fichiers, séparées par des virgules, susceptibles d'être utilisées dans le service CDN. (Si aucune extension n'est sélectionnée, toutes les extensions de fichiers sont admises.)
      * Définissez la liste de contrôle d'accès (**Access Control List**) de chaque **objet** dans votre **compartiment** sur "public-read".

      	  ![Configure object storage](images/configure-origin-cos.png)

  * **SSL Certificate** : si vous sélectionnez **HTTPS Port** pour Server ou pour Object Storage, vous pouvez choisir **Wildcard** ou **DV SAN Certificate** en tant qu'option **SSL Certificate**. Ces deux choix offrent la sécurité renforcée fournie par HTTPS.
    * **Wildcard Certificate** autorise le trafic HTTPS uniquement en cas d'utilisation de **CNAME** et ne nécessite aucune autre action de votre part.
    * **DV SAN Certificate** permet un trafic HTTPS sur votre domaine mais requiert des étapes supplémentaires de vérification.

        ![Configuration HTTPS](images/configure-https.png)


**Etape 7 :**

Configurez la zone **Other Options** : cette section contient des options de configuration pour la zone **Respect Headers**.

   * Lorsque l'option **Respect Headers** est définie sur **On**, les paramètres TTL définis dans l'en-tête par l'origine remplaceront les paramètres TTL par défaut du CDN. La zone **Respect Headers** est définie sur **On** par défaut, mais vous devez configurer cette zone.  

        ![Other options](images/other-options.png)

**Etape 8 :**

Cliquez sur le bouton **Create** en bas à droite de l'écran pour créer votre CDN.
