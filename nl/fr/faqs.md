---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Foire aux questions

## Qu'est-ce qu'un CDN ?

Un CDN (Content Delivery Network) est une collection de serveurs d'équilibrage des charges répartis dans différentes régions du pays ou du monde. Leur contenu Web est distribué à partir d'un serveur d'équilibrage des charges, situé dans la zone géographique la plus proche du client qui adresse la demande de contenu. Cette technique permet à l'utilisateur de recevoir le contenu plus rapidement qu'avec la technique de distribution de contenu à partir d'un emplacement centralisé. Elle garantit une expérience client optimale. 

## Comment fonctionne un CDN ?

Un CDN met en cache le contenu Web sur des serveurs d'équilibrage des charges du monde entier. Lorsqu'un utilisateur fait une demande de contenu Web, la demande est acheminée vers le serveur le plus proche. En réduisant la distance géographique, le réseau CDN offre un débit optimal, une latence réduite et de meilleures performances. 

## Quelle est la procédure de création d'un CDN ?

Votre compte est créé au cours du processus de commande CDN, lorsque vous cliquez sur **Select** après avoir consulté le menu de sélection des fournisseurs.

## Que dois-je faire lorsque mon CDN est à l'état CNAME_CONFIGURATION ?

Mettez à jour votre enregistrement DNS de sorte que votre site Web pointe vers le `CNAME` associé à votre nouveau mappage CDN.
**Remarque** : L'activation de la mise à jour peut prendre jusqu'à 15-30 minutes. Contactez votre fournisseur DNS pour obtenir une évaluation de temps plus précise.

## Quels frais me seront facturés pour l'utilisation du CDN ?

La bande passante utilisée par le CDN vous sera facturée. Si votre CDN n'utilise pas de bande passante, aucuns frais ne vous seront facturés. Le prix de la bande passante est variable en fonction de la zone géographique où se trouve le serveur d'équilibrage des charges.

## Quand serais-je facturé ?

La facturation du CDN dépend de la période de facturation définie dans votre compte {{site.data.keyword.BluSoftlayer_notm}}.

## Où puis-je consulter les valeurs de mesure et d'utilisation ?

Vous pouvez afficher les valeurs de mesure et d'utilisation sur la page de **présentation**, accessible en sélectionnant votre CDN à partir de la page **Content Delivery Networks**. **Remarque** : Une fois votre CDN créé, l'affichage des mesures peut nécessiter jusqu'à 48 heures.

## Existe-t-il un nombre minimum de jours avant de pouvoir afficher les mesures ? Existe-t-il un nombre maximum ?

Oui. Les mesures doivent être collectées pendant au moins 7 jours. Elles peuvent ensuite être visualisées pendant un maximum de 90 jours. Si vous utilisez l'API, il est recommandé de ne pas dépasser 89 jours afin de tenir compte des éventuels décalages horaires.

## Comment fonctionne le certificat générique HTTPS ?

Le certificat générique constitue la manière la plus économique de distribuer du contenu Web à vos utilisateurs finaux et ce, en toute sécurité. Pour en bénéficier, le client doit utiliser le nom d'hôte CNAME comme point d'entrée de service (par exemple, _https://example.cdnedge.bluemix.net_). Une fois le mappage CDN activé sur HTTPS à l'aide du certificat générique, le serveur d'équilibrage des charges contacte le serveur d'origine via HTTPS. Si le serveur d'origine est défini comme nom d'hôte, le serveur d'équilibrage des charges utilise le domaine d'origine comme en-tête SNI (Server Name Indication) pour établir la communication TLS avec le serveur d'origine, par défaut. Toutefois, si le domaine d'origine est spécifié en tant qu'adresse IP, l'en-tête d'hôte d'origine doit être fourni. Une autre astuce consiste à faire signer le certificat d'origine par une autorité de certification.

## Si je sélectionne `delete` dans le menu déroulant dynamique du CDN, mon compte est-il supprimé ?

Non, votre compte CDN n'est pas supprimé. Il existe toujours et vous pouvez créer des CDN supplémentaires.

## La mise en cache utilise-t-elle la commande push ou pull ?

La mise en cache de contenu s'appuie sur un modèle _origin pull_. L'extraction d'origine (Origin Pull) est une méthode par laquelle les données sont "extraites" par le serveur d'équilibrage des charges à partir du serveur d'origine, contrairement au téléchargement manuel du contenu vers le serveur d'équilibrage des charges. 

## Existe-t-il une valeur de durée de vie maximale ? Minimale ?

La valeur maximale de durée de vie est 21 474 836 471 secondes, ce qui correspond environ à 681 ans! La valeur minimale est de 30 secondes.

## Qu'est-ce que l'option de distribution de contenu expiré (Serve Stale) ?

Si le délai de mise en cache d'un contenu arrive à expiration, le serveur d'équilibrage des charges tente de récupérer le contenu du serveur d'origine. Si pour une raison quelconque le serveur est hors service ou ne peut pas être contacté, le serveur d'équilibrage des charges peut distribuer le contenu expiré lorsque cette option a été activée (préférence utilisateurs). Actuellement, nos CDN peuvent uniquement être configurés avec cette option définie sur **Yes** par défaut. La sélection de la valeur **No** n'a aucune incidence. L'option **Serve Stale Content** (distribution de contenu expiré) reste **Yes**.

## Existe-t-il une limite au nombre d'entrées TTL et d'origine ?

Oui, la limite totale est de 75 entrées par CDN.

## Existe-t-il une limite quant au nombre de CDN que je peux posséder ?

Oui, vous ne pouvez pas posséder plus de 10 CDN par compte.

## Existe-t-il un navigateur recommandé pour la configuration du service CDN ?

Oui, les versions les plus récentes de Firefox et de Chrome sont recommandées.

## A quoi sert d'indiquer un chemin d'accès lors de la création de mon CDN ?

Si vous fournissez un chemin d'accès lors de la création d'un CDN, celui-ci vous permet d'isoler les fichiers distribués via le CDN d'un serveur d'origine particulier.

## Comment puis-je savoir si mon CDN fonctionne ?
Exécutez la commande 'curl' en remplaçant "origin.cdntesting.net/assets/ibm_3d.gif" par le chemin d'accès au fichier approprié sur votre origine : 
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
Consultez la page [Aide et support](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp), ou ouvrez un ticket sur la page [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).

## Où puis-je trouver mon CName si je n'en ai pas fourni ? 
Cliquez sur le CDN pour accéder à la page de **présentation**. Dans l'angle inférieur droit apparaît la section **Détails** avec les informations relatives au `CName`.

## Existe-t-il une limite quant au nombre de demandes de purge pouvant être actives simultanément ?
Oui. 5 demandes de purge seulement peuvent être actives en même temps.

## Ma demande de purge d'un chemin donné est en cours. Puis-je soumettre une nouvelle demande pour le même chemin ?
Non. Il ne peut y avoir qu'une seule demande de purge active à la fois pour un même chemin donné.
