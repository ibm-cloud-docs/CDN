---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-30"

keywords: limitations, Akamai, multiple files, purge, hotlink, limit

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Limitations connues
{: #known-limitations}

Les limitations suivantes s'appliquent au service CDN avec le fournisseur Akamai :
* La purge de contenu au niveau des répertoires ou de plusieurs fichiers n'est actuellement pas prise en charge.
* Le nombre total d'origines par CDN est limité à 75.
* HTTPS avec un certificat SAN DV n'est disponible que pour les nouveaux CDN.
* La protection des liens dynamiques ne prend pas en charge les termes de correspondance d'URL `refererValues` dans lesquels le caractère défini devant le premier caractère `.` contient le regroupement de caractères `://`, par exemple `http://www.example.com` ou `www.example.com http://www.example.com`
* HTTPS avec des certificats de caractère générique n'est pas pris en charge pour le moment.
* La fonctionnalité STOP CDN n'est pas prise en charge pour les domaines de certificats de caractère générique.

La fonctionnalité STOP CDN est destinée aux fenêtres de maintenance ne dépassant pas 7 jours. Au-delà de 7 jours, le CDN doit être démarré ou bien il sera désactivé et le trafic vers le CDN CNAME ne sera pas redirigé vers le serveur d'origine.
{: important}
