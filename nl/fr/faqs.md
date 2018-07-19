---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# FAQ (Foire aux questions)

## Qu'est-ce-qu'un réseau de distribution de contenu (CDN)?

Un réseau de distribution de contenu (CDN - Content Delivery Network) est un ensemble de serveurs d'équilibrage des charges répartis dans différentes parties d'un pays ou du monde. Leur contenu Web est distribué à partir d'un serveur d'équilibrage des charges, situé dans la zone géographique la plus proche du client qui adresse la demande de contenu. Cette technique permet à l'utilisateur de recevoir le contenu plus rapidement qu'avec la technique de distribution de contenu à partir d'un emplacement centralisé. Elle garantit une expérience client optimale.

## Comment fonctionne un réseau de distribution de contenu (CDN) ?

L'objectif d'un CDN est de mettre en cache du contenu Web sur des serveurs d'équilibrage des charges dans le monde entier. Lorsqu'un utilisateur fait une demande de contenu Web, la demande est acheminée vers le serveur le plus proche. En réduisant la distance géographique, le réseau CDN offre un débit optimal, une latence réduite et de meilleures performances.

## Comment mon compte de service IBM Cloud Content Delivery Network est-il créé ?

Votre compte est créé lors du processus de commande du CDN, lorsque vous cliquez sur **Select** après avoir navigué dans le menu "Vendor selection".

## Que dois-je faire lorsque mon CDN est à l'état CNAME_CONFIGURATION ?

Dans les CDN basés sur HTTP, mettez à jour votre enregistrement DNS de sorte que votre site Web pointe vers le `CNAME` associé à votre nouveau mappage du CDN. Dans les CDN basés sur HTTPS, cette mise à jour du DNS n'est **PAS** nécessaire.

**Remarque** : L'activation de la mise à jour peut prendre jusqu'à 15-30 minutes. Contactez votre fournisseur DNS pour obtenir une évaluation de temps plus précise.

## Quels frais me seront facturés pour l'utilisation du CDN ?

Vous n'êtes facturé que pour la bande passante utilisée par l'instance IBM Cloud Content Delivery Network. Si votre CDN n'utilise pas de bande passante, aucun frais ne vous sera facturé. Le prix de la bande passante est variable en fonction de la zone géographique où se trouve le serveur d'équilibrage des charges. Vous pouvez voir le prix de la bande passante par région géographique dans la section [Mise en route](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-) de ce service.

## Quand serais-je facturé ?

La facturation d'IBM Cloud Content Delivery Network se fait en fonction de la période de facturation établie dans votre compte {{site.data.keyword.BluSoftlayer_notm}}.

## Si je sélectionne `delete` dans le menu déroulant dynamique du CDN, mon compte est-il supprimé ?

Non, votre compte CDN n'est pas supprimé. Il existe toujours et vous pouvez créer des CDN supplémentaires.

## La mise en cache utilise-t-elle la commande push ou pull ?

La mise en cache de contenu s'appuie sur un modèle _origin pull_. L'extraction d'origine (Origin Pull) est une méthode par laquelle les données sont "extraites" par le serveur d'équilibrage des charges à partir du serveur d'origine, contrairement au téléchargement manuel du contenu vers le serveur d'équilibrage des charges.

## Existe-t-il un navigateur recommandé pour la configuration du service CDN ?

Oui, Firefox et Chrome sont les navigateurs recommandés. Il est recommandé d'utiliser leurs dernières versions avec votre réseau de distribution de contenu IBM Cloud Content Delivery Network.

## Quel est le but de fournir un chemin d'accès lors de la création de mon CDN ?

Si vous fournissez un chemin d'accès lors de la création de votre CDN, celui-ci vous permet d'isoler les fichiers distribués via le CDN depuis un serveur d'origine particulier.

## Mon CDN indique un état erreur. Que dois-je faire maintenant ?

Consultez les pages [Traitement des incidents](troubleshooting.html#troubleshooting) ou [Aide et support](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#getting-help), ou ouvrez un ticket sur le [portail client![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/).

## Où puis-je trouver mon CNAME si je n'en ai pas fourni ?

Cliquez sur votre CDN pour accéder à la page de **présentation** du portail. Dans l'angle droit apparaît la section **Details** avec les informations relatives au `CName`.

## Ma demande de purge d'un chemin donné est en cours. Puis-je soumettre une nouvelle demande pour le même chemin ?

Non. Il ne peut y avoir qu'une seule demande de purge active à la fois pour un même chemin donné.

## Le protocole Internet version 6 (IPv6) est-il pris en charge par le service IBM Cloud Content Delivery Network ? Comment cela fonctionne-t-il ?

Le protocole IPv6 (ou prise en charge double pile) est compatible avec les serveurs d'équilibrage des charges d'Akamai. Il a été conçu pour aider les clients basés uniquement sur IPv4 à accepter les connexions provenant de clients IPv6, à convertir le protocole IPv6 en IPv4 sur le serveur d'équilibrage des charges et à les transmettre à l'origine par l'intermédiaire d'IPv4.

**Remarque :** La création d'un CDN IBM Cloud utilisant une adresse IPv6 en tant qu'adresse de serveur d'origine n'est pas prise en charge.

## Existe-t-il des restrictions concernant les numéros de port HTTP et HTTPS acceptés par Akamai ?

Oui. Pour le fournisseur Akamai, seuls les numéros de ports suivants sont autorisés :
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 et 45002.

## Quelle adresse URL dois-je utiliser pour accéder aux données du chemin d'origine ou du CDN ?
Le chemin d'accès au mappage CDN ou à l'origine est traité en tant que répertoire. Par conséquent, les utilisateurs qui tentent d'accéder au chemin d'origine doivent y accéder en tant que répertoire (en utilisant une barre oblique). Par exemple, si le CDN `www.example.com` est créé à l'aide du chemin qui inclut le répertoire `/images`, l'adresse URL permettant d'y accéder doit être `www.example.com/images/`

Si vous oubliez la barre oblique, en tapant par exemple `www.example.com/images`, le message d'erreur **Page Not Found** s'affiche.

## Comment puis-je configurer mon réseau de distribution de contenu pour le stockage d'objets IBM Cloud (COS) ?

[Voir le didacticiel](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sur la création d'un réseau de distribution de contenu (CDN) pour le stockage d'objets IBM Cloud.

## J'ai reçu une notification indiquant que mon certificat d'origine est arrivé à expiration. Que dois-je faire maintenant ?

Suivez les étapes décrites dans [cet article](https://community.akamai.com/docs/DOC-7708) d'Akamai.

## Quelles sécurités offre la solution IBM CDN avec Akamai ?

En utilisant la plateforme distribuée d'Akamai, vous bénéficiez d'une montée en charge et d'une résilience inégalées avec plus de 240 000 serveurs dans plus de 130 pays. La plateforme d'Akamai se situe entre votre infrastructure et vos utilisateurs finaux, et elle constitue le premier niveau de défense en cas d'augmentation soudaine du trafic. La plate-forme intelligente d'Akamai est également un proxy inversé qui écoute et répond aux requêtes sur les ports 80 et 443 uniquement, ce qui signifie que le trafic sur les autres ports est abandonné à la périphérie sans être transmis à votre infrastructure.
