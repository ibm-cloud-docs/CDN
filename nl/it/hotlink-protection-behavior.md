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

# Classe Hotlink Protection
{: #hotlink-protection-class}

La `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection class` contiene gli attributi utilizzati dalle nostre API Hotlink Protection. Questo oggetto viene utilizzato per impostare la modalità di funzionamento di Hotlink Protection per una CDN mediante il richiamo dell'API.  Viene anche restituito dalle API Hotlink Protection dopo una chiamata API eseguita correttamente.

**class** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`:

* `://` nella prima etichetta dell'URL **non** è supportato.
   * **Valido**: `http*www.example.com`
   * **Non valido**: `http://www.example.com`

* `protectionType`: specifica di consentire o rifiutare l'accesso al contenuto quando una richiesta HTTP ha un valore Referer Header che corrisponde a uno dei termini in `refererValues`. Si verificherà il contrario quando non c'è alcuna corrispondenza.
  * Valore possibile per protectionType:
    * `ALLOW`
    * `DENY`
* `refererValues`: è un elenco separato da singoli spazi di URL referer, che possono anche avere una corrispondenza di carattere jolly, nel formato di una stringa.
  * Alcuni esempi di una stringa **valida** per `refererValues`:
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * Alcuni esempio di una stringa **non valida** per `refererValues`:
   
      |**Descrizione di un esempio non valido**|Esempio
      |-------|-----|
      |Contiene più di 2100 caratteri in totale| `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |Contiene almeno un termine di corrispondenza di URL maggiore di 255 caratteri. | `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |Contiene dei duplicati nell'elenco | `domain1.example.com domain1.example.com`|
      |refererValues vuoti | ` `|
      |Utilizzo di carattere(i) non specificato(i) in [RFC-3986 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://tools.ietf.org/html/rfc3986#section-2) | `domain1.exa}mple.com domain1.example.com`|
      |Il carattere `&` non è supportato| `www.example.com/path&`|
      |Una stringa `refererValues` con almeno un termine di corrispondenza URL che ha una serie di caratteri davanti al primo carattere `.` che contiene `://` non è supportata| `www.example.org http://www.example.com`|


