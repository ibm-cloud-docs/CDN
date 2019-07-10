---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# A propos de HTTPS
{: #about-https}

{{site.data.keyword.cloud}} propose deux façons de sécuriser votre CDN avec HTTPS - certificat de caractère générique et certificat SAN DV (Domain Validation). Ces deux options HTTPS peuvent être configurées en sélectionnant **Port HTTPS** quand vous configurez votre CDN. Le port HTTPS par défaut est 443 mais vous pouvez choisir un port différent pour le routage de votre trafic HTTPS. Une liste des numéros de port autorisés est proposée dans la rubrique [FAQ (Foire aux questions)](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

Pour savoir si vous devez utiliser un **certificat générique** ou un **certificat SAN** pour HTTPS, répondez à la question suivante : souhaitez-vous traiter le trafic HTTPS entre le nom CDN et le nom de domaine CDN ? Si vous souhaitez traiter le trafic HTTPS à partir de l'enregistrement CNAME, sélectionnez **Certificat générique**. Si vous souhaitez traiter le trafic HTTPS à partir du nom de domaine CDN, sélectionnez **Certificat SAN**.

## Prise en charge du certificat de caractère générique
{: #wildcard-certificate-support}

**Remarque** :
Les nouveaux mappages de CDN avec certificat de caractère générique ne sont PAS pris en charge pour le moment.

Le certificat de caractère générique est le moyen le plus simple de distribuer du contenu Web à vos utilisateurs finaux, en toute sécurité. Le nom complet CDN, incluant le suffixe du certificat de caractère générique, **doit** être utilisé en tant que point d'entrée de service (`https://example.cdnedge.bluemix.net`, par exemple) afin de pouvoir utiliser le certificat générique.

IBM Cloud CDN utilise le certificat de caractère générique `*.cdnedge.bluemix.net`. Le nom CNAME (créé pour vous ou fourni par vous), qui se termine par le suffixe `*.cdnedge.bluemix.net`, est ajouté au certificat de caractère générique conservé sur le serveur d'équilibrage des charges CDN. Et donc CNAME devient le seul moyen pour les utilisateurs finaux de se servir de HTTPS pour votre CDN.

![Diagramme relatif à HTTPS et au caractère générique](images/state-diagram-wildcard.png)

## Prise en charge du certificat SAN (Subject Alternate Name)
{: #san-certificate-suport}

Le certificat SAN (Subject Alternative Name) est un certificat SSL numérique qui permet à des domaines multiples, ou des noms d'hôte, d'être protégés par un certificat unique.

Avec un certificat SAN pour HTTPS, votre nom d'hôte CDN principal est ajouté à un certificat qui a été émis par une autorité de certification, ce qui donne à vos utilisateurs la possibilité d'accéder à votre service de façon sécurisée via le nom d'hôte plutôt que le nom CNAME (`https://www.example.com`, par exemple).

Quand la commande CDN est effectuée en utilisant le certificat SAN HTTPS, elle passe via le processus de demande d'un certificat et de création d'une validation DCV (Domain Control Validation). Cette validation DCV est le processus utilisé par l'autorité de certification pour établir que vous êtes bien autorisé à accéder au domaine et à le contrôler. Une action de votre part est requise pour effectuer cette étape. Après que le contrôle ait été établi, le certificat est déployé sur les serveurs d'équilibrage des charges CDN du monde entier. Une fois le certificat déployé, son renouvellement est géré automatiquement. Vous trouverez plus d'informations sur cette fonction dans la rubrique [Introduction aux réseaux de distribution de contenu (CDN)](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support). Les méthodes de validation DCV (Domain Control Validation) sont présentées en détail sur la page [Exécution de la validation DCV (Domain Control Validation) pour HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

Lorsque le CDN passe à l'état EN COURS D'EXECUTION, vous devez conserver l'enregistrement CNAME de nom d'hôte CDN dans votre serveur de noms de domaine. Si l'enregistrement CNAME est retiré, le nom d'hôte CDN peut être retiré du certificat SAN en 3 jours. Si cela se produit, le trafic HTTPS n'est plus distribué avec ce nom d'hôte CDN.
{:note}

![Diagramme relatif à HTTPS avec certificat SAN](images/state-diagram-san.png)
