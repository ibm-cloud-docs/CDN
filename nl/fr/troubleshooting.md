---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Traitement des incidents
{: #troubleshooting}

Ce document présente différentes méthodes permettant de traiter les incidents liés à votre {{site.data.keyword.cloud}} CDN. Si vous avez besoin de contacter le support, prenez soin d'indiquer le numéro de référence de votre CDN.

## Comment savoir que mon CDN fonctionne correctement ?
{: #how-do-I-know-my-cdn-is-working}

Exécutez la commande `curl` suivante en remplaçant `http://your.cdn.domain/uri` par le chemin de fichier respectif sur votre CDN :

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

Si le résultat de la commande `curl` est semblable à l'exemple de format suivant, cela signifie que le CDN fonctionne correctement :

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```
{: screen}

## J'ai reçu une erreur 503. Pourquoi ?
{: #i-received-a-503-error-why}

Le plus souvent, l'erreur 503 est générée lorsqu'il existe un problème lié à un certificat de la chaîne de certificats SSL.

L'erreur qui peut s'afficher est la suivante : `503 Service Unavailable`.  

En même temps que l'erreur 503, un message d'erreur semblable au message suivant peut également apparaître : `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (le numéro de référence réel peut être différent). Dans ce cas, le numéro de référence dans la chaîne de l'erreur débouche sur un échec d'établissement de liaison SSL.

Pour remédier au problème, assurez-vous que les certificats SSL de votre serveur d'origine répondent aux critères suivants :
  * Le certificat **doit** être émis par une autorité de certification sécurisée par Akamai. Vous pouvez afficher la liste de certificats sécurisés Akamai en cliquant sur [ce lien ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)
  * Il **doit** correspondre à la valeur *Host header* configurée sur le CDN
  * Il ne doit **pas** être auto-signé
  * Il ne doit **pas** être arrivé à expiration

Si vous avez vérifié la chaîne de certificats de votre serveur d'origine à l'aide des critères précédents et que l'erreur persiste, consultez la rubrique [Aide et support](/docs/infrastructure/CDN?topic=CDN-gettinghelp). Notez la chaîne d'erreur de référence et mentionnez-la lorsque vous communiquez avec nous.

## Mon nom d'hôte n'est pas chargé sur le navigateur lorsqu'IBM Cloud Object Storage (COS) est le serveur d'origine
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

Lorsque votre CDN {{site.data.keyword.cloud_notm}} est configuré pour utiliser COS comme stockage d'objets, l'accès direct au site Web ne fonctionne pas. Vous devez spécifier le chemin de demande complet dans la barre d'adresse du navigateur (par exemple, `www.example.com/index.html`). Ce comportement est provoqué par une limitation du document d'index dans IBM COS.

## Je ne parviens pas à me connecter via une commande `curl` ou un navigateur à l'aide du nom d'hôte avec HTTPS.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

Si votre CDN a été créé en utilisant HTTPS avec un certificat générique, la connexion doit être établie à l'aide de l'enregistrement CNAME ; par exemple, `https://www.exampleCname.cdnedge.bluemix.net`. Cela inclut **tous** les CDN créés à l'aide de HTTPS avant le 18 juin 2018. Si vous essayez de vous connecter à l'aide du nom d'hôte, une erreur sera générée.

## Quel est le comportement attendu lors du chargement de l'enregistrement CNAME ou du nom d'hôte sur votre navigateur pour les protocoles pris en charge ?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

Ce tableau présente le comportement attendu pour les protocoles pris en charge lors du chargement du **nom d'hôte** ou de l'enregistrement **CNAME** à partir de votre navigateur Web.

<table>
<caption caption-side=“top”>Tableau des comportements attendus</caption>
<thead>
<tr>
<th rowspan=2 scope="col">URL de navigateur</th>
<th rowspan=2 scope="col">CDN avec protocole HTTP uniquement</th>
<th colspan=2 scope="col">CDN avec protocole HTTPS uniquement</th>
<th colspan=2 scope="col">CDN avec les protocoles HTTP et HTTPS</th>
</tr>
<tr>
<th scope="col"> Générique </th>
<th scope="col"> SAN partagé </th>
<th scope="col"> Générique </th>
<th scope="col"> SAN partagé </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> Affichage du message Successful load </td>
<td> Affichage du message 301 Moved permanently </td>
<td> Affichage du message Successful load </td>
<td> Affichage du message 301 Moved permanently </td>
<td> Affichage du message Successful load </td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> Affichage du message Access denied </td>
<td> Redirection vers la page Web IBM Cloud </td>
<td> Affichage du message Successful load </td>
<td> Redirection vers la page Web IBM Cloud </td>
<td> Affichage du message Successful load </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> Affichage du message 301 Moved permanently </td>
		<td> Affichage du message Successful load </td>
		<td> Affichage du message 301 Moved permanently </td>
		<td> Affichage du message Successful load </td>
		<td> Affichage du message 301 Moved permanently </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> Redirection vers la page Web IBM Cloud </td>
		<td> Affichage du message Successful load </td>
		<td> Affichage du message 301 Moved permanently </td>
		<td> Affichage du message Successful load </td>
		<td> Redirection vers la page Web IBM Cloud </td>
</tr>
</tbody>
</table>

**Messages d'erreur courants :**

La plupart du temps, un message `301 Moved permanently` indique que vous tentez d'atteindre un CDN avec un protocole `HTTPS` ou `HTTP_AND_HTTPS` en utilisant le nom d'hôte. En raison d'une limitation du certificat générique HTTPS, vous **devez** utiliser l'enregistrement CNAME pour accéder à votre CDN.

Avec un protocole HTTP **uniquement**, vous recevez le message `301 Moved permanently` si vous tentez d'atteindre votre CDN à l'aide de l'enregistrement CNAME. Dans ce cas, vous pouvez accéder à votre CDN _uniquement_ à l'aide du nom d'hôte.

Le message `Access denied` s'affiche lorsque vous tentez d'atteindre un CDN à l'aide d'un protocole incorrecte. Vérifiez que vous utilisez `http` pour les CDN créés avec le protocole HTTP ou `https` pour les CDN créés avec le protocole HTTPS.

**Erreur de redirection possible :**

Lorsqu'une URL est redirigée vers la page Web CDN {{site.data.keyword.cloud_notm}}, cela signifie le plus souvent que l'URL est incorrecte pour le protocole. Si votre CDN est créé avec un protocole HTTPS ou HTTPS_AND_HTTPS, vous devez utiliser l'enregistrement CNAME pour accéder à votre CDN. Par exemple : `https://examplecname.cdnedge.bluemix.net` pour les mappages HTTPS ou `http://examplecname.cdnedge.bluemix.net` ou `https://examplecname.cdnedge.bluemix.net` pour les mappages HTTP_AND_HTTPS.

Dans ce cas, l'URL est redirigée vers la page Web CDN {{site.data.keyword.cloud_notm}} car le protocole et le domaine sont incorrects pour le protocole du CDN. Lorsqu'il est créé avec HTTP comme protocole _unique_, le CDN peut être atteint _uniquement_ via le nom d'hôte. Par exemple, `http://example.com`.
