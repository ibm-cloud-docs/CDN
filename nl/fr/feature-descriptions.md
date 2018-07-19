---

copyright:
  years: 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Description des fonctions

Cette page présente beaucoup des fonctions incluses avec IBM Cloud CDN optimisé par Akamai.

## Prise en charge de l'origine du serveur hôte

IBM Cloud Content Delivery Network (CDN) peut être configuré pour traiter les contenus d'une origine de serveur hôte en fournissant le nom d'hôte de l'origine, le protocole, le numéro de port et en option, le chemin à partir duquel le contenu doit être servi. Le chemin par défaut est `/*`. Le protocole peut être HTTP, HTTPS, ou les deux. Seuls certains numéros de ports sont pris en charge par Akamai. Consultez la [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports et les plages pris en charge.

## Prise en charge de l'origine du stockage d'objet

IBM Cloud CDN peut être configuré pour servir le contenu provenant d'un noeud final de stockage d'objet en fournissant le noeud final, le nom de compartiment, le protocole et le port.Facultativement, une liste des extensions de fichiers peut être spécifiée pour autoriser uniquement la mise en cache des fichiers possédant ces extensions. Tous les objets du compartiment doivent être définis avec un accès en lecture anonyme ou public.

Pour obtenir des informations complémentaires, consultez le tutoriel [Accelerate delivery of static files using a CDN](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn).

## Prise en charge de plusieurs origines avec différents chemins

Dans certains cas, il est possible que vous souhaitiez livrer certains contenus depuis un serveur d'origine différent. Par exemple, vous souhaitez que certaines photos ou vidéos soient servies à partir de serveurs d'origine différents. IBM Cloud CDN offre la possibilité de configurer plusieurs serveurs d'origine avec plusieurs chemins d'accès. Ceci offre une certaine flexibilité quant au mode et au lieu de stockage des données. Le chemin fourni pour le serveur d'origine doit être unique par rapport au CDN. Le serveur d'origine lui-même n'a pas besoin d'être unique.

## Mappages de CDN basés sur des chemins

Votre service IBM Cloud CDN peut être restreint à un chemin de répertoire spécifique sur le serveur d'origine si vous fournissez le chemin lors de la création du CDN. Un utilisateur final est uniquement autorisé à accéder aux contenus se trouvant dans ce chemin de répertoire. Ainsi, si un CDN `www.example.com` est créé avec le chemin `/videos`, il est uniquement accessible via `www.example.com/videos/`.

## Purge des contenus mis en cache

IBM Cloud CDN offre la possibilité de supprimer ou de "purger" le contenu mis en cache des serveurs d'équilibrage des charges, et ce de manière facile et rapide. A l'heure actuelle, la purge n'est possible que pour un chemin de fichier. L'implémentation de la purge avec Akamai purge actuellement le fichier en environ 5 secondes. L'interface utilisateur offre également la possibilité d'afficher votre historique de purge et d'ajouter des chemins de purge spécifiques à vos favoris.

## Durée de vie (TTL)

La "durée de vie" indique la durée (en secondes) pendant laquelle le serveur d'équilibrage des charges mettra en cache le contenu d'un fichier ou d'un chemin de répertoire particulier. Quand un CDN est créé pour la première fois, une durée de vie globale (TTL) est créée pour le chemin `/\*` avec une durée par défaut de 3600 secondes. La valeur minimale de TTL est de 30 secondes et la valeur maximale de 2147483647 secondes. Pour chaque entrée, le chemin TTL doit être unique pour le CDN. Si plusieurs chemins correspondent à un contenu donné, le chemin qui aura été configuré en dernier sera appliqué pour ce contenu. Prenons l'exemple de deux TTL : `/example/file` est créé en premier avec une valeur de durée de vie de 3000 secondes et `/example/*` est créé par la suite avec une valeur de durée de vie de 4000 secondes. Bien que `/example/file` soit plus spécifique, `/example/*` a été créé en dernier, et par conséquent la durée de vie de `/example/file` sera de 4000 secondes. Une fois créées, les entrées TTL peuvent être éditées (chemin et/ou durée). Elles peuvent également être supprimées.

## Mesures avec vues graphiques

Les mesures d'un CDN particulier sont fournies sur l'onglet Présentation du portail client pour ce mappage de CDN. Il existe deux types de mesures calculés à partir de l'utilisation du CDN : celles qui affichent les changements enregistrés au cours d'une période sous forme de graphique et celles qui sont affichées sous forme de valeurs agrégées.

Les mesures qui affichent les changements relatifs à une période sous forme de graphique offrent trois diagrammes linéaires et un graphique circulaire. Les trois diagrammes linéaires sont : **Bandwidth**, **Hits by Mapping** et **Hits by Type**. Ils affichent l'activité de manière quotidienne pendant toute la durée de l'intervalle spécifié. Les diagrammes **Bandwidth** et **Hits by Mapping** sont des diagrammes à une seule ligne, alors que la répartition **Hits by Type** affiche une ligne pour chaque type d'accès fourni. Le graphique circulaire affiche une répartition par région de la bande passante d'un mappage de CDN sous forme de pourcentage.

Les mesures affichées sous forme de valeurs agrégées incluent **l'utilisation de la bande passante** en Go, le **nombre d'accès total** au serveur d'équilibrage des charges du CDN et le **taux d'accès**. Le taux d'accès indique le nombre de fois en pourcentage où le contenu est livré par le serveur d'équilibrage des charges, et _non_ par son origine. Actuellement, le taux d'accès est affiché comme une fonction de tous vos mappages de CDN et pas seulement du mappage en cours d'affichage.

Par défaut, les numéros agrégés et les graphiques affichent les mesures des 30 derniers jours, mais vous pouvez modifier cette configuration dans le [portail client![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/). Les deux catégories peuvent afficher des mesures sur des périodes de 7, 15, 30, 60 ou 90 jours.

## Prise en charge des en-têtes d'hôte

Le serveur d'équilibrage des charges utilise l'en-tête **Host Header**  lors de la communication avec l'hôte d'origine. Cette fonction offre de la flexibilité quant à la manière dont le service Web est configuré sur l'hôte d'origine. Si l'entrée de l'en-tête d'hôte n'est pas fournie, le service utilise le nom d'hôte du serveur d'origine en tant qu'en-tête d'hôte HTTP par défaut si le serveur d'origine est spécifié en tant que nom d'hôte (et non en tant qu'adresse IP). Si l'en-tête d'hôte n'est pas fourni et le serveur d'origine est fourni sous forme d'adresse IP, le nom d'hôte du CDN (également appelé le nom de domaine du CDN) est utilisé en tant qu'en-tête d'hôte HTTP par défaut.

## Prise en charge du protocole HTTPS

CDN peut être configuré afin d'utiliser le protocole HTTPS pour servir le contenu de façon sécurisée vers les utilisateurs finaux. Cette configuration nécessite qu'un certificat soit paramétré en tant qu'élément de la configuration CDN. Deux types d'options de certificats SSL sont disponibles pour HTTPS : [certificat de caractère générique](about-https.html#Wildcard-Certificate-support) et [certificat SAN (Subject Alternative Name) DV (Domain Validation)](about-https.html#subject-alternate-name-san-certificate-support), ce dernier type étant aussi appelé _certificat SAN_ dans cette documentation.
Le type de certificat SSL à utiliser est un choix important pour le CDN HTTPS. La procédure de configuration du certificat de caractère générique est rapide mais a pour inconvénient que le CDN n'est accessible que par le biais d'un enregistrement CNAME. Le processus de certificat SAN peut prendre 4 à 8 heures pour s'exécuter mais il permet d'utiliser le CDN avec le domaine CDN (c'est à dire le nom d'hôte). Le certificat SAN nécessite également une étape supplémentaire dans la procédure de [**validation DCV (Domain Control Validation)**](how-to-https.html) lors de la configuration. L'utilisation de l'un ou l'autre de ces certificats est gratuite. Consultez le document [Traitement des incidents](troubleshooting.html#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-) pour comprendre les implications de la sélection de l'un ou l'autre type de certificat.

L'hôte d'origine doit aussi avoir son propre certificat SSL pour le nom d'hôte CDN et doit être signé par une autorité de certification reconnue.

Conformément aux recommandations de l'industrie, Akamai ne fait confiance qu'aux certificats racine et non aux certificats intermédiaires, car les certificats intermédiaires de confiance changent fréquemment. Vous trouverez les certificats de confiance Akamai [ici](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Respect Headers

L'option **Respect Headers** permet à la configuration de l'en-tête HTTP de l'origine de remplacer la configuration du CDN. Cette fonction est activée par défaut, mais elle peut être désactivée.

## Traitement des contenus périmés

Lorsque le serveur d'équilibrage des charges du CDN reçoit une demande d'utilisateur et le contenu demandé n'est pas mis en cache, le serveur d'équilibrage des charges s'adresse à l'hôte d'origine pour récupérer le contenu. Ce contenu est ensuite mis en cache pendant la durée de vie spécifiée pour le contenu. Si une demande d'utilisateur est reçue après que la durée de vie spécifiée a expiré, le serveur d'équilibrage des charges s'adresse à l'hôte d'origine pour récupérer le contenu. Si le serveur d'origine ne peut pas être atteint pour une raison quelconque (par exemple parce que l'hôte d'origine est arrêté ou en cas de panne du réseau), le serveur d'équilibrage des charges sert le contenu expiré à la demande. Cette fonction est prise en charge par Akamai et **ne peut pas** être désactivée.

## Arguments Cache Key Query

Les serveurs d'équilibrage des charges Akamai mettent en cache les contenus dans un **magasin de cache**. Pour utiliser les contenus du **magasin de cache**, les serveurs d'équilibrage des charges se servent d'une **clé de cache**. Généralement, une **clé de cache** est générée sur la base d'une portion de l'URL d'un utilisateur final. Dans certains cas, l'URL contient des arguments de fonctions de demandes différents pour chaque utilisateur mais le contenu livré est le même. Par défaut, Akamai utilise les arguments de la fonction de demande pour générer la clé de cache et par conséquent pour générer une clé de cache unique pour chaque utilisateur. Cette méthode n'est pas optimale car elle oblige le serveur d'équilibrage des charges à contacter le serveur d'origine pour les contenus qui sont déjà mis en cache mais à l'aide d'une clé de cache différente. La fonction **Ignore Query Args in Cache Key** permet de spécifier si les arguments de la demande doivent être ignorés lors de la génération d'une clé de cache. Cette fonction s'applique à toute `création` ou `mise à jour` d'une configuration de mappage de CDN, ainsi qu'à toute `création` ou `mise à jour` d'un chemin d'origine.

**Remarque :** la valeur des arguments Cache Key Query (zone **Cache Key Query Args**) peut être configurée dans l'onglet **Paramètres** après la création d'un mappage CDN. En ce qui concerne le chemin d'origine, ces arguments peuvent être configurés lors des opérations de `création` ou de `mise à jour` d'un chemin d'origine.

## Compression de contenu

La **compression de contenu** est activée dans Akamai CDN par défaut pour les types de contenus suivants :
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Lorsque la compression est traitée par le serveur d'équilibrage des charges, le contenu doit avoir une taille minimale de 10 ko.  Dans certains cas, le serveur d'origine se charge de la compression et dans ces cas, il n'existe pas de limite de taille pour les fichiers à compresser. Si le contenu est déjà compressé par le serveur d'origine, il ne peut pas être recompressé. Pour activer la compression de contenu, l'en-tête de requête doit définir `Accept-Encoding: gzip`.

## Optimisation des fichiers volumineux

L'optimisation des fichiers volumineux permet au réseau CDN d'optimiser la distribution des contenus supérieurs à 10 Mo. Cette activation augmente les performances des fichiers volumineux et réduit les temps de latence et de téléchargement. Sans cette fonction, le CDN ne peut pas traiter les fichiers supérieurs à 1,8 Go. Cette fonction permet les téléchargements de fichiers supérieurs à 1,8 Go jusqu'à un maximum de 320 Go.

Pour que l'optimisation des fichiers volumineux fonctionne, les demandes de plages d'octets **doivent** être activées sur le serveur d'origine. Le CDN Akamai utilise une technique appelée la "mise en cache partielle des objets" pour cette optimisation. Lorsqu'un fichier volumineux est demandé, le serveur d'équilibrage des charges vérifie que le fichier répond aux exigences en matière de taille. Cela signifie que les fichiers supérieurs à 10 Mo seront demandés au serveur d'origine par blocs. Une fois que le bloc arrive sur le serveur d'équilibrage des charges, il est mis en cache et automatiquement servi à l'utilisateur. Le bloc suivant est pré-extrait en parallèle par le serveur d'équilibrage des charges, réduisant ainsi la latence. Ce processus continue jusqu'à ce que le fichier entier soit extrait ou que la connexion soit coupée.

Lorsque cette fonction est activée, le traitement d'un contenu de moins de 10 Mo entraîne un léger coût en terme de performance. C'est la raison pour laquelle, cette fonction est uniquement recommandée en cas de fichiers volumineux. Un cas d'utilisation typique serait de créer un nouveau chemin d'origine dans la configuration du CDN et d'activer l'optimisation des fichiers volumineux pour ce chemin.

## Vidéo à la demande

L'optimisation des performances de la **vidéo à la demande** offre une diffusion en flux de haute qualité sur des types de réseaux variés. En tirant parti de la capacité du réseau distribué à distribuer la charge de manière dynamique, IBM Cloud CDN avec Akamai vous offre la possibilité de vous adapter rapidement à de larges audiences, que vous les ayez planifiées ou non.

La **vidéo à la demande** est optimisée pour la distribution de formats de diffusion en flux segmentés tels que HLS, DASH, HDS et HSS. La diffusion en flux de vidéos en direct n'est **pas** prise en charge à l'heure actuelle. Vous pouvez activer la fonction **vidéo à la demande** en sélectionnant l'option correspondante dans le menu déroulant sous **Optimize for** sur l'onglet Settings ou lors de la création d'un nouveau chemin d'origine. Activez uniquement cette fonction lorsque vous souhaitez optimiser la livraison des fichiers vidéos.
