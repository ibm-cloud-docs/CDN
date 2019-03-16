---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Introduction aux réseaux de distribution de contenu (CDN)
{: #about-content-delivery-networks-cdn-}

Un réseau de distribution de contenu (CDN - Content Delivery Network) est un ensemble de serveurs d'équilibrage des charges répartis dans différentes parties d'un pays ou du monde. Le contenu Web est distribué à partir d'un serveur d'équilibrage des charges situé dans la zone géographique la plus proche du client qui adresse la demande de contenu. Cette technique permet à vos utilisateurs finaux de recevoir le contenu dans des délais plus courts et offre une meilleure expérience globale à vos clients.

## Comment fonctionne un CDN ?

L'objectif d'un CDN est de mettre en cache du contenu Web sur des serveurs d'équilibrage des charges dans le monde entier. Lorsqu'un client fait une demande de contenu Web, la demande est acheminée vers le serveur le plus proche. En réduisant la distance géographique, le réseau CDN offre un débit optimal, une latence réduite et de meilleures performances. Grâce à IBM Cloud Content Delivery Network avec Akamai, les fournisseurs de contenu peuvent réaliser une diffusion efficace du contenu demandé dans le monde entier, et ce avec une configuration minimale.

![Diagramme CDN de niveau supérieur](images/high-level-cdn-diagram.png)

## Fonctions

Les fonctions principales du service {{site.data.keyword.cloud}} Content Delivery Network incluent :
  * La prise en charge de l'origine du serveur hôte
  * La prise en charge de l'origine du stockage d'objet
  * La prise en charge de plusieurs origines avec différents chemins
  * Les mappages de CDN basés sur des chemins
  * La purge des contenus mis en cache
  * La durée de vie (TTL)
  * Les mesures avec vues graphiques
  * La prise en charge du protocole HTTPS avec certificat de caractère générique et certificat SAN DV
  * La prise en charge des en-têtes d'hôte
  * Le traitement des contenus périmés
  * Les arguments Cache Key Query
  * La compression de contenu
  * L'optimisation des fichiers volumineux
  * La vidéo à la demande
  * Le contrôle d'accès géographique
  * La protection des liens dynamiques

Consultez le [document de description des fonctions](/docs/infrastructure/CDN/feature-descriptions.html#feature-descriptions) pour des descriptions complètes des fonctions.

## Diagramme de l'architecture

Le diagramme suivant offre une présentation schématique de l'architecture IBM Cloud CDN à trois niveaux.

![Diagramme de l'architecture](images/3-tier-architecture.png)
