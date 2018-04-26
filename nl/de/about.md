---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Informationen zu Content Delivery Networks (CDN)

Content Delivery Network (CDN) ist ein Verbund von Edge-Servern, die über verschiedene Teile eines Landes oder der Welt verteilt sind. Die zugehörigen Webinhalte werden von einem Server bereitgestellt, dessen geografische Region dem Kunden, der die Inhalte anfordert, am nächsten liegt. Dieses Verfahren minimiert die Verzögerung, mit der Ihre Endbenutzer den Inhalt empfangen, und verbessert die allgemeine Serviceerfahrung Ihrer Kunden.

## Funktionsweise von CDN

CDN erreicht sein Ziel durch Caching von Webinhalten auf Edge-Servern rund um die Welt. Wenn ein Kunde Webinhalte anfordert, wird die Inhaltsanforderung an den Edge-Server geleitet, dessen geografische Position dem betreffenden Kunden am nächsten liegt. Durch das Verkürzen des Übertragungswegs für die angeforderten Inhalte, bietet das CDN optimierten Durchsatz, minimierte Latenzzeiten und ein besseres Leistungsverhalten. Unter Verwendung von IBM Cloud Content Delivery Network mit Akamai können Content-Provider rund um den Globus eine effiziente Zustellung angeforderter Inhalte bei minimalem Konfigurationsaufwand realisieren.

Zu den wichtigsten Funktionen des IBM Cloud Content Delivery Network-Service gehören die folgenden:
  * Unterstützung für den Host-Serverursprung (Host Server Origin)
  * Unterstützung für den Objektspeicherursprung (Object Storage Origin)
  * Unterstützung für mehrere Ursprünge mit unterschiedlichen Pfaden
  * Pfadbasierte CDN-Zuordnungen
  * Cacheinhalte bereinigen
  * Lebensdauerfunktion (Time to Live - TTL)
  * Metriken mit grafischen Ansichten
  * HTTPS-Unterstützung mit Platzhalterzertifikat
  * Host-Header-Unterstützung
  * Nicht aktuelle Inhalte bereitstellen
  * Funktion "Abfrageargumente im Cacheschlüssel ignorieren"
  * Inhaltskomprimierung
  * Optimierung für große Dateien
  * Video-on-Demand

## Funktionsbeschreibungen

### Unterstützung für den Host-Serverursprung (Host Server Origin)

IBM Cloud Content Delivery Network (CDN) kann so konfiguriert werden, dass Inhalte von einem Host-Serverursprung (Host Server Origin) aus zugestellt werden. Dazu werden der Hostname des Ursprungsservers, das Protokoll, die Portnummer und optional der Pfad, aus dem die Inhalte zustellt werden sollen, angegeben. Der Standardpfad ist '/\*'. Das Protokoll kann HTTP, HTTPS oder beides sein. Von Akamai werden nur bestimmte Portnummern unterstützt. Informationen zu unterstützten Portnummern/Portnummernbereichen finden Sie in den [häufig gestellten Fragen (FAQs)](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-). Gegenwärtig wird HTTPS mit einem Platzhalterzertifikat unterstützt. Dies bedeutet, dass der Service nur über den CNAME erreichbar ist, der auf `.cdnedge.bluemix.net` endet. Weitere Informationen finden Sie in der Funktionsbeschreibung der [HTTPS-Unterstützung mit Platzhalterzertifikat](about.html#https-with-wildcard-certificate-).

### Unterstützung für den Objektspeicherursprung (Object Storage Origin)

IBM Cloud CDN kann so konfiguriert werden, dass Inhalte aus einem Objektspeicherendpunkt (Object Storage) bereitgestellt werden. Dazu muss der _Endpunkt_, der _Bucketname_, das _Protokoll_ und der _Port_ angegeben werden. Optional kann eine Liste der Dateierweiterungen angegeben werden, um das Caching nur für Dateien mit diesen Erweiterungen zuzulassen. Alle Objekte im Bucket müssen auf anonymen oder öffentlichen Lesezugriff eingestellt werden. Ausführlichere Informationen finden Sie im Lernprogramm zur [Einrichtung von IBM Bluemix Object Storage mit CDN](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn).

### Unterstützung für mehrere Ursprünge mit unterschiedlichen Pfaden

In gewissen Fällen kann es nützlich sein, bestimmte Inhalte von einem anderen Ursprungsserver zuzustellen. Es kann zum Beispiel wünschenswert sein, bestimmte Fotos oder Videos von anderen Ursprungsservers zu liefern. IBM Cloud CDN bietet die Option, mehrere Ursprungsserver mit mehreren Pfaden einzurichten. Dies ermöglicht Flexibilität in Bezug auf die Art und Weise sowie auf den Standort der Datenspeicherung. Der Pfad, der für den Ursprungsserver angegeben wird, muss für das CDN eindeutig sein. Der Ursprungsserver an sich muss nicht eindeutig sein.

### Pfadbasierte CDN-Zuordnungen

Ihr IBM Cloud CDN-Service kann bei der Erstellung des CDN auf einen bestimmten Verzeichnispfad auf dem Ursprungsserver durch die Angabe des Pfads eingeschränkt werden. Ein Endbenutzer ist nur für den Zugriff auf die Inhalte in diesem Verzeichnispfad berechtigt. Wenn zum Beispiel ein CDN 'www.example.com' mit dem Pfad '/videos' erstellt wird, ist es nur über `www.example.com/videos/` zugänglich.

### Bereinigung von Cacheinhalten

IBM Cloud CDN bietet die Möglichkeit, den im Cache gespeicherten Inhalt von Edge-Servern bequem und schnell zu entfernen oder zu "bereinigen". Gegenwärtig ist das Bereinigen nur für einen Dateipfad zulässig. Zurzeit bereinigt die Bereinigungsimplementierung (Purge) mit Akamai die Datei in ungefähr fünf Sekunden. In der Benutzerschnittstelle können Sie den Bereinigungsverlauf anzeigen und bestimmte Bereinigungspfade als Favoriten markieren.

### Lebensdauerfunktion (Time to Live - TTL)

Die Lebensdauer (Time To Live) gibt die Zeitdauer (in Sekunden) an, die der Edge-Server den Inhalt für eine bestimmte Datei oder einen bestimmten Verzeichnispfad im Cache speichert. Wenn ein CDN zuerst erstellt wird, wird ein globaler TTL-Wert für den Pfad ('/\*') mit einer Standarddauer von 3600 Sekunden erstellt. Der Mindestwert für TTL ist 30 Sekunden, der Maximalwert 2.147.483.647 Sekunden. Für jeden Eintrag muss der Pfad für TTL für das CDN eindeutig sein. Wenn mehrere Pfade einem gegebenen Inhalt entsprechen, gilt der zuletzt konfigurierte Pfad für diesen Inhalt. Betrachten Sie zum Beispiel zwei TTLs: Der Pfad `/example/file` wird zuerst mit einem TTL-Wert von 3000 Sekunden erstellt. Später wird der Pfad `/example/*` mit einem Wert von 4000 Sekunden erstellt. Der Pfad `/example/file` ist zwar spezifischer, jedoch wurde der Pfad `/example/*` zuletzt erstellt, sodass der TTL-Wert für `/example/file` 4000 beträgt. Nach der Erstellung können TTL-Einträge in Bezug auf den Pfad und/oder die Zeitdauer bearbeitet werden. Sie können außerdem auch gelöscht werden.

### Metriken mit grafischen Ansichten

Metriken für ein einzelnes CDN werden auf der Registerkarte 'Übersicht' des Kundenportals für die betreffende CDN-Zuordnung bereitgestellt. Zur Nutzung des CDN werden zwei Typen von Metriken berechnet: Metriken, die Werte über einen Zeitraum in Form eines Diagramms darstellen, und Metriken, die in Form von Aggregatwerten aufbereitet werden.

Für Metriken, die Änderungen über einen Zeitraum als Diagramm darstellen, werden drei Kurvendiagramme und ein Kreisdiagramm angezeigt. Die folgenden drei Kurvendiagramme sind verfügbar: **Bandbreite** (Bandwidth), **Treffer nach Zuordnung** (Hits by Mapping) und **Treffer nach Typ** (Hits by Type). Sie stellen die Aktivität nach Tagen über den Verlauf Ihres angegebenen Zeitrahmens dar. Die Diagramme für **Bandbreite** und **Treffer nach Zuordnung** sind Einzelkurvendiagramme, während die Aufgliederung des Diagramms **Treffer nach Typ** eine Kurve für jeden bereitgestellten Treffertyp darstellt. Das Kreisdiagramm zeigt eine regionale Aufgliederung der Bandbreite für eine CDN-Zuordnung auf Prozentsatzbasis an.

Metriken, die für Aggregatwerte angezeigt werden, umfassen die **Bandbreitennutzung** in GB, die **Gesamtzahl Treffer** für den CDN-Edge-Server und die **Trefferquote**. Die Trefferquote gibt den Prozentsatz für die Häufigkeit an, mit der Inhalte durch den Edge-Server und nicht durch seinen Ursprungsserver bereitgestellt werden. Die Trefferquote wird zurzeit als Funktion aller Ihrer CDN-Zuordnungen dargestellt und nicht nur für die CDN-Zuordnung, die Sie gerade anzeigen.

Sowohl die Aggregatwerte als auch die Diagramme zeigen standardmäßig Metriken für die letzten 30 Tage an. Sie haben jedoch die Möglichkeit, dies über das [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) zu ändern. Für beide Kategorien können Metriken für Zeiträume von 7, 15, 30, 60 oder 90 Tagen angezeigt werden.

### HTTPS-Unterstützung mit Platzhalterzertifikat

Das Platzhalterzertifikat ist die ökonomischste Methode zum sicheren Bereitstellen von Webinhalten für Ihre Endbenutzer. Diese Funktion wird durch Auswahl der Option `HTTPS-Port` beim Konfigurieren des CDN oder Ursprungsservers aktiviert. Wenn die CDN-Zuordnung für HTTPS konfiguriert wurde, kontaktiert der Edge-Server den Ursprungsserver über HTTPS mithilfe von Platzhalterzertifikaten. Wenn der Ursprungsserver durch einen Hostnamen angegeben ist, verwendet der Edge-Server standardmäßig den Hostnamen des Ursprungsservers als SNI-Header (SNI - Server Name Indication) für die TLS-Vereinbarung mit dem Ursprungsserver (TLS = Transport Layer Security). Wenn der Ursprungsserver jedoch durch eine IP-Adresse angegeben ist, wird der Hostname des CDN als SNI-Header verwendet. Das Ursprungszertifikat muss von einer anerkannten Zertifizierungsstelle (Certificate Authority, CA) signiert sein.

Bei Verwendung des Platzhalterzertifikats hat ein Endbenutzer **nur** über den CNAME Zugriff auf den Service. Wenn die Domäne zum Beispiel `example.com` ist und der CNAME `example.cdnedge.bluemix.net` lautet, ist Ihr CDN-Service **nur** über die Adresse `https://example.cdnedge.bluemix.net` zugänglich. Erläuterungen zu den Auswirkungen der Verwendung von HTTPS mit Platzhalterzertifikaten finden Sie in den [häufig gestellten Fragen (FAQs)](faqs.html#what-should-be-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-cdn-protocols-).

Als bewährtes Verfahren vertraut Akamai nur Stammzertifikaten und nicht den Zwischenzertifikaten, da sich die Gruppe der Zwischenzertifikate, denen vertraut wird, häufig ändert. Aus Sicherheitsgründen schließt Akamai nur Stammzertifizierungsstellen in den Akamai-Truststore ein. Daher muss der Ursprungsserver die gesamte Zertifikatskette, einschließlich der nicht hierarchischen Zertifikate und aller Zwischenzertifikate (ohne Stammzertifikat) im Rahmen des SSL-Handshakes mit dem Edge-Server senden. Informationen zu den Zertifikaten, denen Akamai vertraut, finden Sie [hier](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

### Host-Header-Unterstützung

Der Edge-Server verwendet den Host-Header bei der Kommunikation mit dem Ursprungshost. Diese Funktion bietet Flexibilität im Hinblick auf die Konfiguration des Web-Service auf dem Ursprungshost. Wenn keine Host-Header-Eingabe angegeben wird, verwendet der Service den Hostnamen des Ursprungsservers als Standard-HTTP-Host-Header, sofern der Ursprungsserver durch den Hostnamen (und nicht durch eine IP-Adresse) angegeben wird. Wenn der Host-Header nicht als Eingabe angegeben wird und der Ursprungsserver durch eine IP-Adresse angegeben wird, wird der CDN-Hostname (auch als CDN-Domänenname bezeichnet) als Standard-HTTP-Host-Header verwendet.

### Header beibehalten

Durch die Option 'Header beibehalten' kann die HTTP-Header-Konfiguration im Ursprungsserver die CDN-Konfiguration überschreiben. Diese Funktion ist standardmäßig aktiviert, kann jedoch inaktiviert werden.

### Nicht aktuelle Inhalte bereitstellen

Wenn der CDN-Edge-Server eine Benutzeranforderung empfängt und der angeforderte Inhalt nicht im Cache enthalten ist, kontaktiert der Edge-Server den Ursprungshost, um den Inhalt abzurufen. Der Inhalt wird anschließend für die Dauer, die durch den TTL-Wert (Time To Live) für den Inhalt angegeben ist, im Cache gespeichert. Wenn eine Benutzeranforderung nach Ablauf der TTL-Dauer empfangen wird, kontaktiert der Edge-Server den Ursprungshost, um den Inhalt abzurufen. Falls der Ursprungsserver nicht erreichbar ist (z. B. weil der Ursprungshost inaktiv ist oder ein Netzproblem vorliegt), stellt der Edge-Server den abgelaufenen (nicht mehr aktuellen) Inhalt als Antwort auf die Anforderung zu. Diese Funktion wird von Akamai unterstützt und **kann nicht** inaktiviert werden.

### Funktion "Abfrageargumente im Cacheschlüssel ignorieren"

Akamai-Edge-Server speichern Inhalte in einem so genannten Cachespeicher ("Cache Store") zwischen. Zur Verwendung der Inhalte aus dem Cachespeicher verwenden Edge-Server einen Cacheschlüssel ("Cache Key"). In der Regel wird ein Cacheschlüssel als Bestandteil der URL eines Endbenutzers generiert. In einigen Fällen enthält die URL Argumente der Abfragefunktion, die sich für Einzelbenutzer unterscheiden, jedoch ist der zugestellte Inhalt identisch. Standardmäßig verwendet Akamai die Argumente der Abfragefunktion, um den Cacheschlüssel zu generieren, sodass für jeden Benutzer einer eigener Cacheschlüssel generiert wird. Diese Methode ist nicht optimal, da sie zur Folge hat, dass der Edge-Server den Ursprungsserver unter Verwendung eines anderen Cacheschlüssels kontaktiert, um Inhalte abzurufen, die sich bereits im Cache befinden. Durch die Funktion **Abfrageargumente im Cacheschlüssel ignorieren** können Sie angeben, ob die Abfrageargumente beim Generieren eines Cacheschlüssels ignoriert werden sollen. Diese Funktion gilt für jedes `Erstellen` oder `Aktualisieren` einer CDN-Zuordnungskonfiguration sowie für jedes `Erstellen` oder `Aktualisieren` eines Ursprungspfads.

**Hinweis:** Abfrageargumente im Cacheschlüssel können über die Registerkarte 'Einstellungen' nach Erstellung einer CDN-Zuordnung konfiguriert werden. Für den Ursprungspfad können sie beim `Erstellen` oder `Aktualisieren` eines Ursprungspfads konfiguriert werden.

### Inhaltskomprimierung

Die Inhaltskomprimierung ist im Akamai-CDN standardmäßig für die folgenden Inhaltstypen aktiviert:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Wenn die Komprimierung durch den Edge-Server ausgeführt wird, muss der Inhalt mindestens 10 KB umfassen. In einigen Fällen wird die Komprimierung durch den Ursprungsserver ausgeführt. In diesen Fällen gibt es keine Begrenzung für die Größe der Dateien, die komprimiert werden sollen. Wenn der Inhalt bereits durch den Ursprungsserver komprimiert wurde, wird er nicht erneut komprimiert. Zur Aktivierung der Inhaltskomprimierung muss im Anforderungsheader `Accept-Encoding: gzip` definiert werden.

### Optimierung für große Dateien

Die Optimierung für große Dateien ermöglicht es dem CDN-Netz, die Zustellung von Inhalten mit Größen über 10 MB zu optimieren. Diese Unterstützung erhöht die Leistung für große Dateien und verringert Latenz- und Downloadzeiten. Ohne diese Funktion kann das CDN keine Dateien mit Größen über 1,8 GB bedienen. Diese Funktion ermöglicht das Herunterladen von Dateien mit Größen über 1,8 GB bis hin zu 320 GB.

Damit die Optimierung für große Dateien funktioniert, **müssen** Bytebereichsanforderungen auf dem Ursprungsserver aktiviert werden. Das Akamai-CDN arbeitet für diese Optimierung mit einem Verfahren, das als Partial Object Caching (partielles Objektcaching) bezeichnet wird. Wenn eine große Datei angefordert wird, prüft der Edge-Server, ob diese Datei die Größenanforderungen erfüllt. Dies bedeutet, dass Dateien, die größer als 10 MB sind, in Teilen (Chunks) vom Ursprungsserver angefordert werden. Wenn der Chunk im Edge-Server eintrifft, wird er im Cache gespeichert und dem Benutzer sofort bereitgestellt. Der nächste Chunk wird vom Edge-Server vorab parallel abgerufen, um die Latenz zu verringern. Dieser Prozess wird fortgesetzt, bis die gesamte Datei abgerufen wurde oder die Verbindung beendet wird.  

Wenn diese Funktion aktiviert ist, entsteht eine geringfügige Leistungsbeeinträchtigung bei der Bereitstellung von Inhalten, die kleiner als 10 MB sind. Daher wird diese Funktion nur für die Bereitstellung großer Dateien empfohlen. Ein typischer Anwendungsfall wäre die Erstellung eines neuen Ursprungspfads in der CDN-Konfiguration und die Aktivierung der Optimierung für große Dateien für diesen Pfad.

### Video-on-Demand

Die Leistungsoptimierung für **Video-on-Demand** liefert ein Streaming mit hoher Qualität über verschiedene Typen von Netzen. Durch die Nutzung der Fähigkeit des verteilten Netzes, die Auslastung dynamisch zu verteilen, bietet IBM Cloud CDN mit Akamai die Möglichkeit, schnelle Skalierungen für geplant oder ungeplant wachsende Benutzergruppen durchzuführen.

**Video-on-Demand** wird für die Verteilung segmentierter Streamingformate wie HLS, DASH, HDS und HSS optimiert. Live-Video-Streams werden gegenwärtig **nicht** unterstützt. Sie können die Funktion **Video-on-Demand** aktivieren, indem Sie die Option im Dropdown-Menü unter **Optimieren für** auf der Registerkarte 'Einstellungen' auswählen oder wenn Sie einen neuen Ursprungspfad erstellen. Sie können diese Funktion nur aktivieren, wenn Sie die Zustellung von Videodateien optimieren.
