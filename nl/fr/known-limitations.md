---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

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
* La purge de contenu au niveau des répertoires ou de plusieurs fichiers n'est actuellement pas pris en charge.
* Le nombre maximal de CDN actuellement autorisés pour un compte {{site.data.keyword.BluSoftlayer_notm}} est de 10.
* Le nombre total d'entrées TTL et d'origines est actuellement limité à 75 par CDN.
* La fonctionnalité de traitement des contenus périmés est toujours définie sur **On**.
* Le type de certificat HTTPS ne peut pas être modifié une fois qu'un mappage est créé (d'un certificat de caractère générique à un certificat SAN DV, par exemple).
* HTTPS avec un certificat SAN DV n'est disponible que pour les nouveaux CDN.
* Un CDN créé avec un certificat SAN DV ne peut être supprimé à moins que son état soit RUNNING, CNAME_Configuration, CREATE_ERROR ou DELETE_ERROR.
