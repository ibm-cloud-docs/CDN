---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Häufig gestellte Fragen (FAQs)

## Was ist ein Content Delivery Network (CDN)?

Content Delivery Network (CDN) ist ein Verbund von Edge-Servern, die über verschiedene Teile eines Landes oder der Welt verteilt sind. Die zugehörigen Webinhalte werden von einem Server bereitgestellt, dessen geografische Region dem Kunden, der die Inhalte anfordert, am nächsten liegt. Dieses Verfahren ermöglicht eine schnellere Zustellung der Inhalte als bei der Bereitstellung an einem zentralen Standort. Dies führt zu einer besserer Serviceerfahrung für Ihre Kunden.

## Wie funktioniert ein Content Delivery Network (CDN)?

CDN erreicht sein Ziel durch Caching von Webinhalten auf Edge-Servern rund um die Welt. Wenn ein Benutzer Webinhalte anfordert, wird die Anfrage an den Edge-Server weitergeleitet, dessen geografischer Standort dem Benutzer am nächsten liegt. Durch das Verkürzen des Übertragungswegs für die angeforderten Inhalte, bietet das CDN optimierten Durchsatz, minimierte Latenzzeiten und ein besseres Leistungsverhalten. 

## Wie wird das Konto für den IBM Cloud Content Delivery Network-Service erstellt?

Ihr Konto wird während des CDN-Bestellprozesses erstellt, wenn Sie auf **Auswählen** klicken, nachdem Sie durch das Menü für die Anbieterauswahl navigiert haben.

## Was kann ich tun, wenn mein CDN den Status CNAME_CONFIGURATION aufweist?

Aktualisieren Sie für ein HTTP-basiertes CDN Ihren DNS-Eintrag so, dass Ihre Website auf den `CNAME` verweist, der Ihrer neuen CDN-Zuordnung zugeordnet ist. Für ein HTTPS-basiertes CDN ist diese DNS-Aktualisierung **nicht** erforderlich.

**Hinweis**: Es kann 15 bis 30 Minuten dauern, bis die Aktualisierung wirksam wird. Eine genaue Zeitschätzung können Sie von Ihrem DNS-Provider erhalten.

## Was wird mir in Rechnung gestellt?

Sie erhalten nur eine Rechnung über die Bandbreite, die pro IBM Cloud Content Delivery Network-Instanz genutzt wird. Wenn Ihr CDN keine Bandbreite nutzt, fallen keine Gebühren an. Der Preis für die Bandbreite variiert je nach regionalem Standort des Edge-Servers. Die Preisgestaltung für die Bandbreite nach geografischer Region finden Sie im Abschnitt [Einführung](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd) für diesen Service.

## Wann erfolgt die Abrechnung?

Die Rechnungsstellung für IBM Cloud Content Delivery Network erfolgt gemäß dem Abrechnungszeitraum, der in Ihrem {{site.data.keyword.BluSoftlayer_notm}}-Konto festgelegt ist.

## Wie kann ich die Metriken und die Nutzung anzeigen?

Die Metriken und die Nutzung können auf der Seite **Übersicht** angezeigt werden, die durch das Auswählen Ihres CDN auf der Seite **Content Delivery Networks** aufgerufen werden kann. **Hinweis:** Nach dem Erstellen eines CDN kann es bis zu 48 Stunden dauern, bevor die Metriken angezeigt werden.

## Ich habe ein CDN erstellt und es hat Datenverkehr über das CDN stattgefunden. Warum werden meine Metriken nicht angezeigt?

Nach der Erstellung eines CDN dauert es 48 Stunden, bis die Metriken angezeigt werden.

## Gibt es eine Mindestanzahl von Tagen, für die ich Metriken anzeigen kann? Gibt es einen Maximalwert?

Es gibt eine minimale und eine maximale Anzahl von Tagen, für die Sie Metriken mit IBM Cloud Content Delivery Network und Akamai anzeigen können. Metriken können für einen Mindestzeitraum von 7 Tagen erfasst werden. Der maximale Zeitraum, für den Metriken angezeigt werden können, beträgt 90 Tage. Falls Sie die API verwenden, werden 89 Tage als Maximalwert empfohlen, um Zeitzonenabweichungen zu berücksichtigen.

## Warum ist die Trefferquote ungleich null, wenn die Gesamtanzahl der Treffer Null ist?
Die Trefferquote stellt den Prozentsatz der Male dar, die Inhalte anstatt aus dem Ursprungsserver aus dem Edge-Server-Cache bereitgestellt wurden. Sie wird wie folgt berechnet:

```
((Edge-Treffer - Eingangstreffer)/Edge-Treffer) * 100

Dabei gilt:
Edge-Treffer werden als "Alle Treffer für Edge-Server von den Endbenutzern" und Eingangstreffer als "Ursprungs- oder Eingangstreffer für den Datenverkehr vom Ursprung zu den Akamai-Edge-Servern" bezeichnet
```
 
Da die Trefferquote nicht pro CDN, sondern auf Kontoebene berechnet wird, ist die Trefferquote für alle CDNs in Ihrem Konto gleich. Diese Tatsache erklärt auch, warum die Trefferrate ungleich null sein kann, wenn die Anzahl der Edge-Treffer für einen bestimmten CDN Null ist.

## Wird mein Konto gelöscht, wenn ich die Option `Löschen` im Überlaufmenü des CDN auswähle?

Nein, das betreffende CDN wird nicht gelöscht. Ihr Konto ist weiterhin vorhanden und Sie können weitere CDN-Instanzen erstellen.

## Werden beim Caching Push- oder Pull-Operationen verwendet?

Für das Caching von Inhalten wird ein Modell _Extrahieren aus Quelle_ verwendet. Bei diesem Modell extrahiert der Edge-Server durch eine Pull-Operation Daten aus dem Ursprungsserver, d. h. die Inhalte werden nicht manuell auf den Edge-Server hochgeladen. 

## Gibt es einen Maximalwert für die Lebensdauer? Gibt es einen Mindestwert?

Der Maximalwert für die Lebensdauer (Time To Live, TTL) sind 2.147.483.647 Sekunden. Dies entspricht ungefähr 68 Jahren. Der Mindestwert sind 30 Sekunden.

## Gibt es einen Grenzwert für die Anzahl der Einträge für Ursprung und TTL?

Ja, der kombinierte Grenzwert sind 75 Einträge pro CDN.

## Gibt es einen Grenzwert für die Anzahl der CDN, die ich verwenden kann?

Ja, Sie können maximal 10 CDN pro Konto verwenden.

## Gibt es empfohlene Browser für die CDN-Servicekonfiguration?

Ja, Firefox und Chrome sind die empfohlenen Browser. Es wird empfohlen, jeweils die aktuellsten Versionen der Browser mit IBM Cloud Content Delivery Network zu verwenden.

## Wozu dient die Angabe eines Pfads beim Erstellen eines CDN?

Wenn Sie einen Pfad beim Erstellen Ihres CDN angeben, können Sie dadurch die Dateien isolieren, die über das CDN von einem bestimmten Ursprungsserver zugestellt werden.

## Woran erkenne ich, dass mein CDN funktioniert?
Führen Sie den folgenden 'curl'-Befehl aus, indem Sie "origin.cdntesting.net/assets/ibm_3d.gif" durch den entsprechenden Dateipfad Ihres Ursprungsservers ersetzen: 
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

## Wo finde ich meinen CNAME, wenn ich keinen angegeben habe? 
Klicken Sie auf Ihr CDN, um auf die Seite **Übersicht** im Portal zuzugreifen. In der rechten Ecke finden Sie einen Abschnitt **Details** mit den Informationen zu `CName`.

## Gibt es einen Grenzwert für die Anzahl der gleichzeitig aktiven Bereinigungsanforderungen?
Ja. Es können höchstens 5 Bereinigungsanforderungen gleichzeitig aktiv sein.

## Meine Bereinigungsanforderung für einen angegebenen Dateipfad ist in Bearbeitung. Kann ich eine neue Anforderung für denselben Dateipfad abschicken?
Nein. Für einen bestimmten Dateipfad darf jeweils nur eine Bereinigungsanforderung aktiv sein.

## Wird Internet Protocol Version 6 (IPv6) mit dem IBM Cloud Content Delivery Network-Service unterstützt? Wie funktioniert es?
IPv6 (oder Dual Stack-Unterstützung) wird von den Edge-Servern von Akamai unterstützt. Es ist so konzipiert, dass Kunden mit ausschließlichem IPv4-Ursprung Verbindungen von IPv6-Clients akzeptieren, von IPv6 in IPv4 in der Peripherie konvertieren und mit IPv4 den Ursprung erreichen können.

**Hinweis:** Das Erstellen eines IBM Cloud CDN mit einer IPv6-Adresse als Adresse des Ursprungsservers wird nicht unterstützt.

## Welche Regeln gelten für den Hostnamen?
Die Eingabezeichenfolge für `Hostname` **muss** die folgenden Voraussetzungen erfüllen:
  * Sie muss aus alphanumerischen Zeichen bestehen.
  * Sie muss kürzer als 254 Zeichen sein.
  * Sie muss auf einen gültigen Domänennamen der höchsten Ebene enden.
  * Sie darf **nicht** auf 'cdnedge.bluemix.net` enden. (Diese Endung wird für CNAMES verwendet und ist reserviert.)

Weitere Details finden Sie in RFC 1035, Abschnitt 2.3.4.

## Welche angepassten Konventionen gelten für die CNAME-Benennung?
Die Eingabezeichenfolge für `CNAME` muss die folgenden Regeln einhalten:
  * Sie **muss** eindeutig sein (d. h. sie kann nicht von einem anderen IBM Cloud CDN verwendet werden).
  * Sie muss kürzer als 64 alphanumerische Zeichen sein.
  * Sie Sonderzeichen `! @ # $ % ^ & *` sind **nicht** zulässig.
  * Unterstreichungszeichen (`_`) sind **nicht** zulässig.
  * Punkte (`.`) sind **nicht** zulässig.
  * Gedankenstriche (`-`) sind zulässig, jedoch kann der CNAME nicht mit einem Gedankenstrich beginnen oder enden.

## Welche Regeln gelten für Bucketnamen?
Die folgenden Regeln gelten für Bucketnamen:
  * Sie müssen mindestens 1 Zeichen lang sein.
  * Sie können nicht mehr als 199 Zeichen lang sein.
  * Sie können Buchstaben (Groß- und Kleinbuchstaben sind zulässig), Ziffern und Bindestriche enthalten.
  * Sie dürfen **nicht** als IP-Adresse formatiert werden (z. B. 127.0.0.1). 
  * Sie können **nicht** mit einem Punkt (.) beginnen.
  * Sie können auf einen Punkt (.) enden.
  * Zwischen Bezeichnungen ist jeweils nur ein Punkt (.) zulässig (z. B. ist 'my..ibmcloud.bucket' nicht zulässig). 

**Hinweis:** Obwohl Großbuchstaben und Punkte die Gültigkeitsprüfung bestehen können, wird empfohlen, DNS-konforme Bucketnamen zu verwenden.

## Welche Regeln gelten für die Pfadzeichenfolge für den Ursprung?
Der Pfad ist beim Erstellen des CDN optional. Wenn er angegeben wird, **muss** der Pfad folgenden Regeln genügen:
  * Er muss kürzer als 1000 Zeichen lang sein.
  * Er muss mit '/' beginnen.

## Gibt des für den Befehl '**Ursprung hinzufügen**' Regeln, die für die Pfadzeichenfolge beachtet werden müssen?
Für **Ursprung hinzufügen** ist der Pfad obligatorisch. Darüber hinaus gilt, dass der Pfad für den Befehl **Ursprung hinzufügen**, wenn Ihr CDN mit einem Pfad erstellt wurde, mit dem CDN-Pfad als Präfix beginnen muss. Wenn als CDN-Pfad beispielsweise `/storage` angegeben ist, würde zum Aufrufen von **Ursprung hinzufügen** mit dem Pfad `/examplePath` der Pfad `/storage/examplePath` angegeben.

## In welchem Format müssen die Dateierweiterungen zum Erstellen eines Objektspeicherursprungstyps für diesen CDN-Service angegeben werden?

Dateierweiterungen sollten durch Kommas voneinander getrennt sein. Beispielsweise ist die Liste 'txt, jpg, xml' eine gültige Liste. Eine Erweiterung darf nicht länger als 10 Zeichen lang sein.

## Gibt es Einschränkungen für zulässige HTTP- und HTTPS-Portnummern für Akamai?
Ja. Für den Akamai-Anbieter sind nur die folgenden Portnummern zulässig:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 und 45002.

## Welche URL soll für den Zugriff auf die Daten im Rahmen des CDN oder Ursprungspfads verwendet werden? 
Der Pfad für eine CDN-Zuordnung oder für den Ursprung wird als Verzeichnis behandelt. Benutzer, die versuchen, auf den Ursprungspfad zuzugreifen, sollten ihn daher als Verzeichnis (mit einem Schrägstrich) verwenden. Wenn beispielsweise das CDN `www.example.com` mit dem Pfad erstellt wird, der das Verzeichnis `/images` enthält, sollte die URL zum Erreichen dieses CDN `www.example.com/images/` lauten.

Durch das Auslassen des Schrägstrichs (wie z. B. bei `www.example.com/images`) kommt es zu einem Fehler des Typs **Seite nicht gefunden**.

## Warum kann ich für HTTPS nicht mithilfe des Hostnamens über einen cURL-Befehl oder Browser eine Verbindung herstellen?

HTTPS wird derzeit nur über das Platzhalterzertifikat unterstützt. Als Ergebnis dieser Einschränkung muss die Verbindung mit CNAME hergestellt werden; das Herstellen einer Verbindung mit dem Hostnamen führt zu einem Fehler.

## Welches Verhalten ist zu erwarten, wenn der CNAME oder der Hostname in Ihrem Browser für die unterstützten CDN-Protokolle geladen wird?

|Browser-URL| CDN nur mit HTTP-Protokoll | CDN nur mit HTTPS-Protokoll | CDN mit HTTP- und HTTPS-Protokoll |
|-------|-----|-----|-----|
|http://hostname| Laden erfolgreich | 301 Permanent verschoben | 301 Permanent verschoben |
|https://hostname | Zugriff verweigert | Umleitung an IBM Cloud Webpage | Umleitung an IBM Cloud Webpage | 
|http://cname| 301 Permanent verschoben | Zugriff verweigert | Laden erfolgreich | 
|https://cname| Umleitung an IBM Cloud Webpage | Laden erfolgreich | Laden erfolgreich |

## Wie wird das Content Delivery Network für IBM Cloud Object Storage (COS) eingerichtet?
[Hier finden Sie ein Lernprogramm](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) zum Erstellen eines Content Delivery Network (CDN) für IBM Cloud Object Storage.

## Warum wird mein Hostname nicht im Browser geladen, wenn IBM Cloud Object Storage (COS) als Ursprung ausgewählt wird?

Wenn Ihr IBM Cloud CDN zur Verwendung von IBM COS als Objektspeicher konfiguriert ist, funktioniert der direkte Zugriff auf die Website nicht. Sie müssen den vollständigen Anforderungspfad in der Adressleiste des Browsers angeben (Beispiel: `www.example.com/index.html`). Dieses Verhalten ist auf die Indexdokumenteinschränkung in IBM COS zurückzuführen.

## Was ist die größte Dateigröße, die über Akamai CDN zugestellt werden kann?

Versuche, Dateien mit Größen über 1,8 GB abzurufen oder zuzustellen, empfangen unter der Standardleistungskonfiguration eine Antwort `403 Zugriff nicht zulässig`. Wenn die Optimierung für große Dateien aktiviert ist, sind Dateidownloads bis zu 320 GB möglich.

## Ich habe eine Benachrichtigung empfangen, dass mein Ursprungszertifikat abläuft. Was kann ich tun?

Führen Sie die Schritte aus, die in [diesem Artikel](https://community.akamai.com/docs/DOC-7708) bei Akamai beschrieben werden.

## Was ist eine Bytebereichsanforderung und wie funktioniert sie mit Akamai CDN?

Eine Bytebereichsanforderung wird zum Abrufen von Teilinhalten von einem Ursprungsserver verwendet. Der HTTP-Anforderungsheader für die Bereichsanforderung gibt an, welcher Teil des Inhalts vom Server zurückgegeben werden soll. Es können mehrere Teile zusammen durch einen Bereichsheader angefordert werden und der Server kann diese Bereiche in einer mehrteiligen Antwort zurücksenden. Wenn der Server Bereiche zurücksendet, antwortet er mit dem Status 206 (Teilinhalt).

Wenn eine Bytebereichsanforderung durch IBM Cloud CDN mit Akamai gesendet wird, empfängt der Benutzer möglicherweise den Antwortcode 200 (OK) für die erste Anforderung und den Antwortcode 206 für alle nachfolgenden Anforderungen. Dies liegt daran, dass Akamai-Edge-Server Inhalte vom Ursprungsserver im komprimierten Format anfordern. Wenn also ein Edge-Server ein Objekt nicht in seinem Cache hat und auch keine Informationen zur Länge des Objektsinhalts hat, kontaktiert er den Ursprungsserver und fordert das gesamte Objekt an. In diesem Fall liefert der Ursprungsserver das Objekt ohne Header für Länge des Inhalts an Akamai und der Endbenutzer empfängt das gesamte Objekt, obwohl es sich um eine Bytebereichsanforderung handelt. Daher der Statuscode 200. Bei nachfolgenden Anforderungen hat der Edge-Server das Objekt in seinem Cache und stellt es mit dem Statuscode 206 zu.

Eine Möglichkeit, den Antwortcode 206 auch für die erste Bytebereichsanforderung sicherzustellen, besteht darin, die Option `Transfer-Encoding: chunked` auf Ihrem Ursprungsserver zu inaktivieren.

## Welche Sicherheit ist in der IBM CDN-Lösung mit Akamai enthalten?

Die Verwendung der verteilten Akamai-Plattform bietet Ihnen konkurrenzlose Skalierbarkeit und Ausfallsicherheit durch mehr als 240.000 Servern in 130 Ländern. Die Akamai-Plattform ist zwischen Ihrer Infrastruktur und Ihren Endbenutzern angesiedelt und fungiert als erste Auffangebene für plötzliche Bedarfsspitzen im Datenverkehr. Akamai Intelligent Platform dient zugleich als Reverse Proxy, der für Anforderungen nur über die Ports 80 und 443 empfangsbereit ist und auf diese Anforderungen reagiert, sodass Datenverkehr über andere Ports bereits an der Peripherie gelöscht und nicht an Ihre Infrastruktur weitergeleitet wird.
