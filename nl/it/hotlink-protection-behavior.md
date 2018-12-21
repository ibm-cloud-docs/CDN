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

# Classe Hotlink Protection

La `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking class` contiene gli attributi utilizzati dalle nostre API Hotlink Protection. Questo oggetto viene utilizzato per impostare la modalità di funzionamento di Hotlink Protection per una CDN mediante il richiamo dell'API. Viene anche restituito dalle API Hotlink Protection dopo una chiamata API eseguita correttamente.

class `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`:

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
    * `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...
      * Contiene più di 2100 caratteri
    * `domain1.example.com domain1.example.com`
      * Contiene dei duplicati nell'elenco
    * ` `
      * refererValues vuoti
    * `domain1.exa}mple.com domain1.example.com`
      * Utilizzo di carattere(i) non specificato(i) in [RFC-3986](https://tools.ietf.org/html/rfc3986#section-2)
    * `www.example.com/path&`
      * Il carattere `&` non è supportato
    * `www.example.org http://www.example.com`
      * Una stringa `refererValues` con almeno un termine di corrispondenza URL che ha una serie di caratteri davanti al primo carattere `.` che contiene `://` non è supportata
