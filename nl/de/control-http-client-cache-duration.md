---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cachedauer eines HTTP-Clients mit Cachesteuerung steuern
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

Bei Verwendung eines CDN sind zwei Caching-Stufen verfügbar:

  * **Caching in der Peripherie** tritt auf, wenn ein CDN-Edge-Server Inhalt vom Ursprung zwischenspeichert.
  * Ein dem Netz der Edge-Server **nachgeordnetes Caching** tritt auf, wenn ein Endbenutzer oder HTTP-Client, z. B. ein anfordernder Browser, Inhalt von einem Edge-Server zwischenspeichert.

Die Methode, die Sie auswählen, um zu steuern, wie lange Inhalte beim Anforderer, z. B. einem Browser, zwischengespeichert werden, hängt von den folgenden Faktoren ab:

  * Ob die [Einstellung für 'Header beibehalten'](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details) EIN oder AUS lautet. Standardmäßig ist sie auf EIN gesetzt.
  * Ob der Ursprungsserver einen `max-age`-Wert im Cache-Control-Header für einen bestimmten Inhalt bereitstellt. 

Unabhängig davon, wie sich diese Faktoren ändern, muss Ihr Ursprung einen Cache-Control-Header für den beabsichtigten Inhalt für die Peripherie bereitstellen, wenn Edge-Server HTTP-Antworten mit dem Cache-Control-Header für diesen Inhalt senden sollen.

Im Wesentlichen weisen die von einem Edge-Server an nachgeordnete Elemente gesendeten Cache-Control-Header den Anforderer an, die zugehörigen Inhalte gemäß den Caching-Anweisungen oder -Werten zwischenzuspeichern, die vom Edge-Server angegeben werden.

## Header beibehalten: Aus
{: #respect-header-off}

Wenn Ihr Ursprung einen Cache-Control-Header mit `max-age`-Anweisung und -Wert für einen bestimmten Inhalt bereitstellt, wird die Cachedauer für diesen Inhalt, der in der Peripherie zwischengespeichert wird, dennoch aus den TTL-Einstellungen Ihres CDN abgeleitet. Darüber hinaus antwortet der Edge-Server dem nachgeordneten Anforderer mit einem `max-age`-Wert für die Cachesteuerung, der dem niedrigeren der folgenden Werte entspricht:
  * `max-age`-Wert des Ursprungs für die Cachesteuerung
  * Verbleibende Zeit, bis der Inhalt an der Peripherie veraltet ist

Wenn Ihr Ursprung jedoch keinen Cache-Control-Header für den Edge-Server bereitstellt, stellen die Edge-Server keinen Cache-Control-Header für den Anforderer bereit. Die Cachedauer für den Inhalt an der Peripherie wird dennoch von den TTL-Einstellungen Ihres CDN abgeleitet.

## Header beibehalten: Ein
{: #respect-header-on}

Wenn Ihr Ursprung einen Cache-Control-Header mit `max-age` für einen bestimmten Inhalt bereitstellt, bestimmt der `max-age`-Wert des Ursprungs für die Cachesteuerung die Cachedauer für diesen Inhalt in der Peripherie und setzt anwendbare TTL-Einstellungen für diesen Inhalt außer Kraft. Darüber hinaus antwortet die Peripherie dem Anforderer mit einem `max-age`-Wert für die Cachesteuerung, der der verbleibenden Zeit entspricht, bis der Inhalt an der Peripherie veraltet ist.

Wenn Ihr Ursprung jedoch keinen Cache-Control-Header für den Edge-Server bereitstellt, stellt der Edge-Server keinen Cache-Control-Header für den Anforderer bereit. Die Cachedauer für den Inhalt an der Peripherie wird dennoch von den TTL-Einstellungen Ihres CDN abgeleitet.

## Zusammenfassung
{: #summary}

|Header beibehalten|Ursprung stellt Cache-Control bereit|Cachedauer des jeweiligen Inhalts auf dem Edge-Server|Edge-Server stellt Cache-Control bereit|
|---|---|---|---|
|Ein|Ja, Ursprung gibt `max-age` an|Cachedauer in der Peripherie durch `max-age`-Wert des Ursprungs außer Kraft gesetzt|Ja, Peripherie stellt ebenfalls einen `max-age`-Wert bereit, der der (außer Kraft gesetzten) Zeit entspricht, bis die Peripherie den Inhalt vom Ursprung aktualisieren muss|
|Ein|Ja, aber Ursprung gibt `max-age` nicht an|Cachedauer in der Peripherie basiert auf der TTL-Konfiguration des CDN|Ja, Peripherie stellt ebenfalls einen `max-age`-Wert bereit, der der Zeit entspricht, bis die Peripherie den Inhalt vom Ursprung aktualisieren muss|
|Ein|Nein|Cachedauer in der Peripherie basiert auf der TTL-Konfiguration des CDN|Nein|
|Aus|Ja, Ursprung gibt `max-age` an|Cachedauer in der Peripherie basiert auf der TTL-Konfiguration des CDN|Ja, Peripherie stellt ebenfalls einen `max-age`-Wert bereit, der dem `max-age`-Wert des Ursprungs oder der Zeit entspricht, bis die Peripherie den Inhalt vom Ursprung aktualisieren muss, je nachdem, welcher Wert niedriger ist|
|Aus|Ja, aber Ursprung gibt `max-age` nicht an|Cachedauer in der Peripherie basiert auf der TTL-Konfiguration des CDN|Ja, Peripherie stellt ebenfalls einen `max-age`-Wert bereit, der der Zeit entspricht, bis die Peripherie den Inhalt vom Ursprung aktualisieren muss|
|Aus|Nein|Cachedauer in der Peripherie basiert auf der TTL-Konfiguration des CDN|Nein|

## Weitere Informationen zur Cachesteuerung
{: #more-information-on-cache-control}

* Informationen zur [Verwaltung des CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* Cachesteuerung entsprechend der Definition in Abschnitt 14.9 von [RFC 2616 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ietf.org/rfc/rfc2616.txt)
