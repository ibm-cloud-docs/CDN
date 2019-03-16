---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Utilisation du contrôle de cache `max-age` pour contrôler la durée de la mise en cache du navigateur

Lorsqu'un CDN est utilisé, il existe un type de mise en cache pour chaque emplacement sur lequel la mise en cache peut se produire :
  * La **mise en cache de niveau navigateur** se produit lorsque le navigateur met en cache un élément de contenu pour une durée spécifique.
  * La **mise en cache de niveau serveur d'équilibrage des charges** se produit lorsqu'un serveur d'équilibrage des charges CDN met en cache un élément de contenu à partir du serveur d'origine pour une durée spécifique qui n'est pas nécessaire identique.

Quelques méthodes permettent de contrôler la durée pendant laquelle le contenu est mis en cache au niveau du navigateur. La méthode à utiliser dépend des facteurs suivants :
  * Vous avez ou non mis à jour ou vous allez ou non [mettre à jour les détails de configuration de votre CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details) avec l'option “Respect Header: On”.
  * Le serveur d'origine fournit ou non une valeur `max-age` dans l'en-tête de contrôle de cache pour un élément de contenu donné. 

Quelle que soit la façon dont ces facteurs changent, votre serveur d'origine doit fournit un en-tête de contrôle de cache non vide pour votre contenu ciblé. Si votre serveur d'origine ne fournit pas un en-tête de contrôle de cache non vide pour le serveur d'équilibrage des charges, celui-ci ne fournira pas son propre en-tête de contrôle de cache au navigateur.

Lorsque le serveur d'équilibrage des charges envoie un en-tête de contrôle de cache au navigateur, la valeur `max-age` pour cet en-tête de contrôle de cache correspond au délai restant avant l'expiration du contenu du serveur d'équilibrage des charges et sa nécessaire revalidation à partir du serveur d'origine. 

## Option Respect Header: Off
Lorsque l'option Respect Header a pour valeur **Off**, vous pouvez contrôler la durée du cache de niveau navigateur en configurant les paramètres de durée de vie de votre CDN. Pour chaque fichier ou répertoire de fichiers, vous devez créer une règle de durée de vie pour son chemin et la durée de cache souhaitée.

Lors de cette configuration, les deux actions suivantes sont effectuées :
  * La règle de durée de vie indique au serveur d'équilibrage des charges qu'il doit effectuer la mise en cache du contenu pendant la durée spécifiée.
  * Lorsque le contenu est distribué à partir du serveur d'équilibrage des charges, celui-ci envoie au navigateur son propre en-tête de contrôle de cache avec une valeur `max-age` basée sur la valeur de la durée de vie.

## Option Respect Header: On
Lorsque l'option  **Respect Header** a pour valeur **On**, vous pouvez choisir de ne pas configurer de règle de durée de vie pour contrôler la durée de la mise en cache au niveau du navigateur pour votre contenu. Dans ce cas, vous devez configurer votre serveur d'origine pour qu'il fournisse un en-tête de contrôle de cache avec une valeur `max-age` pour un élément de contenu donné. Etant donné que l'option Respect Header est activée (On), le serveur d'équilibrage des charges utilise la valeur `max-age` du contrôle de cache à partir du serveur d'origine comme durée de vie pour ce contenu spécifique. Si le contenu est déjà couvert par une entrée de règle de durée de vie pour votre CDN, en réalité, le serveur d'équilibrage des charges continue d'utiliser la valeur `max-age` et remplace la durée de mise en cache au niveau du serveur d'équilibrage des charges qui existe éventuellement pour ce contenu.

Lors de cette configuration, les deux actions suivantes sont effectuées :
  * La valeur de la directive `max-age` du contrôle de cache émanant du serveur d'origine indique au serveur d'équilibrage des charges qu'il doit mettre en cache le contenu pendant la durée spécifiée.
  * Lorsque le contenu est distribué à partir du serveur d'équilibrage des charges, celui-ci envoie son propre en-tête de contrôle de cache au navigateur avec une valeur `max-age` basée sur la valeur `max-age` provenant du serveur d'origine.

Aucun autre contenu couvert par cette règle de durée de vie n'est affecté si vous utilisez la méthode ciblée illustrée dans l'exemple.

Lorsque l'option **Respect Header** a pour valeur **On**, la durée de la mise en cache sur le navigateur peut être contrôlée par une règle de durée de vie, si le serveur d'origine n'envoie pas la directive `max-age` dans l'en-tête de contrôle de cache. Si une valeur `max-age` est spécifiée, celle-ci peut de manière fortuite écraser la valeur de temps sur la règle de durée de vie.

## Récapitulatif

|Option Respect Header|Contrôle de cache fourni par le serveur d'origine|Durée de mise en cache de contenu spécifique sur le serveur d'équilibrage des charges|Contrôle de cache fourni par le serveur d'équilibrage des charges|
|---|---|---|---|
|Activée (On)|Oui, une valeur `max-age` est indiquée|Remplacée par la valeur `max-age` du serveur d'origine|Oui, `max-age` est le délai restant avant la revalidation nécessaire du contenu du serveur d'équilibrage des charges à partir du serveur d'origine|
|Activée (On)|Oui, une valeur `max-age` n'est pas indiquée|Basée sur la configuration de durée de vie du CDN|Oui, `max-age` est le délai restant avant la revalidation nécessaire du contenu du serveur d'équilibrage des charges à partir du serveur d'origine|
|Activée (On)|Non|Basée sur la configuration de durée de vie du CDN|Non|
|Désactivée (Off)|Oui, une valeur `max-age` est indiquée|Basée sur la configuration de durée de vie du CDN|Oui, `max-age` est le délai restant avant la revalidation nécessaire du contenu du serveur d'équilibrage des charges à partir du serveur d'origine|
|Désactivée (Off)|Oui, une valeur `max-age` n'est pas indiquée|Basée sur la configuration de durée de vie du CDN|Oui, `max-age` est le délai restant avant la revalidation nécessaire du contenu du serveur d'équilibrage des charges à partir du serveur d'origine|
|Désactivée (Off)|Non|Basée sur la configuration de durée de vie du CDN|Non|

## Informations complémentaires
* Comment [gérer votre CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Contrôle de cache défini dans la section 14.9 du document [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
