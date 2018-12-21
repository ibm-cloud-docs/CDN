---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Classe de protection des liens dynamiques

La `classe SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` contient les attributs utilisés par vos API de protection des liens dynamiques. Cet objet est utilisé pour définir le comportement de protection des liens dynamiques pour un CDN en appelant l'API. Il est également renvoyé par les API de protection des liens dynamiques après un appel d'API réussi.

Classe `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` :

* `protectionType` : indique d'autoriser ou de refuser l'accès au contenu lorsqu'une requête HTTP contient une valeur d'en-tête Referer qui correspond à l'un des termes dans les valeurs `refererValues`. L'inverse se produit lorsqu'aucune correspondance n'est trouvée.
  * Valeurs possibles de protectionType :
    * `ALLOW`
    * `DENY`
* `refererValues` : est une liste d'URL de référence séparées par un unique espace, qui peuvent également avoir une correspondance par caractères génériques, sous forme de chaîne.
  * Exemples de chaîne **valide** pour `refererValues` :
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * Exemples de chaîne **non valide** pour `refererValues` :
    * `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...
      * Contient plus de 2100 caractères
    * `domain1.example.com domain1.example.com`
      * La liste contient des doublons
    * ` `
      * refererValues vide
    * `domain1.exa}mple.com domain1.example.com`
      * Utilisation d'un ou plusieurs caractères non spécifié(s) dans [RFC-3986](https://tools.ietf.org/html/rfc3986#section-2)
    * `www.example.com/path&`
      * Le caractères ` & ` n'est pas pris en charge
    * `www.example.org http://www.example.com`
      * Une chaîne `refererValues` qui contient au moins un terme de correspondance d'URL avec un caractère défini devant le premier caractère `.` contenant `://` n'est pas pris en charge
