---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

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

Vous n'êtes facturé que pour la bande passante utilisée par l'instance IBM Cloud Content Delivery Network. Si votre CDN n'utilise pas de bande passante, aucun frais ne vous sera facturé. Le prix de la bande passante est variable en fonction de la zone géographique où se trouve le serveur d'équilibrage des charges. Vous pouvez voir le prix de la bande passante par région géographique dans la section [Mise en route](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd) de ce service.

## Quand serais-je facturé ?

La facturation d'IBM Cloud Content Delivery Network se fait en fonction de la période de facturation établie dans votre compte {{site.data.keyword.BluSoftlayer_notm}}.

## Où puis-je consulter les valeurs de mesure et d'utilisation ?

Vous pouvez afficher les valeurs de mesure et d'utilisation sur la page de **présentation**, accessible en sélectionnant votre CDN à partir de la page **Content Delivery Networks**. **REMARQUE** : après avoir créé votre CDN, il peut s'écouler jusqu'à 48 heures avant que les mesures apparaissent.

## J'ai créé un CDN et il y avait un trafic de données à travers le CDN. Pourquoi mes mesures ne s'affichent-elles pas ?

Après la création d'un CDN, il faut attendre 48 heures pour que les mesures apparaissent.

## Existe-t-il un nombre minimum de jours pendant lesquels je peux afficher les mesures ? Existe-t-il un nombre maximum ?

Il existe un nombre minimum et un nombre maximum de jours pendant lesquels vous pouvez visualiser les mesures à l'aide d'IBM Cloud Content Delivery Network avec Akamai. Les mesures doivent être collectées pendant au moins 7 jours. Elles peuvent ensuite être visualisées pendant un maximum de 90 jours. Si vous utilisez l'API, il est recommandé de ne pas dépasser 89 jours afin de tenir compte des éventuels décalages horaires.

## Pourquoi le taux d'accès n'est-il pas zéro lorsque le nombre total de correspondances est égal à zéro ?
Le taux d'accès correspond au pourcentage de distribution du contenu à partir de la mémoire cache du serveur d'équilibrage des charges en lieu et place du serveur d'origine. Il est calculé comme suit :

```
((Edge hits - Ingress hits)/Edge hits) * 100

where: 
Edge hits is defined as "All hits to the edge servers from the end-users"
Ingress hits is defined as "Origin or Ingress hits are for traffic from your origin to Akamai edge servers"
```
 
Etant donné que le taux d'accès est calculé au niveau du compte et non pour chaque CDN, il sera le même pour tous les CDN de votre compte. Cette situation explique pourquoi le taux d'accès peut être une valeur différente de zéro lorsque le nombre d'accès au serveur d'équilibrage des charges d'un CDN particulier est égal à zéro.

## Si je sélectionne `delete` dans le menu déroulant dynamique du CDN, mon compte est-il supprimé ?

Non, votre compte CDN n'est pas supprimé. Il existe toujours et vous pouvez créer des CDN supplémentaires.

## La mise en cache utilise-t-elle la commande push ou pull ?

La mise en cache de contenu s'appuie sur un modèle _origin pull_. L'extraction d'origine (Origin Pull) est une méthode par laquelle les données sont "extraites" par le serveur d'équilibrage des charges à partir du serveur d'origine, contrairement au téléchargement manuel du contenu vers le serveur d'équilibrage des charges. 

## Existe-t-il une valeur de durée de vie maximale ? Minimale ?

La valeur maximale du paramètre Time To Live est 2 147 483 647 secondes, ce qui correspond à environ 68 ans ! La valeur minimale est de 30 secondes.

## Existe-t-il une limite au nombre d'entrées TTL et d'origine ?

Oui, la limite totale est de 75 entrées par CDN.

## Existe-t-il une limite quant au nombre de CDN que je peux posséder ?

Oui, vous ne pouvez pas posséder plus de 10 CDN par compte.

## Existe-t-il un navigateur recommandé pour la configuration du service CDN ?

Oui, Firefox et Chrome sont les navigateurs recommandés. Il est recommandé d'utiliser leurs dernières versions avec votre réseau de distribution de contenu IBM Cloud Content Delivery Network.

## Quel est le but de fournir un chemin d'accès lors de la création de mon CDN ?

Si vous fournissez un chemin d'accès lors de la création de votre CDN, celui-ci vous permet d'isoler les fichiers distribués via le CDN depuis un serveur d'origine particulier.

## Comment puis-je savoir si mon CDN fonctionne ?
Exécutez la commande 'curl' suivante en remplaçant "origin.cdntesting.net/assets/ibm_3d.gif" par le chemin d'accès aux fichiers respectif sur votre serveur d'origine : 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
Si la sortie de la commande correspond au format suivant, le CDN fonctionne normalement : 
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## Mon CDN indique un état erreur. Que dois-je faire maintenant ?
Consultez la page [Aide et support](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp), ou ouvrez un ticket sur le [portail client![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/).

## Où puis-je trouver mon CNAME si je n'en ai pas fourni ? 
Cliquez sur votre CDN pour accéder à la page de **présentation** du portail. Dans l'angle droit apparaît la section **Details** avec les informations relatives au `CName`.

## Existe-t-il une limite quant au nombre de demandes de purge pouvant être actives simultanément ?
Oui. 5 demandes de purge seulement peuvent être actives en même temps.

## Ma demande de purge d'un chemin donné est en cours. Puis-je soumettre une nouvelle demande pour le même chemin ?
Non. Il ne peut y avoir qu'une seule demande de purge active à la fois pour un même chemin donné.

## Le protocole Internet version 6 (IPv6) est-il pris en charge par le service IBM Cloud Content Delivery Network ? Comment cela fonctionne-t-il ? 
Le protocole IPv6 (ou prise en charge double pile) est compatible avec les serveurs d'équilibrage des charges d'Akamai. Il a été conçu pour aider les clients basés uniquement sur IPv4 à accepter les connexions provenant de clients IPv6, à convertir le protocole IPv6 en IPv4 sur le serveur d'équilibrage des charges et à les transmettre à l'origine par l'intermédiaire d'IPv4.

**REMARQUE :** La création d'un CDN IBM Cloud utilisant une adresse IPv6 en tant qu'adresse de serveur d'origine n'est pas prise en charge.

## Quelles sont les règles applicables au nom d'hôte ?
La chaîne d'entrée `Hostname` **doit** : 
  * être composée de caractères alphanumériques
  * comprendre moins de 254 caractères
  * se terminer par un nom de domaine de niveau supérieur valide
  * ne doit **pas** se terminer par 'cdnedge.bluemix.net` (cette terminaison est réservée à CNAMES)

Pour plus de détails, veuillez vous référer à la RFC 1035, section 2.3.4.

## Que sont les conventions de dénomination CNAME personnalisées ?
La chaîne d'entrée `CNAME` doit être conforme aux règles suivantes :
  * elle **doit** être unique (elle ne peut pas être utilisée par un autre service IBM Cloud CDN)
  * elle doit contenir moins de 64 caractères alphanumériques
  * elle ne doit **pas** comporter les caractères spéciaux `! @ # $ % ^ & *`
  * elle ne doit **pas** comporter des traits de soulignement (`_`)
  * elle ne doit **pas** comporter de point (`.`)
  * Les tirets (`-`) sont autorisés, mais le CNAME ne peut pas commencer ou se terminer par un tiret

## Quelles sont les règles applicables aux noms de compartiment ? 
Les noms de compartiment :
  * doivent comporter un minimum de 1 caractère
  * ne peuvent pas comprendre plus de 199 caractères
  * peuvent contenir des lettres (les lettres majuscules et minuscules sont autorisées), des chiffres et des traits d'union
  * ne doivent **pas** être formatés en tant qu'adresse IP (par exemple, 127.0.0.1)
  * ne peuvent **pas** commencer par un point (.)
  * peuvent se terminer par un point (.)
  * Un seul point (.) est autorisé entre les libellés (par exemple, my..ibmcloud.bucket n'est pas autorisé). 

**REMARQUE** : bien que les lettres en majuscules et les points puissent être validés, nous recommandons de toujours utiliser des noms de compartiment conformes au DNS.

## Quelles sont les règles applicables à la chaîne de chemin d'accès pour l'origine ?
Le chemin d'accès est facultatif lorsque vous créez votre CDN. Cependant, s'il est fourni, le chemin d'accès **doit** :
  * avoir une longueur de moins de 1000 caractères
  * commencer par '/'

## Dans le cadre de la commande **Add Origin**, quelles sont les règles à suivre pour spécifier la chaîne de chemin d'accès ?
Le chemin est obligatoire avec **Add Origin**. De même, si votre CDN a été créé avec un chemin d'accès, le chemin d'accès spécifié sous **Add Origin** doit commencer par le chemin du CDN comme préfixe. Par exemple, si le chemin du CDN a été spécifié sous la forme `/storage` et que vous souhaitez appeler la commande **Add Origin** avec un chemin appelé `/examplePath`, le chemin fourni doit être `/storage/examplePath`'.

## Dans quel format dois-je fournir les extensions de fichiers pour créer un type d'origine de stockage d'objet avec ce service CDN ?

Les extensions de fichiers doivent être séparées par des virgules. Par exemple, la liste "txt, jpg, xml" est valide. Les extensions particulières ne doivent en aucun cas dépasser 10 caractères.

## Existe-t-il des restrictions concernant les numéros de port HTTP et HTTPS acceptés par Akamai ?
Oui. Pour le fournisseur Akamai, seuls les numéros de ports suivants sont autorisés :
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 et 45002.

## Quelle adresse URL dois-je utiliser pour accéder aux données du chemin d'origine ou du CDN ? 
Le chemin d'accès au mappage CDN ou à l'origine est traité en tant que répertoire. Par conséquent, les utilisateurs qui tentent d'accéder au chemin d'origine doivent y accéder en tant que répertoire (en utilisant une barre oblique). Par exemple, si le CDN `www.example.com` est créé à l'aide du chemin qui inclut le répertoire `/images`, l'adresse URL permettant d'y accéder doit être `www.example.com/images/`

Si vous oubliez la barre oblique, en tapant par exemple `www.example.com/images`, le message d'erreur **Page Not Found** s'affiche.

## Sous HTTPS, pourquoi ne puis-je pas me connecter via une commande curl ou un navigateur à l'aide de Hostname ?

A l'heure actuelle, le protocole HTTPS est pris en charge uniquement via un certificat générique. Par conséquent, la connexion doit être établie à l'aide de CNAME et non de Hostname, faute de quoi une erreur se produit.

## Que se passe-t-il lorsque je charge le CNAME ou le nom d'hôte sur mon navigateur avec les protocoles CDN pris en charge ?

|URL navigateur| CDN avec protocole HTTP | CDN avec protocole HTTPS | CDN avec protocoles HTTP et HTTPS |
|-------|-----|-----|-----|
|http://hostname| Chargement réussi | Erreur 301, redirection permanente | Erreur 301, redirection permanente |
|https://hostname | Accès refusé | Redirection à la page Web d'IBM Cloud | Redirection à la page Web d'IBM Cloud | 
|http://cname| Erreur 301, redirection permanente | Accès refusé | Chargement réussi | 
|https://cname| Redirection à la page Web d'IBM Cloud | Chargement réussi | Chargement réussi |

## Comment puis-je configurer mon réseau de distribution de contenu pour le stockage d'objets IBM Cloud (COS) ?
[Voir le didacticiel](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sur la création d'un réseau de distribution de contenu (CDN) pour le stockage d'objets IBM Cloud.

## Pourquoi mon nom d'hôte ne se charge-t-il pas sur le navigateur lorsque je sélectionne IBM Cloud Object Storage (COS) comme origine ?

Lorsque votre réseau de distribution de contenu IBM Cloud CDN est configuré pour utiliser IBM COS comme outil de stockage d'objet, l'accès direct au site Web ne fonctionne pas. Vous devez spécifier le chemin de requête complet dans la barre d'adresse du navigateur (par exemple, `www.example.com/index.html`). Ce comportement est dû à une limitation du document d'index dans IBM COS.

## Quelle est la plus grande taille de fichier pouvant être livrée via le CDN Akamai ?

Les tentatives d'envoi ou de livraison de fichiers supérieurs à 1,8 Go recevront une réponse `403 Access Forbidden` si la configuration de performance par défaut est utilisée. Si l'optimisation des fichiers volumineux est activée, les téléchargements de fichiers allant jusqu'à 320 Go sont possibles. 

## J'ai reçu une notification indiquant que mon certificat d'origine est arrivé à expiration. Que dois-je faire maintenant ?

Suivez les étapes décrites dans [cet article](https://community.akamai.com/docs/DOC-7708) d'Akamai.

## Qu'est-ce qu'une demande de plage d'octets et comment fonctionne-t-elle avec Akamai CDN ?

Une demande de plage d'octets est utilisée pour extraire des contenus partiels d'un serveur d'origine. L'en-tête de plage de la demande HTTP indique quelle partie du contenu doit être renvoyée par le serveur. Un en-tête de plage permet de demander plusieurs parties en même temps et le serveur peut renvoyer ces plages dans une réponse multipartie. Si le serveur renvoie des plages, il répond avec un statut 206 (contenu partiel).

Lorsqu'une demande de plage d'octets est envoyée via IBM Cloud CDN avec Akamai, l'utilisateur peut recevoir un code de réponse 200 (OK) pour la première demande et un code de réponse 206 pour toutes les demandes suivantes. Ceci est dû au fait que les serveurs d'équilibrage des charges demandent les contenus du serveur d'origine dans un format compressé. Ainsi, lorsqu'un serveur d'équilibrage des charges n'a pas d'objet dans son cache ni d'information sur la longueur du contenu de l'objet, il ira jusqu'à l'origine et demandera l'objet entier. Dans ce cas, l'origine sert l'objet sans l'en-tête de longueur de contenu à Akamai et l'utilisateur final reçoit l'objet entier même s'il s'agit d'une demande de plage d'octets. Ceci explique le code de statut 200. Lors des demandes suivantes, le serveur d'équilibrage des charges contient l'objet dans son cache et renvoie le code d'état 206.

Une façon de garantir une réponse 206, même pour la première demande de plage d'octets est de désactiver `Transfer-Encoding: chunked` sur votre serveur d'origine.

## Quelles sécurités offre la solution IBM CDN avec Akamai ?

En utilisant la plateforme distribuée d'Akamai, vous bénéficiez d'une montée en charge et d'une résilience inégalées avec plus de 240 000 serveurs dans plus de 130 pays. La plateforme d'Akamai se situe entre votre infrastructure et vos utilisateurs finaux, et elle constitue le premier niveau de défense en cas d'augmentation soudaine du trafic. La plate-forme intelligente d'Akamai est également un proxy inversé qui écoute et répond aux requêtes sur les ports 80 et 443 uniquement, ce qui signifie que le trafic sur les autres ports est abandonné à la périphérie sans être transmis à votre infrastructure.
