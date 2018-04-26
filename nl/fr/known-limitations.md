---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limitations connues

Les limitations suivantes s'appliquent au nouveau service CDN avec le fournisseur Akamai :
* HTTPS est actuellement pris en charge via les certificats génériques uniquement.
* La purge de contenu au niveau des répertoires ou de plusieurs fichiers n'est actuellement pas pris en charge.
* Le nombre maximal de CDN actuellement autorisés pour un compte {{site.data.keyword.BluSoftlayer_notm}} est de 10.
* Le nombre total d'entrées TTL et d'origines est actuellement limité à 75 par CDN.
* La fonctionnalité Serve Stale Content est toujours définie sur **On**, même si le CDN a été créé avec l'option **Off**. 
* Si le CDN a été créé avec les options **Server** et **HTTP Port**, la zone d'origine ne peut être ajoutée qu'avec l'option **Server**.
