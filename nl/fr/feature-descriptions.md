---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

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

# Description des fonctions
{: #feature-descriptions}

Cette page présente beaucoup des fonctions incluses avec {{site.data.keyword.cloud}} CDN optimisé par Akamai.

## Prise en charge de l'origine du serveur hôte
{: #host-server-origin-support}

IBM Cloud Content Delivery Network (CDN) peut être configuré pour traiter les contenus d'une origine de serveur hôte en fournissant le nom d'hôte de l'origine, le protocole, le numéro de port et en option, le chemin à partir duquel le contenu doit être servi. Le chemin par défaut est `/*`. Le protocole peut être HTTP, HTTPS, ou les deux. Seuls certains numéros de ports sont pris en charge par Akamai. Consultez la [FAQ](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) pour connaître les numéros de ports et les plages pris en charge.

## Prise en charge de l'origine du stockage d'objet
{: #object-storage-file-support}

IBM Cloud CDN peut être configuré pour servir le contenu provenant d'un noeud final de stockage d'objet en fournissant le noeud final, le nom de compartiment, le protocole et le port. Facultativement, une liste des extensions de fichiers peut être spécifiée pour autoriser uniquement la mise en cache des fichiers possédant ces extensions. Tous les objets du compartiment doivent être définis avec un accès en lecture anonyme ou public.

Pour obtenir des informations complémentaires, consultez le tutoriel [Accelerate delivery of static files using a CDN](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn).

## Prise en charge de plusieurs origines avec différents chemins
{: #support-for-multiple-origins-with-distinct-paths}

Dans certains cas, il est possible que vous souhaitiez livrer certains contenus depuis un serveur d'origine différent. Par exemple, vous souhaitez que certaines photos ou vidéos soient distribuées à partir de serveurs d'origine différents. IBM Cloud CDN offre la possibilité de configurer plusieurs serveurs d'origine avec plusieurs chemins d'accès. Ceci offre une certaine flexibilité quant au mode et au lieu de stockage des données. Le chemin fourni pour le serveur d'origine doit être unique par rapport au CDN. Le serveur d'origine lui-même n'a pas besoin d'être unique.

## Mappages de CDN basés sur des chemins
{: #path-based-cdn-mappings}

Votre service IBM Cloud CDN peut être restreint à un chemin de répertoire spécifique sur le serveur d'origine si vous fournissez le chemin lors de la création du CDN. Un utilisateur final est uniquement autorisé à accéder aux contenus se trouvant dans ce chemin de répertoire. Ainsi, si un CDN `www.example.com` est créé avec le chemin `/videos`, il est **uniquement** accessible via `www.example.com/videos/*`.

## Purge des contenus mis en cache
{: #purge-cached-content}

IBM Cloud CDN offre la possibilité de supprimer ou de "purger" le contenu mis en cache des serveurs d'équilibrage des charges, et ce de manière facile et rapide. A l'heure actuelle, la purge n'est possible que pour un chemin de fichier. L'implémentation de la purge avec Akamai purge actuellement le fichier en environ 5 secondes. L'interface utilisateur offre également la possibilité d'afficher votre historique de purge et d'ajouter des chemins de purge spécifiques à vos favoris.

## Durée de vie (TTL)
{: #time-to-live-ttl}

La "durée de vie" indique la durée (en secondes) pendant laquelle le serveur d'équilibrage des charges conservera le contenu en cache d'un fichier ou d'un chemin de répertoire particulier. Quand un CDN est créé pour la première fois, une durée de vie globale (TTL) est créée pour le chemin `/\*` avec une durée par défaut de 3600 secondes. La valeur minimale de TTL est de 0 secondes et la valeur maximale de 2147483647 secondes. Pour chaque entrée, le chemin TTL doit être unique pour le CDN. Si plusieurs chemins correspondent à un contenu donné, le chemin qui aura été configuré en dernier sera appliqué pour ce contenu. Prenons l'exemple de deux TTL : `/example/file` est créé en premier avec une valeur de durée de vie de 3000 secondes et `/example/*` est créé par la suite avec une valeur de durée de vie de 4000 secondes. Bien que `/example/file` soit plus spécifique, `/example/*` a été créé en dernier, et par conséquent la durée de vie de `/example/file` sera de 4000 secondes. Une fois créées, les entrées TTL peuvent être éditées (chemin et/ou durée). Elles peuvent également être supprimées.

## Mesures avec vues graphiques
{: #metrics-with-graphical-views}

Les mesures d'un CDN particulier sont fournies sur l'onglet Présentation du portail client pour ce mappage de CDN. Il existe deux types de mesures calculés à partir de l'utilisation du CDN : celles qui affichent les changements enregistrés au cours d'une période sous forme de graphique et celles qui sont affichées sous forme de valeurs agrégées.

Les mesures qui affichent les changements relatifs à une période sous forme de graphique offrent trois diagrammes linéaires et un graphique circulaire. Les trois diagrammes linéaires sont : **Bandwidth**, **Hits by Mapping** et **Hits by Type**. Ils affichent l'activité de manière quotidienne pendant toute la durée de l'intervalle spécifié. Les diagrammes **Bandwidth** et **Hits by Mapping** sont des diagrammes à une seule ligne, alors que la répartition **Hits by Type** affiche une ligne pour chaque type d'accès fourni. Le graphique circulaire affiche une répartition par région de la bande passante d'un mappage de CDN sous forme de pourcentage.

Les mesures affichées sous forme de valeurs agrégées incluent **l'utilisation de la bande passante** en Go, le **nombre d'accès total** au serveur d'équilibrage des charges du CDN et le **taux d'accès**. Le taux d'accès indique le nombre de fois en pourcentage où le contenu est livré par le serveur d'équilibrage des charges, et _non_ par son origine. Actuellement, le taux d'accès est affiché comme une fonction de tous vos mappages de CDN et pas seulement du mappage en cours d'affichage.

Par défaut, les numéros agrégés et les graphiques affichent les mesures des 30 derniers jours, mais vous pouvez modifier cette configuration dans le [portail client![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/). Les deux catégories peuvent afficher des mesures sur des périodes de 7, 15, 30, 60 ou 90 jours.

## Prise en charge des en-têtes d'hôte
{: #host-header-support}

Le serveur d'équilibrage des charges utilise l'en-tête **Host Header**  lors de la communication avec l'hôte d'origine. Cette fonction offre de la flexibilité quant à la manière dont le service Web est configuré sur l'hôte d'origine. Elle permet en particulier les scénarios d'utilisation dans lesquels un client dispose de plusieurs serveurs Web configurés sur le même hôte d'origine. Si l'entrée de l'en-tête d'hôte n'est pas fournie, le service utilise le nom d'hôte du serveur d'origine en tant qu'en-tête d'hôte HTTP par défaut si le serveur d'origine est spécifié en tant que nom d'hôte (et non en tant qu'adresse IP). Si l'en-tête d'hôte n'est pas fourni et le serveur d'origine est fourni sous forme d'adresse IP, le nom d'hôte du CDN (également appelé le nom de domaine du CDN) est utilisé en tant qu'en-tête d'hôte HTTP par défaut.

## Prise en charge du protocole HTTPS
{: #https-protocol-support}

CDN peut être configuré afin d'utiliser le protocole HTTPS pour servir le contenu de façon sécurisée vers les utilisateurs finaux. Cette configuration nécessite qu'un certificat soit paramétré en tant qu'élément de la configuration CDN. Deux types d'options de certificats SSL sont disponibles pour HTTPS : [certificat de caractère générique](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) et [certificat SAN (Subject Alternative Name) DV (Domain Validation)](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport), ce dernier type étant aussi appelé _certificat SAN_ dans cette documentation.

Le type de certificat SSL à utiliser est un choix important pour le CDN HTTPS. La procédure de configuration du certificat de caractère générique est rapide mais a pour inconvénient que le CDN n'est accessible que par le biais d'un enregistrement CNAME. Le processus de certificat SAN peut prendre 4 à 8 heures pour s'exécuter mais il permet d'utiliser le CDN avec le domaine CDN (c'est à dire le nom d'hôte). Le certificat SAN nécessite également une étape supplémentaire dans la procédure de [**validation DCV (Domain Control Validation)**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) lors de la configuration. L'utilisation de l'un ou l'autre de ces certificats est gratuite. Consultez le document [Traitement des incidents](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-) pour comprendre les implications de la sélection de l'un ou l'autre type de certificat.

L'hôte d'origine doit aussi avoir son propre certificat SSL pour le nom d'hôte CDN et doit être signé par une autorité de certification reconnue.

Conformément aux recommandations de l'industrie, Akamai ne fait confiance qu'aux certificats racine et non aux certificats intermédiaires, car les certificats intermédiaires de confiance changent fréquemment. Vous trouverez les certificats de confiance Akamai [ici](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Respect Headers
{: #respect-headers}

L'option **Respect Headers** permet à la configuration de l'en-tête HTTP de l'origine de remplacer la configuration du CDN. Cette fonction est activée par défaut, mais elle peut être désactivée.

## Traitement des contenus périmés
{: #serve-stale-content}

Lorsque le serveur d'équilibrage des charges du CDN reçoit une demande d'utilisateur et le contenu demandé n'est pas mis en cache, le serveur d'équilibrage des charges s'adresse à l'hôte d'origine pour récupérer le contenu. Ce contenu est ensuite mis en cache pendant la durée de vie spécifiée pour le contenu. Si une demande d'utilisateur est reçue après que la durée de vie spécifiée a expiré, le serveur d'équilibrage des charges s'adresse à l'hôte d'origine pour récupérer le contenu. Si le serveur d'origine ne peut pas être atteint pour une raison quelconque (par exemple parce que l'hôte d'origine est arrêté ou en cas de panne du réseau), le serveur d'équilibrage des charges sert le contenu expiré à la demande. Cette fonction est prise en charge par Akamai et **ne peut pas** être désactivée.

## Optimisation des clés de cache
{: #cache-key-optimization}

Les serveurs d'équilibrage des charges Akamai mettent en cache les contenus dans un **magasin de cache**. Pour utiliser les contenus du **magasin de cache**, les serveurs d'équilibrage des charges se servent d'une **clé de cache**. Généralement, une **clé de cache** est générée sur la base d'une portion de l'URL d'un utilisateur final. Dans certains cas, l'URL contient des arguments de fonctions de demandes différents pour chaque utilisateur mais le contenu livré est le même. Par défaut, Akamai utilise les arguments de la fonction de demande pour générer la clé de cache et par conséquent pour générer une clé de cache unique pour chaque utilisateur. Cette méthode n'est pas optimale car elle oblige le serveur d'équilibrage des charges à contacter le serveur d'origine pour les contenus qui sont déjà mis en cache mais à l'aide d'une clé de cache différente. La fonction d'**optimisation de clé de cache** permet de spécifier les arguments de requête à inclure ou ignorer lors de la génération d'une clé de cache. Cette fonction s'applique à toute `création` ou `mise à jour` d'une configuration de mappage de CDN, ainsi qu'à toute `création` ou `mise à jour` d'un chemin d'origine.

La valeur d'**d'optimisation de clé de cache** peut être configurée dans l'onglet **Paramètres** après la création d'un mappage CDN. En ce qui concerne le chemin d'origine, ces arguments peuvent être configurés lors des opérations de `création` ou de `mise à jour` d'un chemin d'origine.
{: note}

## Compression de contenu
{: #content-compression}

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
{: #large-file-optimization}

L'optimisation des fichiers volumineux permet au réseau CDN d'optimiser la distribution des contenus supérieurs à 10 Mo. Cette activation augmente les performances des fichiers volumineux et réduit les temps de latence et de téléchargement. Sans cette fonction, le CDN ne peut pas traiter les fichiers supérieurs à 1,8 Go. Cette fonction permet les téléchargements de fichiers supérieurs à 1,8 Go, jusqu'à un maximum de 320 Go.

Pour que l'optimisation des fichiers volumineux fonctionne, les demandes de plages d'octets **doivent** être activées sur le serveur d'origine. Le CDN Akamai utilise une technique appelée la _mise en cache partielle des objets_ pour cette optimisation. Lorsqu'un fichier volumineux est demandé, le serveur d'équilibrage des charges vérifie que le fichier répond aux exigences en matière de taille. Cela signifie que les fichiers supérieurs à 10 Mo seront demandés au serveur d'origine par blocs. Une fois que le bloc arrive sur le serveur d'équilibrage des charges, il est mis en cache et automatiquement distribué à l'utilisateur. Le bloc suivant est pré-extrait en parallèle par le serveur d'équilibrage des charges, réduisant ainsi la latence. Ce processus continue jusqu'à ce que le fichier entier soit extrait ou que la connexion soit coupée.

Lorsque cette fonction est activée, le traitement d'un contenu de moins de 10 Mo entraîne un léger coût en terme de performance. C'est la raison pour laquelle, cette fonction est uniquement recommandée en cas de fichiers volumineux. Un cas d'utilisation typique serait de créer un nouveau chemin d'origine dans la configuration du CDN et d'activer l'optimisation des fichiers volumineux pour ce chemin.

## Vidéo à la demande
{: #video-on-demand}

L'optimisation des performances de la **vidéo à la demande** offre une diffusion en flux de haute qualité sur des types de réseaux variés. En tirant parti des paramètres de contrôle de cache préconfigurés et de la capacité du réseau distribué pour répartir la charge de manière dynamique, IBM Cloud CDN avec Akamai vous offre la possibilité de vous adapter rapidement à de larges audiences, que vous les ayez planifiées ou non.

La **vidéo à la demande** est optimisée pour la distribution de formats de diffusion en flux segmentés tels que HLS, DASH, HDS et HSS. La diffusion en flux de vidéos en direct n'est **pas** prise en charge à l'heure actuelle. Vous pouvez activer la fonction **vidéo à la demande** en sélectionnant l'option correspondante dans le menu déroulant sous **Optimize for** sur l'onglet Settings ou lors de la création d'un nouveau chemin d'origine. Activez uniquement cette fonction lorsque vous souhaitez optimiser la livraison des fichiers vidéo.

## Contrôle d'accès géographique
{: #geographical-access-control}

Le contrôle d'accès géographique est un comportement basé sur les règles qui vous permet de définir le paramètre `access-type` pour un groupe d'utilisateurs, en fonction de leur emplacement géographique. Deux types de comportement sont disponibles : **Autoriser** et **Refuser**.

Le type d'accès `Autoriser` vous permet d'autoriser le trafic vers des régions sélectionnées, en fonction du type de région. Lorsque le trafic est autorisé pour certaines régions, il est implicitement bloqué pour toutes les autres régions. Par exemple, vous pouvez choisir d'`autoriser` le trafic vers des continents sélectionnés, par exemple, l'Europe et l'Océanie, et entraîner ainsi le blocage de l'accès pour tous les autres continents.

En revanche, le comportement `Refuser` bloque l'accès à votre service pour le groupe spécifié, mais autorise l'accès pour toutes les autres régions non spécifiées. Par exemple, si vous affectez la valeur `Refuser` au type d'accès Contrôle d'accès géographique pour l'Europe et l'Océanie, les utilisateurs de ces continents ne pourront **pas** utiliser votre service, tandis que les utilisateurs sur tous les autres continents auront accès à ce service.

Cette fonction est accessible à partir de la page **Paramètres** de votre configuration de CDN.

## Protection des liens dynamiques
{: #hotlink-protection}

La protection des liens dynamiques est un comportement basé sur des règles qui permet de contrôler si certains sites Web sont autorisés à accéder à votre contenu depuis votre CDN. En général, le navigateur inclut un en-tête `Referer` lorsqu'une requête HTTP est émise depuis un lien d'une page Web et quand ce lien pointe sur un actif distant. Le lien qu'utilise ce site Web pour accéder à un actif d'un autre site Web est appelé _lien dynamique_.  Deux types de comportement sont disponibles : **ALLOW** (autoriser) et **DENY** (refuser).

Lorsque `protectionType` est défini sur `ALLOW` :
* Si la valeur de l'en-tête `Referer` d'une requête envoyée à votre CDN correspond à l'une des valeurs `refererValues` spécifiées, votre CNDS **diffusera** le contenu demandé.
* Sinon, votre CDN ne diffusera **pas** le contenu.

Lorsque `protectionType` est défini sur `DENY` :
* Si la valeur de l'en-tête `Referer` d'une requête envoyée à votre CDN correspond à l'une des valeurs `refererValues` spécifiées, votre CNDS **ne diffusera pas** le contenu demandé.
* Sinon, votre CDN diffusera le contenu.
