---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-25"

keywords: hotlink, protection, class, behavior, API, valid string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Classe de protection des liens dynamiques
{: #hotlink-protection-class}

La `classe SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` contient les attributs utilisés par vos API de protection des liens dynamiques. Cet objet est utilisé pour définir le comportement de protection des liens dynamiques pour un CDN en appelant l'API.  Il est également renvoyé par les API de protection des liens dynamiques après un appel d'API réussi.

**Classe** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` :

* `://` dans le premier libellé de l'URL n'est **pas** pris en charge.
   * **Valide** : `http*www.example.com`
   * **Non valide** : `http://www.example.com`

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
   
      |**Description d'un exemple non valide**| Exemple
      |-------|-----|
      | Contient plus de 2100 caractères au total| `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |Contient au moins un terme de correspondance d'URL comportant plus de 255 caractères | `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |La liste contient des doublons | `domain1.example.com domain1.example.com`|
      |refererValues vide | ` `|
      |Utilisation de caractère(s) non spécifié(s) dans [RFC-3986 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://tools.ietf.org/html/rfc3986#section-2) | `domain1.exa}mple.com domain1.example.com`|
      |Le caractères ` & ` n'est pas pris en charge| `www.example.com/path&`|
      |Une chaîne `refererValues` qui contient au moins un terme de correspondance d'URL avec un caractère défini devant le premier caractère `.` contenant `://` n'est pas pris en charge| `www.example.org http://www.example.com`|


