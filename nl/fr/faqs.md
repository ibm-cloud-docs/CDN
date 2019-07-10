---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# FAQ (Foire aux questions)
{: #faqs}

## Qu'est-ce-qu'un réseau de distribution de contenu (CDN)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Un réseau de distribution de contenu (CDN - Content Delivery Network) est un ensemble de serveurs d'équilibrage des charges répartis dans différentes parties d'un pays ou du monde. Leur contenu Web est distribué à partir d'un serveur d'équilibrage des charges, situé dans la zone géographique la plus proche du client qui adresse la demande de contenu. Cette technique permet à l'utilisateur de recevoir le contenu plus rapidement qu'avec la technique de distribution de contenu à partir d'un emplacement centralisé. Elle garantit une expérience client optimale.

## Comment fonctionne un réseau de distribution de contenu (CDN) ?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

L'objectif d'un CDN est de mettre en cache du contenu Web sur des serveurs d'équilibrage des charges dans le monde entier. Lorsqu'un utilisateur fait une demande de contenu Web, la demande est acheminée vers le serveur le plus proche. En réduisant la distance géographique, le réseau CDN offre un débit optimal, une latence réduite et de meilleures performances.

## Comment mon compte de service IBM Cloud Content Delivery Network est-il créé ?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

Votre compte est créé lors du processus de commande du CDN. Si vous créez un CDN à partir du portail existant, lorsque vous cliquez sur le bouton **Commander CDN**, sous la page **Réseau -> Réseau de diffusion de contenu (CDN)**, votre compte est créé. Si vous créez un CDN à partir du portail {{site.data.keyword.cloud}}, lorsque vous cliquez sur le bouton **Créer** sous la page **Catalogue -> Réseau -> Réseau de diffusion de contenu (CDN)**, votre compte est créé.

## Que dois-je faire lorsque mon CDN a pour statut CNAME Configuration ?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

Pour les CDN HTTP et HTTPS basés sur les certificats SAN, mettez à jour votre enregistrement DNS de sorte que votre site Web pointe vers le `CNAME` associé à votre nouveau mappage de CDN. Pour les CDN HTTPS basés sur les certificats génériques, la mise à jour de cet enregistrement DNS n'est **PAS** nécessaire.

**Remarque** : L'activation de la mise à jour peut prendre jusqu'à 15-30 minutes. Contactez votre fournisseur DNS pour obtenir une évaluation de temps plus précise.

## Comment puis-je ajouter dans DNS un enregistrement CNAME pour mon domaine CDN ?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

Dans votre page de configuration DNS pour votre domaine CDN, vous pouvez créer un enregistrement CNAME avec pour nom de domaine CDN l'hôte et le CNAME IBM utilisés pour configurer le CDN, en tant que CNAME. Le IBM CNAME se termine par `cdnedge.bluemix.net`.

Un enregistrement CNAME standard se présente comme suit sur la page de configuration de DNS :

| **Type de ressource** | **Hôte** | **Pointe vers (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutes |


## Quels frais me seront facturés pour mon CDN ?
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

Vous n'êtes facturé que pour la bande passante utilisée par l'instance IBM Cloud Content Delivery Network. Si votre CDN n'utilise pas de bande passante, aucun frais ne vous sera facturé. Le prix de la bande passante est variable en fonction de la zone géographique où se trouve le serveur d'équilibrage des charges. Vous pouvez voir le prix de la bande passante par région géographique dans le [document de tarification](/docs/infrastructure/CDN?topic=CDN-pricing) de ce service.

## Quand serais-je facturé pour mon CDN ?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

La facturation d'IBM Cloud Content Delivery Network se fait en fonction de la période de facturation établie dans votre compte {{site.data.keyword.BluSoftlayer_notm}}.

## Si je sélectionne `delete` dans le menu déroulant dynamique du CDN, mon compte est-il supprimé ?
{: faq}

Non, votre compte CDN n'est pas supprimé. Il existe toujours et vous pouvez créer des CDN supplémentaires.

## La mise en cache de contenu utilise-t-elle la commande push ou pull ?
{: #does-content-caching-use-push-or-pull}
{: faq}

La mise en cache de contenu s'appuie sur un modèle _origin pull_. L'extraction d'origine (Origin Pull) est une méthode par laquelle les données sont "extraites" par le serveur d'équilibrage des charges à partir du serveur d'origine, contrairement au téléchargement manuel du contenu vers le serveur d'équilibrage des charges.

## Existe-t-il un navigateur recommandé pour la configuration du service CDN ?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Oui, Firefox et Chrome sont les navigateurs recommandés. Il est recommandé d'utiliser leurs dernières versions avec votre réseau de distribution de contenu IBM Cloud Content Delivery Network.

## Quel est le but de fournir un chemin d'accès lors de la création de mon CDN ?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

Si vous fournissez un chemin d'accès lors de la création de votre CDN, celui-ci vous permet d'isoler les fichiers distribués via le CDN depuis un serveur d'origine particulier.

## Mon CDN indique un état erreur. Que dois-je faire maintenant ?
{: faq}

Consultez les pages [Traitement des incidents](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) ou [Aide et support](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp), ou ouvrez un ticket sur le [portail client![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/).

## Où puis-je trouver le CNAME de mon CDN si je n'en ai pas fourni ?
{: faq}

Cliquez sur votre CDN pour accéder à la page de **présentation** du portail. Dans l'angle droit apparaît la section **Details** avec les informations relatives au `CName`.

## Ma demande de purge d'un chemin donné est en cours. Puis-je soumettre une nouvelle demande pour le même chemin ?

Non. Il ne peut y avoir qu'une seule demande de purge active à la fois pour un même chemin donné.

## Le protocole Internet version 6 (IPv6) est-il pris en charge par le service IBM Cloud Content Delivery Network ? Comment cela fonctionne-t-il ?
{: faq}

Le protocole IPv6 (ou prise en charge double pile) est compatible avec les serveurs d'équilibrage des charges d'Akamai. Il a été conçu pour aider les clients basés uniquement sur IPv4 à accepter les connexions provenant de clients IPv6, à convertir le protocole IPv6 en IPv4 sur le serveur d'équilibrage des charges et à les transmettre à l'origine par l'intermédiaire d'IPv4.

**Remarque :** La création d'un CDN IBM Cloud utilisant une adresse IPv6 en tant qu'adresse de serveur d'origine n'est pas prise en charge.

## Existe-t-il des restrictions concernant les numéros de port HTTP et HTTPS acceptés par Akamai ?
{: faq}

Oui. Pour le fournisseur Akamai, seuls les numéros de ports suivants sont autorisés :
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 et 45002.

## Quelle adresse URL dois-je utiliser pour accéder aux données du chemin d'origine ou du CDN ?
{: faq}

Le chemin d'accès au mappage CDN ou à l'origine est traité en tant que répertoire. Par conséquent, les utilisateurs qui tentent d'accéder au chemin d'origine doivent y accéder en tant que répertoire (en utilisant une barre oblique). Par exemple, si le CDN `www.example.com` est créé à l'aide du chemin qui inclut le répertoire `/images`, l'adresse URL permettant d'y accéder doit être `www.example.com/images/`

Si vous oubliez la barre oblique, en tapant par exemple `www.example.com/images`, le message d'erreur **Page Not Found** s'affiche.

## Comment puis-je configurer mon réseau de distribution de contenu pour le stockage d'objets IBM Cloud (COS) ?
{: faq}

[Voir le didacticiel](/docs/tutorials?topic=solution-tutorials-static-files-cdn) sur la création d'un réseau de distribution de contenu (CDN) pour le stockage d'objets IBM Cloud.

## J'ai reçu une notification indiquant que mon certificat d'origine est arrivé à expiration. Que dois-je faire maintenant ?
{: faq}

Suivez les étapes décrites dans [cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://community.akamai.com/docs/DOC-7708) d'Akamai.

## Quelle sécurité offre la solution IBM CDN avec Akamai ?
{: faq}

En utilisant la plateforme distribuée d'Akamai, vous bénéficiez d'une montée en charge et d'une résilience inégalées avec des milliers de serveurs dans plus de 50 pays. La plateforme d'Akamai se situe entre votre infrastructure et vos utilisateurs finaux, et elle constitue le premier niveau de défense en cas d'augmentation soudaine du trafic. La plate-forme intelligente d'Akamai est également un proxy inversé qui écoute et répond aux requêtes sur les ports 80 et 443 uniquement, ce qui signifie que le trafic sur les autres ports est abandonné à la périphérie sans être transmis à votre infrastructure.

## Les cookies du serveur d'origine sont-ils conservés par le CDN Akamai ?
{: faq}

Pour un contenu qui ne peut pas être mis en cache ou pour n'importe quel contenu non mis en cache, les cookies du serveur d'origine sont conservés. Pour le contenu qui est mis en cache par des serveurs d'équilibrage des charges, les cookies ne sont pas conservés.

## Comment puis-je utiliser la console IBM Cloud pour autoriser d'autres utilisateurs à créer ou gérer un CDN ?
{: faq}

L'utilisateur principal du compte peut fournir aux autres utilisateurs les droits leur permettant de créer et gérer un CDN.

Sur la page principale de la console IBM Cloud, procédez comme suit pour éditer des droits :
 * Sélectionnez l'onglet **Gérer**
 * Sélectionnez **Accès (IAM)**
 * Cliquez sur l'onglet **Utilisateurs** dans le panneau de gauche
 * Cliquez sur l'**Utilisateur** concerné
 * Puis sélectionnez l'onglet **Infrastructure classique**
 * Ensuite, sous l'onglet **Droits**, développez la catégorie **Services**
 * Sélectionnez **Gérer un compte CDN**.
 * Cliquez sur le bouton **Sauvegarder**

Sur la page principale de la console existante, procédez comme suit pour éditer des droits :
 * Sélectionnez l'onglet **Compte**.
 * Sélectionnez **Utilisateurs -> Liste d'utilisateurs**.
 * Cliquez sur le **nom d'utilisateur** souhaité.
 * Sélectionnez l'onglet**Autorisations du portail**.
 * Sélectionnez l'onglet **Services**.
 * Sélectionnez **Gérer un compte CDN**.
 * Cliquez sur le bouton **Modifier droits pour le portail**.

## Pourquoi le bouton Créer est-il absent ou désactivé sur la page https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai ?
{: faq}

Si vous êtes l'utilisateur principal du compte, vous devez mettre à niveau le compte pour que le bouton Créer s'affiche ou soit activé sur cette page. Sur la page de la console IBM Cloud, effectuez les opérations suivantes en tant qu'utilisateur principal du compte :
 * Ouvrez le panneau de navigation de gauche en cliquant sur l'icône `triple barre` dans l'angle supérieur gauche de la page Web
 * Sélectionnez **Infrastructure classique**
 * Cliquez sur le bouton **Mise à niveau du compte** et suivez les instructions

Si vous êtes l'un des utilisateurs secondaires du compte, l'utilisateur principal doit vous accorder le droit `Ajouter/mettre à niveau des services` pour que le bouton Créer s'affiche ou soit activé sur cet page. Sur la page de la console IBM Cloud, l'utilisateur principal du compte doit effectuer les opérations suivantes pour éditer vos droits :
 * Sélectionner l'onglet **Gérer**
 * Sélectionner **Accès (IAM)**
 * Cliquer sur l'onglet **Utilisateurs** dans le panneau de gauche
 * Cliquer sur l'**Utilisateur** concerné
 * Puis sélectionner l'onglet **Infrastructure classique**
 * Ensuite, sous l'onglet **Droits**, développer la catégorie **Account**
 * Sélectionner **Ajouter/mettre à niveau des services**
 * Cliquer sur le bouton **Sauvegarder**

## Pourquoi ne puis-je pas accéder à ma page Web via mon CDN après avoir configuré la protection des liens dynamiques avec `protectionType` `ALLOW` ?
{: faq}

Prenons un exemple dans lequel le domaine de votre site Web pour les utilisateurs finaux est configuré avec le nom de domaine/d'hôte de votre CDN : `cdn.example.com`. Lorsqu'un utilisateur tente d'accéder à une page Web par navigation directe depuis la barre de navigation du navigateur, ce dernier n'envoie généralement pas d'en-tête Referer dans sa requête HTTP. Ainsi, lorsque vous naviguer directement de cette manière jusqu'à `https://cdn.example.com/`, votre CDN considère que la requête ne contient pas de concordances avec les valeurs `refererValues` spécifiées. Lorsque le CDN évalue la réponse ou l'effet approprié via votre protection des liens dynamiques, il détermine qu'une non-concordance s'est produite. Par conséquent, votre CDN refusera l'accès, au lieu de l'autoriser (ALLOW).

## Puis-je utiliser un noeud final privé du stockage d'objets dans les paramètres de CDN ?
{: faq}

Non, un CDN peut uniquement se connecter à Object Storage sur des noeuds finaux publics.

## Puis-je utiliser la fonctionnalité Brotli dans le service CDN ?
{: faq}

Non, la fonctionnalité Brotli n'est pas prise en charge avec notre service CDN associé à Akamai.

## Comment créer un noeud final de CDN sans utiliser le domaine ?
{: faq}

Vous pouvez créer un noeud final de CDN sans utiliser le domaine, mais UNIQUEMENT pour un CDN de type **HTTPS de caractère générique**. Lors de la création d'un CDN de ce type, votre **CNAME** agit en tant que noeud final du CDN et le **CNAME** est utilisé pour le trafic.
