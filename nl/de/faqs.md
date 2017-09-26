---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Häufig gestellte Fragen (FAQs)

## Was ist Content Delivery Network (CDN)?

Content Delivery Network (CDN) ist eine Sammlung von Edge-Servern, die über verschiedene Teile eines Landes oder der Welt verteilt sind. Die zugehörigen Webinhalte werden von einem Server bereitgestellt, dessen geografische Region dem Kunden, der die Inhalte anfordert, am nächsten liegt. Dieses Verfahren ermöglicht eine schnellere Zustellung der Inhalte als bei der Bereitstellung an einem zentralen Standort. Dies führt zu einer besserer Serviceerfahrung für Ihre Kunden.

## Wie funktioniert CDN?

CDN erreicht seinen beabsichtigten Zweck durch das Caching von Webinhalten auf Edge-Servern auf der ganzen Welt. Wenn ein Benutzer Webinhalte anfordert, wird die Anfrage an den Edge-Server weitergeleitet, dessen geografischer Standort dem Benutzer am nächsten liegt. Durch das Verkürzen des Übertragungswegs für die angeforderten Inhalte, bietet das CDN optimierten Durchsatz, minimierte Latenzzeiten und ein besseres Leistungsverhalten. 

## Wie wird mein CDN-Konto erstellt?

Ihr Konto wird während des CDN-Bestellprozesses erstellt, wenn Sie auf **Auswählen** klicken, nachdem Sie die Schritte im Menü für die Anbieterauswahl ausgeführt haben.

## Was kann ich tun, wenn mein CDN den Status CNAME_CONFIGURATION aufweist?

Aktualisieren Sie Ihren DNS-Datensatz, damit Ihre Website auf die zugeordnete Eigenschaft `CNAME` Ihrer neuen CDN-Zuordnung verweist.
**Hinweis**: Es kann 15 bis 30 Minuten dauern, bis die Aktualisierung wirksam wird. Eine genaue Zeitschätzung können Sie von Ihrem DNS-Provider erhalten.

## Was wird mir in Rechnung gestellt?

Abgerechnet wird die genutzte Bandbreite pro CDN. Wenn Ihr CDN keine Bandbreite nutzt, fallen keine Gebühren an. Der Preis für die Bandbreite variiert je nach regionalem Standort des Edge-Servers.

## Wann erfolgt die Abrechnung?

Die CDN-Abrechnung erfolgt für den in Ihrem {{site.data.keyword.BluSoftlayer_notm}}-Konto festgelegten Abrechnungszeitraum. 

## Wie kann ich die Metriken und die Nutzung anzeigen?

Die Metriken und die Nutzung können auf der Seite **Übersicht** angezeigt werden, die durch das Auswählen Ihres CDN auf der Seite **Content Delivery Networks** aufgerufen werden kann. **Hinweis**: Nach dem Erstellen Ihres CDN können bis zu 48 Stunden vergehen, bevor Metriken angezeigt werden.

## Gibt es eine Mindestanzahl von Tagen, für die ich Metriken anzeigen kann? Gibt es einen Maximalwert?

Ja. Metriken können für einen Mindestzeitraum von 7 Tagen erfasst werden. Der maximale Zeitraum, für den Metriken angezeigt werden können, beträgt 90 Tage. Falls Sie die API verwenden, werden 89 Tage als Maximalwert empfohlen, um Zeitzonenabweichungen zu berücksichtigen.

## Wie funktioniert das HTTPS-Platzhalterzertifikat?

Das Platzhalterzertifikat ist die ökonomischste Methode zum sicheren Bereitstellen von Webinhalten für Ihre Endbenutzer. Bei dieser Methode muss der Kunde den CNAME-Hostnamen als Serviceeingangspunkt verwenden (z. B. _https://example.cdnedge.bluemix.net_). Nachdem die Verwendung des Platzhalterzertifikats für die CDN-Zuordnung aktiviert ist, stellt der Edge-Server über HTTPS die Verbindung zum Ursprungsserver her. Wenn der Ursprungsserver als Hostname angegeben wird, verwendet der Edge-Server standardmäßig die Ursprungsdomäne als SNI-Header (SNI = Server Name Indication) für die TLS-Vereinbarung mit dem Ursprungsserver (TLS = Transport Layer Security). Wird die Ursprungsdomäne jedoch als IP-Adresse angegeben, sollte der Ursprungshost-Header bereitgestellt werden. Tipp: Das Ursprungszertifikat muss von einer anerkannten Zertifizierungsstelle (Certificate Authority, CA) signiert sein.

## Wird mein Konto gelöscht, wenn ich die Option `Löschen` im Überlaufmenü des CDN auswähle?

Nein, das betreffende CDN wird nicht gelöscht. Ihr Konto ist weiterhin vorhanden und Sie können weitere CDN-Instanzen erstellen.

## Werden beim Caching Push- oder Pull-Operationen verwendet?

Für das Caching von Inhalten wird ein Modell _Extrahieren aus Quelle_ verwendet. Bei diesem Modell extrahiert der Edge-Server durch eine Pull-Operation Daten aus dem Ursprungsserver, d. h. die Inhalte werden nicht manuell auf den Edge-Server hochgeladen. 

## Gibt es einen Maximalwert für die Lebensdauer? Gibt es einen Mindestwert?

Der Maximalwert für die Lebensdauer (Time To Live, TTL) sind 21.474.836.471 Sekunden. Dies entspricht ungefähr 681 Jahren. Der Mindestwert sind 30 Sekunden.

## Was ist die Option zum Bereitstellen nicht aktualler Serverinhalte?

Wenn die Caching-Zeit für einen Inhalt abläuft, versucht der Edge-Server, den Inhalt vom Ursprungsserver abzurufen. Ist der Ursprungsserer aus irgendeinem Grund nicht betriebsbereit oder nicht erreichbar, kann der Edge-Server den nicht aktuellen Inhalt bereitstellen, wenn diese Option aktiviert ist. Diese Option wird für die meisten Server bevorzugt. Unsere CDN können derzeit nur mit dem Standardwert **Yes** für diese Option konfiguriert werden. Das Ändern dieser Option in **No** bleibt wirkungslos, da die Option für nicht aktuelle Serverinhalte**** automatisch wieder auf **Yes** gesetzt wird.

## Gibt es einen Grenzwert für die Anzahl der Einträge für Ursprung und TTL?

Ja, der kombinierte Grenzwert sind 75 Einträge pro CDN.

## Gibt es einen Grenzwert für die Anzahl der CDN, die ich verwenden kann?

Ja, Sie können maximal 10 CDN pro Konto verwenden.

## Gibt es empfohlene Browser für die CDN-Servicekonfiguration?

Ja, Firefox und Chrome (jeweils in der aktuellen Version) sind die empfohlenen Browser.

## Aus welchem Grund wird beim Erstellen des CDN ein Pfad angegeben?

Wenn Sie bei der CDN-Erstellung einen Pfad angeben, können Sie die Dateien isolieren, die über CDN aus einem bestimmten Ursprungsserver bereitgestellt werden.

## Woran erkenne ich, dass mein CDN funktioniert?
Führen Sie den folgenden Befehl 'curl' aus und ersetzen Sie darin 'origin.cdntesting.net/assets/ibm_3d.gif' durch den entsprechenden Dateipfad auf Ihrem Ursprungsserver: 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
Wenn die Ausgabe des Befehls das folgende Format aufweist, funktioniert CDN erwartungsgemäß:
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## Mein CDN weist einen Fehlerstatus auf. Was kann ich tun?
Lesen Sie die Informationen auf der Seite ['Hilfe und Unterstützung anfordern'](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp) oder öffnen Sie ein Support-Ticket im [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/).

## Wo finde ich 'CName', wenn ich keinen solchen Namen angegeben habe? 
Klicken Sie auf das CDN, um die Seite **Übersicht** zu öffnen. In der rechten Ecke finden Sie einen Abschnitt **Details** mit den Informationen zu `CName`.

## Gibt es einen Grenzwert für die Anzahl der gleichzeitig aktiven Bereinigungsanforderungen?
Ja. Es können höchstens 5 Bereinigungsanforderungen gleichzeitig aktiv sein.

## Meine Bereinigungsanforderung für einen angegebenen Dateipfad ist in Bearbeitung. Kann ich eine neue Anforderung für denselben Dateipfad abschicken?
Nein. Für einen bestimmten Dateipfad darf jeweils nur eine Bereinigungsanforderung aktiv sein.
