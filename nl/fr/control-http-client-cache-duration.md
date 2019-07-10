---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Utilisation du contrôle de cache pour contrôler la durée en cache d'un client HTTP
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

Lorsque vous utilisez un CDN, deux niveaux de mise en cache sont disponibles :

  * La **mise en cache au niveau du serveur d'équilibrage des charges**, qui se produit lorsqu'un serveur d'équilibrage des charges CDN met en cache un élément de contenu à partir du serveur d'origine.
  * La **mise en cache en aval** depuis le réseau périphérique des serveurs, qui se produit lorsqu'un utilisateur final ou un client HTTP, tel un navigateur demandeur, met en cache un élément de contenu à partir d'un serveur d'équilibrage des charges.

La méthode choisie pour contrôler pendant combien de temps le contenu est mis en cache au niveau du demandeur, tel un navigateur, dépend des facteurs suivants :

  * Le [paramètre Respect Header](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details) est défini sur ON ou OFF. Par défaut, il est défini sur ON.
  * Le serveur d'origine fournit ou non une valeur `max-age` dans l'en-tête de contrôle de cache pour un élément de contenu particulier. 

Indépendamment de la manière dont ces facteurs se modifient, votre origine doit fournir au serveur d'équilibrage des charges un en-tête de contrôle de cache pour le contenu escompté si vous voulez que les serveurs d'équilibrage des charges envoient des réponses HTTP avec l'en-tête de contrôle de cache pour ce contenu.

Pour l'essentiel, les en-têtes de contrôle de cache envoyés depuis un serveur d'équilibrage des charges en aval demandent au demandeur de mettre en cache le contenu associé selon les directives ou les valeurs de mise en cache indiquées par le serveur d'équilibrage des charges.

## Option Respect Header: Off
{: #respect-header-off}

Si votre origine fournit un en-tête de contrôle de cache avec une valeur et une directive `max-age` pour un élément de contenu spécifique, la durée en cache pour cet élément de contenu mis en cache sur le serveur d'équilibrage des charges est toujours dérivée des paramètres TTL du CDN. En outre, le serveur d'équilibrage des charges répond au demandeur en aval avec une valeur de contrôle de cache `max-age` égale ou inférieure à :
  * La valeur `max-age` de contrôle de cache du serveur d'origine.
  * Le temps restant avant péremption du contenu sur le serveur d'équilibrage des charges.

Toutefois, si votre serveur d'origine ne fournit pas un en-tête de contrôle de cache au serveur d'équilibrage des charges, celui-ci ne fournira pas d'en-tête de contrôle de cache au demandeur. La durée en cache du serveur d'équilibrage des charges pour votre contenu est toujours dérivée des paramètres TTL du CDN.

## Option Respect Header: On
{: #respect-header-on}

Si votre origine fournit un en-tête de contrôle de cache avec `max-age` pour un élément de contenu spécifique, la valeur `max-age` de contrôle de cache du serveur d'origine devient la durée en cache de cet élément de contenu sur le serveur d'équilibrage des charges, supplantant tout paramètre TTL applicable pour cet élément de contenu. En outre, le serveur d'équilibrage des charges répond au demandeur avec une valeur de contrôle de cache `max-age` égale au temps restant avant péremption du contenu sur le serveur d'équilibrage des charges.

Toutefois, si votre serveur d'origine ne fournit pas un en-tête de contrôle de cache au serveur d'équilibrage des charges, celui-ci ne fournira pas d'en-tête de contrôle de cache au demandeur. La durée en cache du serveur d'équilibrage des charges pour votre contenu est toujours dérivée des paramètres TTL du CDN.

## Récapitulatif
{: #summary}

|Option Respect Header|Contrôle de cache fourni par le serveur d'origine|Durée en cache de contenu spécifique sur le serveur d'équilibrage des charges|Contrôle de cache fourni par le serveur d'équilibrage des charges|
|---|---|---|---|
|Activée (On)|Oui, le serveur d'origine fournit une valeur `max-age`|La durée en cache du serveur d'équilibrage des charges remplace la valeur `max-age` du serveur d'origine|Oui, le serveur d'équilibrage des charges fournit également une valeur `max-age` qui correspond (se substitue) au délai accordé au serveur d'équilibrage des charges avant actualisation du contenu sur le serveur d'origine|
|Activée (On)|Oui, mais le serveur d'origine ne spécifie pas de valeur `max-age`|La durée en cache du serveur d'équilibrage des charges basée sur la configuration TTL de CDN|Oui, le serveur d'équilibrage des charges fournit également une valeur `max-age` qui correspond au délai accordé au serveur d'équilibrage des charges avant actualisation du contenu sur le serveur d'origine|
|Activée (On)|Non|La durée en cache du serveur d'équilibrage des charges basée sur la configuration TTL de CDN|Non|
|Désactivée (Off)|Oui, le serveur d'origine fournit une valeur `max-age`|La durée en cache du serveur d'équilibrage des charges basée sur la configuration TTL de CDN|Oui, le serveur d'équilibrage des charges fournit également une valeur `max-age` inférieure à la valeur `max-age` du serveur d'origine et qui correspond au délai accordé au serveur d'équilibrage des charges avant actualisation du contenu sur le serveur d'origine|
|Désactivée (Off)|Oui, mais le serveur d'origine ne spécifie pas de valeur `max-age`|La durée en cache du serveur d'équilibrage des charges basée sur la configuration TTL de CDN|Oui, le serveur d'équilibrage des charges fournit également une valeur `max-age` qui correspond au délai accordé au serveur d'équilibrage des charges avant actualisation du contenu sur le serveur d'origine|
|Désactivée (Off)|Non|La durée en cache du serveur d'équilibrage des charges basée sur la configuration TTL de CDN|Non|

## Informations supplémentaire sur le contrôle de cache
{: #more-information-on-cache-control}

* Comment [gérer votre CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* Contrôle de cache défini dans la section 14.9 du document [RFC 2616 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ietf.org/rfc/rfc2616.txt)
