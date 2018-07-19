---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Mesures

## Où puis-je consulter les valeurs de mesure et d'utilisation ?

Vous pouvez afficher les valeurs de mesure et d'utilisation sur la page de **présentation**, accessible en sélectionnant votre CDN à partir de la page **Content Delivery Networks**. **REMARQUE** : après avoir créé votre CDN, il peut s'écouler jusqu'à 48 heures avant que les mesures apparaissent.

## J'ai créé un CDN et il y avait un trafic de données à travers le CDN. Pourquoi mes mesures ne s'affichent-elles pas ?

Après la création d'un CDN, il faut attendre 48 heures pour que les mesures apparaissent.


## Existe-t-il un nombre minimum de jours pendant lesquels je peux afficher les mesures ? Existe-t-il un nombre maximum ?

Il existe un nombre minimum et un nombre maximum de jours pendant lesquels vous pouvez visualiser les mesures à l'aide d'IBM Cloud Content Delivery Network avec Akamai. Les mesures doivent être collectées pendant au moins 7 jours. Elles peuvent ensuite être visualisées pendant un maximum de 90 jours. Si vous utilisez l'API, il est recommandé de ne pas dépasser 89 jours afin de tenir compte des éventuels décalages horaires.

## Pourquoi le taux d'accès n'est-il pas zéro lorsque le nombre total de correspondances est égal à zéro ?
Le taux d'accès correspond au pourcentage de distribution du contenu à partir de la mémoire cache du serveur d'équilibrage des charges en lieu et place du serveur d'origine. Il est calculé comme suit :

> ((Edge hits - Ingress hits)/Edge hits) * 100

où :
"Edge hits" correspond à tous les accès aux serveurs d'équilibrage des charges effectués par les utilisateurs finaux  
"Ingress hits" correspond au trafic depuis votre serveur d'origine vers les serveurs d'équilibrage des charges Akamai

Etant donné que le taux d'accès est calculé au niveau du compte et non pour chaque CDN, il sera le même pour tous les CDN de votre compte. Cette situation explique pourquoi le taux d'accès peut être une valeur différente de zéro lorsque le nombre d'accès au serveur d'équilibrage des charges d'un CDN particulier est égal à zéro.

