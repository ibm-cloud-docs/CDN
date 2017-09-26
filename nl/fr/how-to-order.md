---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Procédure de commande d'un CDN

La présente section va vous expliquer comment commander un CDN (Content Delivery Network, réseau de distribution de contenu). Vous pouvez commander votre CDN soit à la page [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/), soit à la page [Bluemix Catalog Portal](https://www.ibm.com/cloud-computing/bluemix/).

## A partir du portail de contrôle :

1. Pour commencer, connectez-vous au [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/) à l'aide de vos identifiants unique.

2. Dans la barre de navigation située en haut de l'écran, sélectionnez **Network -> CDN**.

   ![Network menu options](images/network-cdn.png)

3. Sur la page **Content Delivery Networks**, cliquez sur le bouton **Order CDN** en haut à droite de la page.

	![Select order CDN](images/order-cdn-button.png)

## A partir du portail Bluemix :

1. Connectez-vous au [Portail Bluemix](https://www.ibm.com/cloud-computing/bluemix/)

2. Cliquez sur **IBM Bluemix Catalog**. Dans la barre de navigation située dans la partie gauche de l'écran, sélectionnez **Network**.

   ![Bluemix CDN Navigation](images/bluemix_navigation.png)

3. Cliquez sur **CDN Tile** pour accéder à la page de sélection du fournisseur.

   ![Bluemix CDN Icon](images/bluemix_tile.png)

4. Dans l'écran **Select a CDN Provider**, choisissez un fournisseur CDN parmi les options proposées. Cliquez sur le bouton **Selected** en bas de l'écran pour valider votre choix et cliquez sur **Start Provision** pour démarrer le processus de mise à disposition.

	![Select CDN provider](images/newReducedSizeVendorSelectAndProvision.png)
	
5. Renseignez la zone **Configure Name** : 
      * Indiquez la valeur _hostname_ (**required**), qui constitue l'identificateur principal de votre CDN (par exemple, _example.testingcdn.net_).
      * Vous pouvez aussi fournir une valeur _CNAME_ personnalisée (telle que _myfirstcdn.cdnedge.bluemix.net_). Si aucune valeur CNAME n'est saisie, une valeur est créée pour vous. <validation information to be included here>
      
      ![Configure Name](images/configure-hostname-cname.png)
		
6. Renseignez la zone **Configure Your Origin** : Pour configurer cette zone, sélectionnez soit **Server**, soit **Object Storage**. (La zone **Path** est facultative. <validation information>)
		
  * Option **Server** : Si vous sélectionnez l'option **Server**, indiquez le nom d'hôte ou l'adresse IP du serveur d'origine à partir duquel les données sont mises en cache. 
      * Renseignez la zone **IP address** du serveur d'origine si vous avez sélectionné cette option.
      * Vous pouvez également spécifier les zones **HTTP port** et/ou **HTTPS port**. (Ces zones indiquent le protocole et le numéro de port à utiliser pour contacter le serveur d'origine.)

	   ![Configure origin server](images/configure-origin-server.png)
		
  * Option **Object Storage** : Si vous sélectionnez l'option **Object Storage**, fournissez les informations suivantes :
      * le noeud final (**Endpoint**) à partir duquel récupérer l'objet,
      * le nom du compartiment (**Bucket**) dans lequel est stocké le contenu, et
      * le port HTTPS (**HTTPS port**).
      * Vous pouvez également indiquer les extensions de fichiers, séparées par des virgules, susceptibles d'être utilisées dans le service CDN. (Si aucune extension n'est sélectionnée, toutes les extensions de fichiers sont admises.)
      * Définissez la liste de contrôle d'accès (**Access Control List**) de chaque **objet** dans votre **compartiment** sur "public-read".
		
	   ![Configure object storage](images/configure-origin-object-storage.png)

7. Configurez la zone **Other Options** : Cette section contient des options de configuration liées aux zones **Serve Stale Content** et **Respect Headers**.
    
     * **Serve Stale Content** : La zone **Serve Stale Content** est une valeur booléenne qui indique s'il faut distribuer du contenu en cache expiré si le serveur d'origine n'est pas joignable. Du fait d'une limitation actuelle, la fonctionnalité **Serve Stale Content** est activée par défaut, que l'utilisateur l'ait configurée sur **On** ou **Off**.
     * **Respect Headers** : Lorsque la zone **Respect Headers** est configurée sur **On**, les paramètres TTL définis dans l'en-tête par l'origine sont remplacés par le paramètre TTL CDN par défaut. La zone **Respect Headers** est définie sur **On** par défaut, mais vous devez configurer cette zone.

		![Other options](images/other-options.png)
		
8. Cliquez sur le bouton **Create** en bas à droite de l'écran pour créer votre CDN.
