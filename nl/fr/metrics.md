---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Métriques
{: #metrics}

La première fois que vous sélectionnez votre CDN dans la liste, la page de présentation s'ouvre. La quantité totale de bande passante, le nombre total d'accès et le taux d'accès pour la période sélectionnée (par défaut, 30 jours) s'affichent.

  ![Présentation des métriques](images/metrics-overview.png)

Juste au-dessous de la présentation figure une représentation graphique de la quantité totale de bande passante, de la quantité de bande passante par région, du nombre total d'accès et du nombre d'accès par type.

  ![Graphiques de métriques](images/metrics-graphs.png)

**REMARQUE** : après que vous avez créé votre CDN, il peut s'écouler jusqu'à 48 heures avant que les métriques apparaissent.

## Existe-t-il un nombre minimum de jours pendant lesquels je peux afficher les métriques ? Existe-t-il un nombre maximum ?

Il existe un nombre minimum et un nombre maximum de jours pendant lesquels vous pouvez visualiser les métriques à l'aide d'{{site.data.keyword.cloud}} Content Delivery Network avec Akamai. Les métriques doivent être collectées pendant au moins 7 jours. Elles peuvent ensuite être visualisées pendant un maximum de 90 jours. Si vous utilisez l'API, il est recommandé de ne pas dépasser 89 jours afin de tenir compte des éventuels décalages horaires.

## Pourquoi le taux d'accès n'est-il pas zéro lorsque le nombre total d'accès est égal à zéro ?
Le taux d'accès correspond au pourcentage de distribution du contenu à partir de la mémoire cache du serveur d'équilibrage des charges en lieu et place du serveur d'origine. Il est calculé comme suit :

`((Edge hits - Ingress hits)/Edge hits) * 100

`

où :

_Edge hits_ correspond à tous les accès aux serveurs d'équilibrage des charges effectués par les utilisateurs finaux.  
_Ingress hits_ correspond au trafic depuis votre serveur d'origine vers les serveurs d'équilibrage des charges Akamai.

Etant donné que le taux d'accès est calculé au niveau du compte et non pour chaque CDN, il sera le même pour tous les CDN de votre compte. Cette situation explique pourquoi le taux d'accès peut être une valeur différente de zéro lorsque le nombre d'accès au serveur d'équilibrage des charges d'un CDN particulier est égal à zéro.

## Les métriques sont-elles mises à jour en temps réel ?

Non. Les métriques sont mises à jour toutes les 24 heures.
