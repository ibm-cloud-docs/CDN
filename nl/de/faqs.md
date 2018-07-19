---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

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

Sie erhalten nur eine Rechnung über die Bandbreite, die pro IBM Cloud Content Delivery Network-Instanz genutzt wird. Wenn Ihr CDN keine Bandbreite nutzt, fallen keine Gebühren an. Der Preis für die Bandbreite variiert je nach regionalem Standort des Edge-Servers. Die Preisgestaltung für die Bandbreite nach geografischer Region finden Sie im Abschnitt [Einführung](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-) für diesen Service.

## Wann erfolgt die Abrechnung?

Die Rechnungsstellung für IBM Cloud Content Delivery Network erfolgt gemäß dem Abrechnungszeitraum, der in Ihrem {{site.data.keyword.BluSoftlayer_notm}}-Konto festgelegt ist.

## Wird mein Konto gelöscht, wenn ich die Option `Löschen` im Überlaufmenü des CDN auswähle?

Nein, das betreffende CDN wird nicht gelöscht. Ihr Konto ist weiterhin vorhanden und Sie können weitere CDN-Instanzen erstellen.

## Werden beim Caching Push- oder Pull-Operationen verwendet?

Für das Caching von Inhalten wird ein Modell _Extrahieren aus Quelle_ verwendet. Bei diesem Modell extrahiert der Edge-Server durch eine Pull-Operation Daten aus dem Ursprungsserver, d. h. die Inhalte werden nicht manuell auf den Edge-Server hochgeladen.

## Gibt es empfohlene Browser für die CDN-Servicekonfiguration?

Ja, Firefox und Chrome sind die empfohlenen Browser. Es wird empfohlen, jeweils die aktuellsten Versionen der Browser mit IBM Cloud Content Delivery Network zu verwenden.

## Wozu dient die Angabe eines Pfads beim Erstellen eines CDN?

Wenn Sie einen Pfad beim Erstellen Ihres CDN angeben, können Sie dadurch die Dateien isolieren, die über das CDN von einem bestimmten Ursprungsserver zugestellt werden.

## Mein CDN weist einen Fehlerstatus auf. Was kann ich tun?

Informationen finden Sie auf den Seiten [Fehlerbehebung](troubleshooting.html#troubleshooting) oder [Hilfe und Support anfordern](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#getting-help); alternativ können Sie ein Ticket im [Kundenportal![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) öffnen.

## Wo finde ich meinen CNAME, wenn ich keinen angegeben habe?

Klicken Sie auf Ihr CDN, um auf die Seite **Übersicht** im Portal zuzugreifen. In der rechten Ecke finden Sie einen Abschnitt **Details** mit den Informationen zu `CName`.

## Meine Bereinigungsanforderung für einen angegebenen Dateipfad ist in Bearbeitung. Kann ich eine neue Anforderung für denselben Dateipfad abschicken?

Nein. Für einen bestimmten Dateipfad darf jeweils nur eine Bereinigungsanforderung aktiv sein.

## Wird Internet Protocol Version 6 (IPv6) mit dem IBM Cloud Content Delivery Network-Service unterstützt? Wie funktioniert es?

IPv6 (oder Dual Stack-Unterstützung) wird von den Edge-Servern von Akamai unterstützt. Es ist so konzipiert, dass Kunden mit ausschließlichem IPv4-Ursprung Verbindungen von IPv6-Clients akzeptieren, von IPv6 in IPv4 in der Peripherie konvertieren und mit IPv4 den Ursprung erreichen können.

**Hinweis:** Das Erstellen eines IBM Cloud CDN mit einer IPv6-Adresse als Adresse des Ursprungsservers wird nicht unterstützt.

## Gibt es Einschränkungen für zulässige HTTP- und HTTPS-Portnummern für Akamai?

Ja. Für den Akamai-Anbieter sind nur die folgenden Portnummern zulässig:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 und 45002.

## Welche URL soll für den Zugriff auf die Daten im Rahmen des CDN oder Ursprungspfads verwendet werden?
Der Pfad für eine CDN-Zuordnung oder für den Ursprung wird als Verzeichnis behandelt. Benutzer, die versuchen, auf den Ursprungspfad zuzugreifen, sollten ihn daher als Verzeichnis (mit einem Schrägstrich) verwenden. Wenn beispielsweise das CDN `www.example.com` mit dem Pfad erstellt wird, der das Verzeichnis `/images` enthält, sollte die URL zum Erreichen dieses CDN `www.example.com/images/` lauten.

Durch das Auslassen des Schrägstrichs (wie z. B. bei `www.example.com/images`) kommt es zu einem Fehler des Typs **Seite nicht gefunden**.

## Wie wird das Content Delivery Network für IBM Cloud Object Storage (COS) eingerichtet?

[Hier finden Sie ein Lernprogramm](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) zum Erstellen eines Content Delivery Network (CDN) für IBM Cloud Object Storage.

## Ich habe eine Benachrichtigung empfangen, dass mein Ursprungszertifikat abläuft. Was kann ich tun?

Führen Sie die Schritte aus, die in [diesem Artikel](https://community.akamai.com/docs/DOC-7708) bei Akamai beschrieben werden.

## Welche Sicherheit ist in der IBM CDN-Lösung mit Akamai enthalten?

Die Verwendung der verteilten Akamai-Plattform bietet Ihnen konkurrenzlose Skalierbarkeit und Ausfallsicherheit durch mehr als 240.000 Servern in 130 Ländern. Die Akamai-Plattform ist zwischen Ihrer Infrastruktur und Ihren Endbenutzern angesiedelt und fungiert als erste Auffangebene für plötzliche Bedarfsspitzen im Datenverkehr. Akamai Intelligent Platform dient zugleich als Reverse Proxy, der für Anforderungen nur über die Ports 80 und 443 empfangsbereit ist und auf diese Anforderungen reagiert, sodass Datenverkehr über andere Ports bereits an der Peripherie gelöscht und nicht an Ihre Infrastruktur weitergeleitet wird.
