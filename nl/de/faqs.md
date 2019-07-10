---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# Häufig gestellte Fragen (FAQs)
{: #faqs}

## Was ist ein Content Delivery Network (CDN)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Content Delivery Network (CDN) ist ein Verbund von Edge-Servern, die über verschiedene Teile eines Landes oder der Welt verteilt sind. Die zugehörigen Webinhalte werden von einem Edge-Server bereitgestellt, dessen geografische Region dem Kunden, der die Inhalte anfordert, am nächsten liegt. Dieses Verfahren ermöglicht eine schnellere Zustellung der Inhalte als bei der Bereitstellung an einem zentralen Standort. Dies führt zu einer besserer Serviceerfahrung für Ihre Kunden.

## Wie funktioniert ein Content Delivery Network (CDN)?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

CDN erreicht sein Ziel durch Caching von Webinhalten auf Edge-Servern rund um die Welt. Wenn ein Benutzer Webinhalte anfordert, wird die Anfrage an den Edge-Server weitergeleitet, dessen geografischer Standort dem Benutzer am nächsten liegt. Durch das Verkürzen des Übertragungswegs für die angeforderten Inhalte, bietet das CDN optimierten Durchsatz, minimierte Latenzzeiten und ein besseres Leistungsverhalten.

## Wie wird das Konto für den IBM Cloud Content Delivery Network-Service erstellt?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

Ihr Konto wird während des CDN-Bestellprozesses erstellt. Wenn Sie ein CDN über das traditionelle Portal erstellen, wird Ihr Konto erstellt, wenn Sie unter **Netz -> Seite 'CDN'** auf die Schaltfläche **CDN bestellen** klicken. Wenn Sie ein CDN über das {{site.data.keyword.cloud}}-Portal erstellen, wird Ihr Konto erstellt, wenn Sie auf der Seite **Katalog -> Netz -> Content Delivery Network** auf die Schaltfläche **Erstellen** klicken.

## Was kann ich tun, wenn mein CDN den Status 'CNAME-Konfiguration' aufweist?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

Aktualisieren Sie für ein HTTPS-CDN Ihren DNS-Eintrag so, dass Ihre Website auf den Wert für `CNAME` verweist, der Ihrer neuen CDN-Zuordnung zugeordnet ist. Für ein CDN, das auf HTTPS mit Wildcard-Zertifikat basiert, ist diese DNS-Aktualisierung **NICHT** erforderlich.

**Hinweis**: Es kann 15 bis 30 Minuten dauern, bis die Aktualisierung wirksam wird. Eine genaue Zeitschätzung können Sie von Ihrem DNS-Provider erhalten.

## Wie füge ich einen CNAME-Datensatz für meine CDN-Domäne in DNS hinzu?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

Auf der DNS-Konfigurationsseite für Ihre CDN-Domäne können Sie einen CNAME-Datensatz erstellen, der den CDN-Domänennamen als Host und den IBM CNAME, den Sie zum Konfigurieren des CDN verwendet haben, als CNAME enthält. Der IBM CNAME endet mit `cdnedge.bluemix.net`.

Ein typischer CNAME-Datensatz auf der Seite für die DNS-Konfiguration weist folgendes Format auf:

| **Ressourcentyp** | **Host** | **Verweist auf (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 Minuten |


## Was wird mir in meinem CDN in Rechnung gestellt?
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

Sie erhalten nur eine Rechnung über die Bandbreite, die pro IBM Cloud Content Delivery Network-Instanz genutzt wird. Wenn Ihr CDN keine Bandbreite nutzt, fallen keine Gebühren an. Der Preis für die Bandbreite variiert je nach regionalem Standort des Edge-Servers. Die Preisstruktur für die Bandbreite nach geografischer Region finden Sie im [Preisstrukturdokument](/docs/infrastructure/CDN?topic=CDN-pricing) für diesen Service.

## Wann erhalte ich die Rechnung für mein CDN?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

Die Rechnungsstellung für IBM Cloud Content Delivery Network erfolgt gemäß dem Abrechnungszeitraum, der in Ihrem {{site.data.keyword.BluSoftlayer_notm}}-Konto festgelegt ist.

## Wird mein Konto gelöscht, wenn ich die Option `Löschen` im Überlaufmenü des CDN auswähle?
{: faq}

Nein, das betreffende CDN wird nicht gelöscht. Ihr Konto ist weiterhin vorhanden und Sie können weitere CDN-Instanzen erstellen.

## Werden beim Inhalts-Caching Push- oder Pull-Operationen verwendet?
{: #does-content-caching-use-push-or-pull}
{: faq}

Für das Caching von Inhalten wird ein Modell _Extrahieren aus Quelle_ verwendet. Bei diesem Modell extrahiert der Edge-Server durch eine Pull-Operation Daten aus dem Ursprungsserver, d. h. die Inhalte werden nicht manuell auf den Edge-Server hochgeladen.

## Gibt es empfohlene Browser für die CDN-Servicekonfiguration?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Ja, Firefox und Chrome sind die empfohlenen Browser. Es wird empfohlen, jeweils die aktuellsten Versionen der Browser mit IBM Cloud Content Delivery Network zu verwenden.

## Wozu dient die Angabe eines Pfads beim Erstellen eines CDN?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

Wenn Sie einen Pfad beim Erstellen Ihres CDN angeben, können Sie dadurch die Dateien isolieren, die über das CDN von einem bestimmten Ursprungsserver zugestellt werden.

## Mein CDN weist einen Fehlerstatus auf. Was kann ich tun?
{: faq}

Informationen finden Sie auf den Seiten [Fehlerbehebung](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) oder [Hilfe und Support anfordern](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp); alternativ können Sie ein Ticket im [Kundenportal![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) öffnen.

## Wo finde ich den CNAME für mein CDN, wenn ich keinen CNAME bereitgestellt habe?
{: faq}

Klicken Sie auf Ihr CDN, um auf die Seite **Übersicht** im Portal zuzugreifen. In der rechten Ecke finden Sie einen Abschnitt **Details** mit den Informationen zu `CName`.

## Meine Bereinigungsanforderung für einen angegebenen Dateipfad ist in Bearbeitung. Kann ich eine neue Anforderung für denselben Dateipfad abschicken?

Nein. Für einen bestimmten Dateipfad darf jeweils nur eine Bereinigungsanforderung aktiv sein.

## Wird Internet Protocol Version 6 (IPv6) mit dem IBM Cloud Content Delivery Network-Service unterstützt? Wie funktioniert es?
{: faq}

IPv6 (oder Dual Stack-Unterstützung) wird von den Edge-Servern von Akamai unterstützt. Es ist so konzipiert, dass Kunden mit ausschließlichem IPv4-Ursprung Verbindungen von IPv6-Clients akzeptieren, von IPv6 in IPv4 in der Peripherie konvertieren und mit IPv4 den Ursprung erreichen können.

**Hinweis:** Das Erstellen eines IBM Cloud CDN mit einer IPv6-Adresse als Adresse des Ursprungsservers wird nicht unterstützt.

## Gibt es Einschränkungen für zulässige HTTP- und HTTPS-Portnummern für Akamai?
{: faq}

Ja. Für den Akamai-Anbieter sind nur die folgenden Portnummern zulässig:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 und 45002.

## Welche URL soll für den Zugriff auf die Daten im Rahmen des CDN oder Ursprungspfads verwendet werden?
{: faq}

Der Pfad für eine CDN-Zuordnung oder für den Ursprung wird als Verzeichnis behandelt. Benutzer, die versuchen, auf den Ursprungspfad zuzugreifen, sollten ihn daher als Verzeichnis (mit einem Schrägstrich) verwenden. Wenn beispielsweise das CDN `www.example.com` mit dem Pfad erstellt wird, der das Verzeichnis `/images` enthält, sollte die URL zum Erreichen dieses CDN `www.example.com/images/` lauten.

Durch das Auslassen des Schrägstrichs (wie z. B. bei `www.example.com/images`) kommt es zu einem Fehler des Typs **Seite nicht gefunden**.

## Wie wird das Content Delivery Network für IBM Cloud Object Storage (COS) eingerichtet?
{: faq}

[Hier finden Sie ein Lernprogramm](/docs/tutorials?topic=solution-tutorials-static-files-cdn) zum Erstellen eines Content Delivery Network (CDN) für IBM Cloud Object Storage.

## Ich habe eine Benachrichtigung empfangen, dass mein Ursprungszertifikat abläuft. Was kann ich tun?
{: faq}

Führen Sie die in [diesem Artikel ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://community.akamai.com/docs/DOC-7708) von Akamai beschriebenen Schritte aus.

## Welche Sicherheit ist in der IBM CDN-Lösung mit Akamai enthalten?
{: faq}

Die Verwendung der verteilten Akamai-Plattform bietet Ihnen konkurrenzlose Skalierbarkeit und Ausfallsicherheit durch Tausende von Servern in mehr als 50 Ländern. Die Akamai-Plattform ist zwischen Ihrer Infrastruktur und Ihren Endbenutzern angesiedelt und fungiert als erste Auffangebene für plötzliche Bedarfsspitzen im Datenverkehr. Akamai Intelligent Platform dient zugleich als Reverse Proxy, der für Anforderungen nur über die Ports 80 und 443 empfangsbereit ist und auf diese Anforderungen reagiert, sodass Datenverkehr über andere Ports bereits an der Peripherie gelöscht und nicht an Ihre Infrastruktur weitergeleitet wird.

## Werden Ursprungsservercookies in der Akamai-CDN-Instanz beibehalten?
{: faq}

Für nicht zwischenspeicherbare Inhalte bzw. Inhalte, die nicht zwischengespeichert werden, werden Cookies des Ursprungsservers beibehalten. Für von Edge-Servern zwischengespeicherte Inhalte werden keine Cookies beibehalten.

## Wie kann ich anderen Benutzern über die IBM Cloud-Konsole die Berechtigung zum Erstellen und Verwalten eines CDN erteilen?
{: faq}

Der Masterbenutzer des Kontos kann anderen Benutzern die Berechtigung zum Erstellen oder Verwalten eines CDN erteilen.

Führen Sie in der Hauptseite der IBM Cloud-Konsole zum Bearbeiten von Berechtigungen die folgenden Schritte aus:
 * Wählen Sie die Registerkarte **Verwalten** aus.
 * Wählen Sie **Zugriff (IAM)** aus.
 * Klicken Sie im linken Fensterbereich auf die Registerkarte **Benutzer**.
 * Klicken Sie auf den gewünschten **Benutzer**.
 * Wählen Sie anschließend die Registerkarte **Klassische Infrastruktur** aus.
 * Erweitern Sie dann auf der Registerkarte **Berechtigungen** die Kategorie **Services**.
 * Wählen Sie **CDN-Konto verwalten** aus.
 * Klicken Sie auf die Schaltfläche **Speichern**.

Führen Sie auf der Hauptseite der traditionellen Konsole die folgenden Schritte aus, um Berechtigungen zu bearbeiten:
 * Wählen Sie die Registerkarte **Konto** aus.
 * Wählen Sie **Benutzer -> Benutzerliste** aus.
 * Klicken Sie in der Rubrik **Benutzername** auf den gewünschten Namen.
 * Wählen Sie anschließend die Registerkarte **Portalberechtigungen** aus.
 * Wählen Sie die Registerkarte **Services** aus.
 * Wählen Sie **CDN-Konto verwalten** aus.
 * Klicken Sie auf die Schaltfläche **Portalberechtigungen bearbeiten**.

## Warum ist die Schaltfläche 'Erstellen' auf der Seite 'https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai' inaktiviert oder wird gar nicht angezeigt?
{: faq}

Wenn Sie der Masterbenutzer des Kontos sind, müssen Sie ein Upgrade für das Konto durchführen, damit die Schaltfläche 'Erstellen' auf der Seite angezeigt bzw. aktiviert wird. Führen Sie auf der IBM Cloud-Konsolenseite diese Schritte als Masterbenutzer des Kontos aus:
 * Öffnen Sie den linken Navigationsbereich, indem Sie auf das Symbol mit den `drei Balken` in der linken oberen Ecke der Webseite klicken.
 * Wählen Sie **Klassische Infrastruktur** aus.
 * Klicken Sie auf die Schaltfläche **Upgrade für Konto durchführen** und gehen Sie den Anweisungen entsprechend vor.

Wenn Sie einer der sekundären Benutzer des Kontos sind, muss Ihnen der Masterbenutzer des Kontos die Berechtigung für die Aktion `Services hinzufügen/Upgrades für Services durchführen` erteilen, damit die Schaltfläche 'Erstellen' auf der Seite angezeigt bzw. aktiviert wird. Der Masterbenutzer des Kontos muss auf der IBM Cloud-Konsolenseite diese Schritte ausführen, um Ihre Berechtigungen zu bearbeiten:
 * Wählen Sie die Registerkarte **Verwalten** aus.
 * Wählen Sie **Zugriff (IAM)** aus.
 * Klicken Sie im linken Fensterbereich auf die Registerkarte **Benutzer**.
 * Klicken Sie auf den gewünschten **Benutzer**.
 * Wählen Sie anschließend die Registerkarte **Klassische Infrastruktur** aus.
 * Erweitern Sie dann auf der Registerkarte **Berechtigungen** die Kategorie **Konto**.
 * Wählen Sie **Services hinzufügen/Upgrade für Services durchführen** aus.
 * Klicken Sie auf die Schaltfläche **Speichern**.

## Warum kann ich meine Webseite nicht über mein CDN erreichen, nachdem ich Hot-Link-Schutz mit `protectionType` `ALLOW` konfiguriert habe?
{: faq}

Lassen Sie uns ein Beispiel betrachten, bei dem die Domäne Ihrer Website für Endbenutzer als Domänen-/Hostname Ihres CDN konfiguriert ist: `cdn.example.com`. Wenn ein Benutzer versucht, eine Webseite durch direkte Navigation über die Navigationsleiste des Browsers zu erreichen, sendet der Browser in der Regel keine Referer-Header in seiner HTTP-Anforderung. Wenn Sie beispielsweise versuchen, auf diese Weise direkt zu `https://cdn.example.com/` zu navigieren, geht Ihr CDN davon aus, dass die Anforderung eine Nichtübereinstimmung mit den angegebenen `refererValues` enthält. Wenn das CDN den entsprechenden Effekt oder die entsprechende Antwort über den Hot-Link-Schutz auswertet, wird festgestellt, dass eine Nichtübereinstimmung aufgetreten ist. Daher verweigert Ihr CDN den Zugriff, obwohl 'ALLOW' konfiguriert ist.

## Kann ich den privaten Endpunkt des Objektspeichers in den CDN-Einstellungen verwenden?
{: faq}

Nein, CDN kann nur eine Verbindung zum Objektspeicher für öffentliche Endpunkte herstellen.

## Kann ich die Brotli-Funktion im CDN-Service verwenden?
{: faq}

Nein, die Brotli-Funktion wird von unserem CDN-Service mit Akamai nicht unterstützt.

## Wie erstelle ich einen CDN-Endpunkt ohne die Verwendung der Domain?
{: faq}

Sie können einen CDN-Endpunkt ohne die Verwendung der Domäne erstellen, aber nur für einen CDN des Typs **Wildcard HTTPS**. Beim Erstellen eines CDN vom Typ **Wildcard HTTPS**fungiert Ihr **CNAME** als CDN-Endpunkt. Der **CNAME** wird verwendet, um den Datenverkehr zu bedienen.
