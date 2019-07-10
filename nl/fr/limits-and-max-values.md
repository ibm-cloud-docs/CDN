---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limites et valeurs maximales
{: #limits-and-maximum-values}

## Existe-t-il une valeur de durée de vie maximale ? Minimale ?
{: #is-there-a-maximum-value-for-time-to-live}

La valeur maximale du paramètre Time To Live est 2 147 483 647 secondes, ce qui correspond à environ 68 ans ! La valeur minimale est de 0 secondes.

## Existe-t-il une limite au nombre d'entrées TTL et d'origine ?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Oui, la limite totale est de 75 entrées par CDN.

## Quelle est la plus grande taille de fichier pouvant être livrée via le CDN Akamai ?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Les tentatives d'envoi ou de livraison de fichiers supérieurs à 1,8 Go recevront une réponse `403 Access Forbidden` si la configuration de performance par défaut est utilisée. Si l'optimisation des fichiers volumineux est activée, les téléchargements de fichiers allant jusqu'à 320 Go sont possibles.

## Quelle est la taille maximale pour les en-têtes de réponse d'origine ?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

La taille maximale d'un en-tête de réponse est de 16 ko. Au cas où certaines origines tenteraient de définir des cookies trop grands pour que le client les retourne dans les demandes suivantes, Akamai définit cette limite sur les serveurs d'équilibrage des charges. Si les en-têtes de réponse dépassent 16 ko, la demande reçoit une réponse de type `502 Passerelle incorrecte`. 

## Quelle est la taille maximale pour les en-têtes de demande du client ?
{: #what-is-the-maximum-size-for-the-client-request-headers}

La taille maximale d'un en-tête de demande est de 16 ko. Si les en-têtes de demande dépassent 32 ko, le serveur d'équilibrage des charges Akamai renvoie une réponse de type `400 Demande incorrecte`.
