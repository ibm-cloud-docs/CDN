---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-01"

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

Content Delivery Network (CDN) ist ein Verbund von Edge-Servern, die über verschiedene Teile eines Landes oder der Welt verteilt sind. Die zugehörigen Webinhalte werden von einem Edge-Server bereitgestellt, dessen geografische Region dem Kunden, der die Inhalte anfordert, am nächsten liegt. Dieses Verfahren ermöglicht eine schnellere Zustellung der Inhalte als bei der Bereitstellung an einem zentralen Standort. Dies führt zu einer besserer Serviceerfahrung für Ihre Kunden.

## Wie funktioniert ein Content Delivery Network (CDN)?

CDN erreicht sein Ziel durch Caching von Webinhalten auf Edge-Servern rund um die Welt. Wenn ein Benutzer Webinhalte anfordert, wird die Anfrage an den Edge-Server weitergeleitet, dessen geografischer Standort dem Benutzer am nächsten liegt. Durch das Verkürzen des Übertragungswegs für die angeforderten Inhalte, bietet das CDN optimierten Durchsatz, minimierte Latenzzeiten und ein besseres Leistungsverhalten.

## Wie wird das Konto für den IBM Cloud Content Delivery Network-Service erstellt?

Ihr Konto wird während des CDN-Bestellprozesses erstellt. Wenn Sie ein CDN über das traditionelle Portal erstellen, wird Ihr Konto erstellt, wenn Sie unter **Netz -> Seite 'CDN'** auf die Schaltfläche **CDN bestellen** klicken. Wenn Sie ein CDN über das IBM Cloud-Portal erstellen, wird Ihr Konto erstellt, wenn Sie auf der Seite **Katalog -> Netz -> Content Delivery Network** auf die Schaltfläche **Erstellen** klicken.

## Was kann ich tun, wenn mein CDN den Status 'CNAME-Konfiguration' aufweist?

Aktualisieren Sie für ein HTTPS-CDN Ihren DNS-Eintrag so, dass Ihre Website auf den Wert für `CNAME` verweist, der Ihrer neuen CDN-Zuordnung zugeordnet ist. Für ein CDN, das auf HTTPS mit Wildcard-Zertifikat basiert, ist diese DNS-Aktualisierung **NICHT** erforderlich.

**Hinweis**: Es kann 15 bis 30 Minuten dauern, bis die Aktualisierung wirksam wird. Eine genaue Zeitschätzung können Sie von Ihrem DNS-Provider erhalten.

Ein typischer CNAME-Datensatz auf der Seite für die DNS-Konfiguration weist folgendes Format auf:

| **Ressourcentyp** | **Host** | **Verweist auf (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 Minuten |


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

Informationen finden Sie auf den Seiten [Fehlerbehebung](troubleshooting.html#troubleshooting) oder [Hilfe und Support anfordern](getting-help.html#gettinghelp); alternativ können Sie ein Ticket im [Kundenportal![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) öffnen.

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

## Werden Ursprungsservercookies in der Akamai-CDN-Instanz beibehalten? 

Für nicht zwischenspeicherbare Inhalte bzw. Inhalte, die nicht zwischengespeichert werden, werden Cookies des Ursprungsservers beibehalten. Für von Edge-Servern zwischengespeicherte Inhalte werden keine Cookies beibehalten.

## Wie kann ich anderen Benutzern über die IBM Cloud-Konsole die Berechtigung zum Erstellen und Verwalten eines CDN erteilen?

Der Masterbenutzer des Kontos kann anderen Benutzern in der IBM Cloud-Konsole die Berechtigung zum Erstellen oder Verwalten eines CDN erteilen. Führen Sie in der Hauptseite der IBM Cloud-Konsole zum Bearbeiten von Berechtigungen die folgenden Schritte aus: 
 * Wählen Sie die Registerkarte **Konto** aus.
 * Wählen Sie **Benutzer -> Benutzerliste** aus.
 * Klicken Sie in der Rubrik **Benutzername** auf den gewünschten Namen.
 * Wählen Sie anschließend die Registerkarte **Portalberechtigungen** aus.
 * Wählen Sie die Registerkarte **Services** aus.
 * Wählen Sie **CDN-Konto verwalten** aus.
 * Klicken Sie auf die Schaltfläche **Portalberechtigungen bearbeiten**.
 * Legen Sie die erforderlichen Berechtigungen fest.

