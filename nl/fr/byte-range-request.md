---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Utilisation des demandes de plages d'octets
{: #working-with-byte-range-requests}

En vous servant d'une demande de plage d'octets, vous pouvez extraire des contenus partiels d'un serveur d'origine. Ce document vous permet de comprendre les codes de statut des réponses que vous pouvez voir.

Quand une **demande de plage d'octets** est envoyée via {{site.data.keyword.cloud}} CDN avec Akamai, l'utilisateur peut recevoir un code de réponse `200 (OK)` pour la première demande et un code de réponse `206` pour toutes les demandes suivantes, puisque les serveurs d'équilibrage des charges Akamai demandent le contenu depuis l'origine en format compressé. Ainsi, quand un serveur d'équilibrage des charges n'a pas d'objet dans son cache ni d'informations sur la longueur du contenu de l'objet, il effectue un transfert vers l'origine et demande l'objet dans son intégralité. En retour, l'origine sert l'objet sans l'en-tête de longueur de contenu à Akamai et l'utilisateur final reçoit l'objet entier même s'il s'agit d'une demande de plage d'octets, ce qui explique le code de statut `200`. Lors des demandes suivantes, le serveur d'équilibrage des charges a l'objet dans son cache et renvoie le code d'état `206`.

L'en-tête **Range HTTP request** indique quelle partie du contenu doit être renvoyée par le serveur. Un en-tête de plage permet de demander plusieurs parties en même temps et le serveur peut renvoyer ces plages dans une réponse multipartie. Si le serveur renvoie des plages, il répond avec un statut 206 (contenu partiel).

Une façon de garantir une réponse 206, même pour la première demande de plage d'octets est de désactiver `Transfer-Encoding: chunked` sur votre serveur d'origine.
