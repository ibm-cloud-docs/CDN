---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Commande d'un CDN
{: #order-a-cdn}

La présente section vous explique comment commander un réseau de distribution de contenu (CDN). Votre CDN peut être commandé à partir de la [console {{site.data.keyword.cloud}}](https://cloud.ibm.com/login).

## Préparation à la commande
{: #preparation-for-ordering}

La procédure de navigation dans la page CDN pour passer votre commande est décrite ci-après.

**Etape 1**

* Connectez-vous à votre compte à partir de la [console IBM Cloud](https://cloud.ibm.com/login)

**Etape 2**

Cliquez sur [Catalogue IBM Cloud](https://cloud.ibm.com/catalog/). Dans la barre de navigation située dans la partie gauche de l'écran, sélectionnez **Network**.

   ![CDN Navigation IBM Cloud CDN](images/bluemix_navigation.png)

**Etape 3**

Cliquez sur **CDN Tile**.

   ![Icône IBM Cloud CDN](images/bluemix_tile.png)


## Commande d'un nouveau CDN
{: #order-a-new-cdn}

Une fois que vous avez atteint la page de commande, procédez comme suit pour créer et configurer votre CDN.

### Etape 1 : Créez votre compte CDN
{: #create-your-cdn-account}

Cliquez sur **Create** en bas à droite afin de créer un compte CDN si vous n'en possédez pas et d'être redirigé vers l'écran CDN Configure.

   ![Présentation de CDN](images/content-delivery.png)

### Etape 2 : Nommez votre CDN
{: #step-2-name-your-cdn}

Renseignez la zone **Configure Name** :  

  * Spécifiez la zone **Hostname** (zone **obligatoire**), qui sert d'identificateur principal pour votre CDN (par exemple, `example.testingcdn.net`).  
  * Vous pouvez aussi fournir une valeur **CNAME** personnalisée (telle que `myfirstcdn.cdnedge.bluemix.net`). Si aucune valeur CNAME n'est saisie, une valeur est créée pour vous. Le suffixe `cdnedge.bluemix.net` est automatiquement ajouté à l'enregistrement CNAME. L'utilisation d'un enregistrement CNAME inapproprié peut provoquer l'arrêt des services.

       ![Configure Name](images/configure-hostname-cname.png)  

Une fois que vous avez commandé un CDN, vous **devez** configurer le CNAME avec votre fournisseur DNS.
{: note}
### Etape 3 : Configurez votre origine
{: #step-3-configure-your-origin}

Renseignez la zone **Configure Your Origin** : Pour configurer cette zone, sélectionnez soit **Server**, soit **Object Storage**.  

  * **Etape 3, Option 1 : Option Server**

     ![Configure Origin](images/configure-origin-server.png)

      * Vous devez renseigner la zone **Origin Server Address** (nom d'hôte ou adresse IPv4 du serveur d'origine). Si l'option **HTTPS port** est également sélectionné, la zone **Origin Server Address** doit contenir le nom d'hôte et non l'adresse IP.

      * Renseignez la zone **Host header** (facultatif). Si aucun en-tête d'hôte n'est fourni, la valeur de **Hostname** sera utilisée par défaut. Pour en savoir plus sur l'en-tête d'hôte, consultez la description de la fonction de [prise en charge des en-têtes d'hôte](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support).  

      * Renseignez la zone **Path** avec une valeur permettant l'extraction du contenu à partir du serveur d'origine (facultatif). Reportez-vous à la description de la fonction pour les [mappages de CDN basés sur les chemins](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) afin de comprendre les implications de l'ajout d'un chemin à ce stade.

      * Vous pouvez également spécifier les zones **HTTP port** et/ou **HTTPS port**. Ces zones indiquent quels protocoles et numéros de port peuvent être utilisés pour contacter le serveur d'origine. Pour utiliser des numéros de port qui ne sont pas ceux définis par défaut, consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour obtenir la liste des numéros de port autorisés.

      * **SSL Certificate** : cette option apparaît _uniquement_ lorsque l'option HTTPS Port est sélectionnée. Si vous sélectionnez **HTTPS Port** pour l'option Server ou Object Storage, vous pouvez choisir **Wildcard** ou **DV SAN Certificate** comme valeur pour l'option SSL Certificate. Ces deux choix offrent la sécurité renforcée fournie par HTTPS.
        * **Wildcard Certificate** autorise le trafic HTTPS uniquement en cas d'utilisation de **CNAME** et ne nécessite aucune autre action de votre part.
        * **DV SAN Certificate** permet un trafic HTTPS sur votre domaine mais requiert des étapes supplémentaires de vérification. Pour comprendre les étapes requises et les contraintes de temps liées au choix de cette option, voir la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https).

	     ![Configure origin server](images/ssl-cert-options.png)

  * **Etape 3, Option 2 : Option Object Storage**

    ![Configure Object storage](images/configure-origin-object-storage.png)

      * Vous **devez** spécifier le **noeud final** à partir duquel extraire l'objet.

      * Renseignez la zone **Host header** (facultatif). Si aucun en-tête d'hôte n'est fourni, la valeur de **Hostname** sera utilisée par défaut. Pour en savoir plus sur l'en-tête d'hôte, consultez la description de la fonction de [prise en charge des en-têtes d'hôte](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support).  

      * Renseignez la zone **Path** avec une valeur permettant l'extraction du contenu à partir du serveur d'origine (facultatif). Reportez-vous à la description de la fonction pour les [mappages de CDN basés sur les chemins](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) afin de comprendre les implications de l'ajout d'un chemin à ce stade.

      * Vous **devez** fournir le nom du **compartiment** dans lequel votre contenu est stocké.

      * Vous pouvez également spécifier les zones **HTTP port** et/ou **HTTPS port**. Ces zones indiquent quels protocoles et numéros de port peuvent être utilisés pour contacter le serveur d'origine. Pour utiliser des numéros de port qui ne sont pas ceux définis par défaut, consultez la [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour obtenir la liste des numéros de port autorisés.

      * **SSL Certificate** : cette option apparaît _uniquement_ lorsque l'option HTTPS Port est sélectionnée. Si vous sélectionnez **HTTPS Port** pour l'option Server ou Object Storage, vous pouvez choisir **Wildcard** ou **DV SAN Certificate** comme valeur pour l'option SSL Certificate. Ces deux choix offrent la sécurité renforcée fournie par HTTPS.
        * **Wildcard Certificate** autorise le trafic HTTPS uniquement en cas d'utilisation de **CNAME** et ne nécessite aucune autre action de votre part.
        * **DV SAN Certificate** permet un trafic HTTPS sur votre domaine mais requiert des étapes supplémentaires de vérification. Pour comprendre les étapes requises et les contraintes de temps liées au choix de cette option, voir la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https).

        ![Configure HTTPS](images/ssl-cert-options.png)

Vous devez affecter à la liste de contrôle d'accès (**Access Control List** - ACL) de chaque objet de votre compartiment la valeur "public-read" avec votre fournisseur de stockage d'objets Cloud.
{: note}
      
### Etape 4
{: #step-4}

* Vous devez cocher la case **I have read the Master Service Agreement and agree to the terms therein** en bas à droite, au-dessus du bouton **Créer**.

* Puis cliquez sur le bouton **Créer** dans l'angle inférieur droit pour créer votre CDN.
