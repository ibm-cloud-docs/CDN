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

# Limites et valeurs maximales

## Existe-t-il une valeur de durée de vie maximale ? Minimale ?

La valeur maximale du paramètre Time To Live est 2 147 483 647 secondes, ce qui correspond à environ 68 ans ! La valeur minimale est de 30 secondes.

## Existe-t-il une limite au nombre d'entrées TTL et d'origine ?

Oui, la limite totale est de 75 entrées par CDN.

## Existe-t-il une limite quant au nombre de CDN que je peux posséder ?

Oui, vous ne pouvez pas posséder plus de 10 CDN par compte.

## Existe-t-il une limite quant au nombre de demandes de purge pouvant être actives simultanément ?
Oui. 5 demandes de purge seulement peuvent être actives en même temps.

## Quelle est la plus grande taille de fichier pouvant être livrée via le CDN Akamai ?

Les tentatives d'envoi ou de livraison de fichiers supérieurs à 1,8 Go recevront une réponse `403 Access Forbidden` si la configuration de performance par défaut est utilisée. Si l'optimisation des fichiers volumineux est activée, les téléchargements de fichiers allant jusqu'à 320 Go sont possibles.
