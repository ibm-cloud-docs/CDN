---

copyright:
  years: 2018
lastupdated: "2018-10-04"

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

La valeur maximale du paramètre Time To Live est 2 147 483 647 secondes, ce qui correspond à environ 68 ans ! La valeur minimale est de 0 secondes.

## Existe-t-il une limite au nombre d'entrées TTL et d'origine ?

Oui, la limite totale est de 75 entrées par CDN.

## Quelle est la plus grande taille de fichier pouvant être livrée via le CDN Akamai ?

Les tentatives d'envoi ou de livraison de fichiers supérieurs à 1,8 Go recevront une réponse `403 Access Forbidden` si la configuration de performance par défaut est utilisée. Si l'optimisation des fichiers volumineux est activée, les téléchargements de fichiers allant jusqu'à 320 Go sont possibles.
