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

# Klasse für Hot-Link-Schutz
{: #hotlink-protection-class}

Die Klasse `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` enthält die Attribute, die von unseren Hot-Link-Schutz-APIs verwendet werden. Dieses Objekt wird verwendet, um das Verhalten des Hot-Link-Schutzes für ein CDN durch Aufrufen der API festzulegen.  Es wird auch von Hot-Link-Schutz-APIs nach einem erfolgreichen API-Aufruf zurückgegeben.

**Klasse** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`:

* `://` in der ersten Bezeichnung der URL wird **nicht** unterstützt.
   * **Gültig**: `http*www.example.com`
   * **Ungültig**: `http://www.example.com`

* `protectionType`: Gibt an, ob Zugriff auf Inhalte zugelassen oder verweigert wird, wenn der Wert des Referer-Headers einer HTTP-Anforderung mit einem der Begriffe in `refererValues` übereinstimmt. Das Gegenteil geschieht, wenn es keine Übereinstimmung gibt.
  * Mögliche Werte für protectionType:
    * `ALLOW`
    * `DENY`
* `refererValues`: Eine durch ein Leerzeichen getrennte Liste von Referrer-URLs, die auch in Form von Zeichenfolgen mit Platzhalterzeichen angegeben werden können.
  * Einige Beispiele für eine **gültige** Zeichenfolge für `refererValues`:
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * Einige Beispiele für eine **ungültige** Zeichenfolge für `refererValues`:
   
      |**Beschreibung des ungültigen Beispiels**| Beispiel
      |-------|-----|
      |Enthält mehr als 2100 Zeichen insgesamt| `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |Enthält mindestens einen URL-Übereinstimmungsbegriff, der mehr als 255 Zeichen enthält.| `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |Liste enthält Duplikate | `domain1.example.com domain1.example.com`|
      |Leere Zeichenfolge für 'refererValues' | ` `|
      |Verwendung von Zeichen, die nicht in [RFC-3986 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc3986#section-2) spezifiziert sind.| `domain1.exa}mple.com domain1.example.com`|
      |Zeichen `&` wird nicht unterstützt| `www.example.com/path&`|
      |Eine `refererValues`-Zeichenfolge mit mindestens einem URL-Abgleichbegriff, bei dem die Zeichengruppe vor dem ersten Zeichen `.` die Zeichenfolge `://` enthält, wird nicht unterstützt.| `www.example.org http://www.example.com`|


