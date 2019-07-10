---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Funktionsbeschreibungen
{: #feature-descriptions}

Auf dieser Seite werden zahlreiche Features ausführlicher erläutert, die in {{site.data.keyword.cloud}} CDN mit Unterstützung von Akamai bereitgestellt werden.

## Unterstützung für den Host-Serverursprung (Host Server Origin)
{: #host-server-origin-support}

IBM Cloud Content Delivery Network (CDN) kann so konfiguriert werden, dass Inhalte von einem Host-Serverursprung (Host Server Origin) aus zugestellt werden. Dazu werden der Hostname des Ursprungsservers, das Protokoll, die Portnummer und optional der Pfad, aus dem die Inhalte zustellt werden sollen, angegeben. Der Standardpfad lautet `/*`. Das Protokoll kann HTTP, HTTPS oder beides sein. Von Akamai werden nur bestimmte Portnummern unterstützt. Informationen zu unterstützten Portnummern/Portnummernbereichen finden Sie in den [häufig gestellten Fragen (FAQs)](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

## Unterstützung für den Objektspeicherursprung (Object Storage Origin)
{: #object-storage-file-support}

IBM Cloud CDN kann so konfiguriert werden, dass Inhalt aus einem Object Storage-Endpunkt durch Angeben des Endpunkts, des Bucketnamens, des Protokolls und des Ports bereitgestellt werden. Optional kann eine Liste der Dateierweiterungen angegeben werden, um das Caching nur für Dateien mit diesen Erweiterungen zuzulassen. Alle Objekte im Bucket müssen auf anonymen oder öffentlichen Lesezugriff eingestellt werden.

In diesem Lernprogramm zum Thema [Vorgehensweise zum Konfigurieren von IBM Cloud Object Storage mit CDN](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) werden detailliertere Informationen bereitgestellt.

## Unterstützung für mehrere Ursprünge mit unterschiedlichen Pfaden
{: #support-for-multiple-origins-with-distinct-paths}

In gewissen Fällen kann es nützlich sein, bestimmte Inhalte von einem anderen Ursprungsserver zuzustellen. Es kann zum Beispiel wünschenswert sein, bestimmte Fotos oder Videos von anderen Ursprungsservers zu liefern. IBM Cloud CDN bietet die Option, mehrere Ursprungsserver mit mehreren Pfaden einzurichten. Dies ermöglicht Flexibilität in Bezug auf die Art und Weise sowie auf den Standort der Datenspeicherung. Der Pfad, der für den Ursprungsserver angegeben wird, muss für das CDN eindeutig sein. Der Ursprungsserver an sich muss nicht eindeutig sein.

## Pfadbasierte CDN-Zuordnungen
{: #path-based-cdn-mappings}

Ihr IBM Cloud CDN-Service kann bei der Erstellung des CDN auf einen bestimmten Verzeichnispfad auf dem Ursprungsserver durch die Angabe des Pfads eingeschränkt werden. Ein Endbenutzer ist nur für den Zugriff auf die Inhalte in diesem Verzeichnispfad berechtigt. Beispiel: Wenn die CDN-Instanz `www.example.com` mit dem Pfad `/videos` erstellt wird, ist ein Zugriff **nur** mit `www.example.com/videos/*` möglich.

## Cacheinhalte bereinigen
{: #purge-cached-content}

IBM Cloud CDN bietet die Möglichkeit, den im Cache gespeicherten Inhalt von Edge-Servern bequem und schnell zu entfernen oder zu "bereinigen". Gegenwärtig ist das Bereinigen nur für einen Dateipfad zulässig. Zurzeit bereinigt die Bereinigungsimplementierung (Purge) mit Akamai die Datei in ungefähr fünf Sekunden. In der Benutzerschnittstelle können Sie den Bereinigungsverlauf anzeigen und bestimmte Bereinigungspfade als Favoriten markieren.

## Lebensdauerfunktion (Time to Live - TTL)
{: #time-to-live-ttl}

Die Lebensdauer (Time To Live) gibt die Zeitdauer (in Sekunden) an, die der Edge-Server den Cacheinhalt für eine bestimmte Datei oder einen bestimmten Verzeichnispfad im Cache aufbewahrt. Wenn eine CDN-Instanz zum ersten Mal erstellt wird, wird ein globaler TTL-Wert für den Pfad `/\*` mit einer Standarddauer von 3600 Sekunden erstellt. Der Mindestwert für TTL beträgt 0 Sekunden, der Maximalwert 2.147.483.647 Sekunden. Für jeden Eintrag muss der Pfad für TTL für das CDN eindeutig sein. Wenn mehrere Pfade einem gegebenen Inhalt entsprechen, gilt der zuletzt konfigurierte Pfad für diesen Inhalt. Betrachten Sie zum Beispiel zwei TTLs: Der Pfad `/example/file` wird zuerst mit einem TTL-Wert von 3000 Sekunden erstellt. Später wird der Pfad `/example/*` mit einem Wert von 4000 Sekunden erstellt. Der Pfad `/example/file` ist zwar spezifischer, jedoch wurde der Pfad `/example/*` zuletzt erstellt, sodass der TTL-Wert für `/example/file` 4000 beträgt. Nach der Erstellung können TTL-Einträge in Bezug auf den Pfad und/oder die Zeitdauer bearbeitet werden. Sie können außerdem auch gelöscht werden.

## Metriken mit grafischen Ansichten
{: #metrics-with-graphical-views}

Metriken für ein einzelnes CDN werden auf der Registerkarte 'Übersicht' des Kundenportals für die betreffende CDN-Zuordnung bereitgestellt. Zur Nutzung des CDN werden zwei Typen von Metriken berechnet: Metriken, die Werte über einen Zeitraum in Form eines Diagramms darstellen, und Metriken, die in Form von Aggregatwerten aufbereitet werden.

Für Metriken, die Änderungen über einen Zeitraum als Diagramm darstellen, werden drei Kurvendiagramme und ein Kreisdiagramm angezeigt. Die folgenden drei Kurvendiagramme sind verfügbar: **Bandbreite**, **Treffer nach Zuordnung** und **Treffer nach Typ**. Sie stellen die Aktivität nach Tagen über den Verlauf Ihres angegebenen Zeitrahmens dar. Die Diagramme für **Bandbreite** und **Treffer nach Zuordnung** sind Einzelkurvendiagramme, während die Aufgliederung des Diagramms **Treffer nach Typ** eine Kurve für jeden bereitgestellten Treffertyp darstellt. Das Kreisdiagramm zeigt eine regionale Aufgliederung der Bandbreite für eine CDN-Zuordnung auf Prozentsatzbasis an.

Metriken, die für Aggregatwerte angezeigt werden, umfassen die **Bandbreitennutzung** in GB, die **Gesamtzahl Treffer** für den CDN-Edge-Server und die **Trefferquote**. Die Trefferquote gibt den Prozentsatz für die Häufigkeit an, mit der Inhalte durch den Edge-Server und _nicht_ durch seinen Ursprungsserver bereitgestellt werden. Die Trefferquote wird zurzeit als Funktion aller Ihrer CDN-Zuordnungen dargestellt und nicht nur für die CDN-Zuordnung, die Sie gerade anzeigen.

Sowohl die Aggregatwerte als auch die Diagramme zeigen standardmäßig Metriken für die letzten 30 Tage an. Sie haben jedoch die Möglichkeit, dies über das [Kundenportal ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/) zu ändern. Für beide Kategorien können Metriken für Zeiträume von 7, 15, 30, 60 oder 90 Tagen angezeigt werden.

## Host-Header-Unterstützung
{: #host-header-support}

Vom Edge-Server wird der **Host-Header** für die Kommunikation mit dem Ursprungshost verwendet. Diese Funktion bietet Flexibilität im Hinblick auf die Konfiguration des Web-Service auf dem Ursprungshost. Insbesondere kann ein Anwendungsfall verwendet werden, bei dem ein Client mehrere Web-Server auf demselben Ursprungshost konfiguriert hat. Wenn keine Host-Header-Eingabe angegeben wird, verwendet der Service den Hostnamen des Ursprungsservers als Standard-HTTP-Host-Header, sofern der Ursprungsserver durch den Hostnamen (und nicht durch eine IP-Adresse) angegeben wird. Wenn der Host-Header nicht als Eingabe angegeben wird und der Ursprungsserver durch eine IP-Adresse angegeben wird, wird der CDN-Hostname (auch als CDN-Domänenname bezeichnet) als Standard-HTTP-Host-Header verwendet.

## Unterstützung für HTTPS-Protokoll
{: #https-protocol-support}

CDN kann so konfiguriert werden, dass das HTTPS-Protokoll zum sicheren Bereitstellen des Inhalts für die Endbenutzer verwendet wird. Für diese Konfiguration ist das Einrichten eines SSL-Zertifikats im Rahmen der CDN-Konfiguration erforderlich. Für HTTPS stehen zwei Arten von SSL-Zertifikatsoptionen zur Verfügung: ein [Wildcard-Zertifikat](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) und ein [DV-SAN-Zertifikat](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport). Dieser Typ wird in dieser Dokumentation auch als _SAN-Zertifikat_ bezeichnet.

Der Typ des SSL-Zertifikats für HTTPS-CDN sollte mit Bedacht ausgewählt werden. Die Konfiguration eines Wildcard-Zertifikats ist zwar schnell, der Nachteil ist jedoch, dass auf CDN nur über einen CNAME zugegriffen werden kann. Die Durchführung des Prozesses für ein SAN-Zertifikat dauert zwar vier bis acht Stunden, dafür kann CDN mit der CDN-Domäne (also dem Hostnamen) verwendet werden. Für ein SAN-Zertifikat ist auch ein weiterer Schritt für eine [**Validierung der Domänensteuerung**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) während der Konfiguration erforderlich. Für die Verwendung beider Zertifikate fallen keine Kosten an. Informationen zu den Auswirkungen bei der Auswahl eines bestimmten Zertifikats finden Sie unter [Fehlerbehebung für Dokumente](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-).

Der Ursprungshost muss auch über ein eigenes SSL-Zertifikat für den CDN-Hostnamen verfügen; das Zertifikat muss auch von einer anerkannten Zertifizierungsstelle (Certificate Authority, CA) signiert sein.

Als bewährtes Verfahren vertraut Akamai nur Stammzertifikaten und nicht den Zwischenzertifikaten, da sich die Gruppe der Zwischenzertifikate, denen vertraut wird, häufig ändert. Die von Akamai als vertrauenswürdig anerkannten Zertifikate finden Sie [im Web an dieser Position](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Header beibehalten
{: #respect-headers}

Die Option **Header beibehalten** ermöglicht das Überschreiben der CDN-Konfiguration durch die Konfiguration des HTTP-Headers auf dem Ursprungsserver. Diese Funktion ist standardmäßig aktiviert, kann jedoch inaktiviert werden.

## Nicht aktuelle Inhalte bereitstellen
{: #serve-stale-content}

Wenn der CDN-Edge-Server eine Benutzeranforderung empfängt und der angeforderte Inhalt nicht im Cache enthalten ist, kontaktiert der Edge-Server den Ursprungshost, um den Inhalt abzurufen. Der Inhalt wird anschließend für die Dauer, die durch den TTL-Wert (Time To Live) für den Inhalt angegeben ist, im Cache gespeichert. Wenn eine Benutzeranforderung nach Ablauf der TTL-Dauer empfangen wird, kontaktiert der Edge-Server den Ursprungshost, um den Inhalt abzurufen. Falls der Ursprungsserver nicht erreichbar ist (z. B. weil der Ursprungshost inaktiv ist oder ein Netzproblem vorliegt), stellt der Edge-Server den abgelaufenen (nicht mehr aktuellen) Inhalt als Antwort auf die Anforderung zu. Diese Funktion wird von Akamai unterstützt und **kann nicht** inaktiviert werden.

## Optimierung für Cacheschlüssel
{: #cache-key-optimization}

Akamai-Edge-Server speichern Inhalt in einem so genannten **Cachespeicher** ("Cache Store") zwischen. Zur Verwendung der Inhalte aus dem **Cachespeicher** verwenden Edge-Server einen **Cacheschlüssel**. In der Regel wird ein **Cacheschlüssel** als Bestandteil der URL eines Endbenutzers generiert. In einigen Fällen enthält die URL Argumente der Abfragefunktion, die sich für Einzelbenutzer unterscheiden, jedoch ist der zugestellte Inhalt identisch. Standardmäßig verwendet Akamai die Argumente der Abfragefunktion, um den Cacheschlüssel zu generieren, sodass für jeden Benutzer einer eigener Cacheschlüssel generiert wird. Diese Methode ist nicht optimal, da sie zur Folge hat, dass der Edge-Server den Ursprungsserver unter Verwendung eines anderen Cacheschlüssels kontaktiert, um Inhalte abzurufen, die sich bereits im Cache befinden. Mit der Funktion **Optimierung für Cacheschlüssel** können Sie angeben, welche Abfrageargumente bei der Generierung eines Cacheschlüssels eingeschlossen oder ignoriert werden sollen. Diese Funktion gilt für jedes `Erstellen` oder `Aktualisieren` einer CDN-Zuordnungskonfiguration sowie für jedes `Erstellen` oder `Aktualisieren` eines Ursprungspfads.

Der Wert für **Optimierung für Cacheschlüssel** kann nach der Erstellung einer CDN-Zuordnung auf der Registerkarte **Einstellungen** konfiguriert werden. Für den Ursprungspfad können sie während der Operationen zum Erstellen (`create`) oder Aktualisieren (`update`) eines Ursprungspfads konfiguriert werden.
{: note}

## Inhaltskomprimierung
{: #content-compression}

Die **Inhaltskomprimierung** ist in der Akamai-CDN-Instanz standardmäßig für die folgenden Inhaltstypen aktiviert:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Wenn die Komprimierung durch den Edge-Server ausgeführt wird, muss der Inhalt mindestens 10 KB umfassen.  In einigen Fällen wird die Komprimierung durch den Ursprungsserver ausgeführt. In diesen Fällen gibt es keine Begrenzung für die Größe der Dateien, die komprimiert werden sollen. Wenn der Inhalt bereits durch den Ursprungsserver komprimiert wurde, wird er nicht erneut komprimiert. Zur Aktivierung der Inhaltskomprimierung muss im Anforderungsheader `Accept-Encoding: gzip` definiert werden.

## Optimierung für große Dateien
{: #large-file-optimization}

Die Optimierung für große Dateien ermöglicht es dem CDN-Netz, die Zustellung von Inhalten mit Größen über 10 MB zu optimieren. Diese Unterstützung erhöht die Leistung für große Dateien und verringert Latenz- und Downloadzeiten. Ohne diese Funktion kann das CDN keine Dateien mit Größen über 1,8 GB bedienen. Diese Funktion ermöglicht das Herunterladen von Dateien mit Größen über 1,8 GB bis hin zu 320 GB.

Damit die Optimierung für große Dateien funktioniert, **müssen** Bytebereichsanforderungen auf dem Ursprungsserver aktiviert werden. Das Akamai-CDN arbeitet für diese Optimierung mit einem Verfahren, das als _Partial Object Caching_ (partielles Objektcaching) bezeichnet wird. Wenn eine große Datei angefordert wird, prüft der Edge-Server, ob diese Datei die Größenanforderungen erfüllt. Dies bedeutet, dass Dateien, die größer als 10 MB sind, in Teilen (Chunks) vom Ursprungsserver angefordert werden. Wenn der Chunk im Edge-Server eintrifft, wird er im Cache gespeichert und dem Benutzer sofort bereitgestellt. Der nächste Chunk wird vom Edge-Server vorab parallel abgerufen, um die Latenz zu verringern. Dieser Prozess wird fortgesetzt, bis die gesamte Datei abgerufen wurde oder die Verbindung beendet wird.

Wenn diese Funktion aktiviert ist, entsteht eine geringfügige Leistungsbeeinträchtigung bei der Bereitstellung von Inhalten, die kleiner als 10 MB sind. Daher wird diese Funktion nur für die Bereitstellung großer Dateien empfohlen. Ein typischer Anwendungsfall wäre die Erstellung eines neuen Ursprungspfads in der CDN-Konfiguration und die Aktivierung der Optimierung für große Dateien für diesen Pfad.

## Video-on-Demand
{: #video-on-demand}

Die Leistungsoptimierung für **Video-on-Demand** liefert ein Streaming mit hoher Qualität über verschiedene Typen von Netzen. Durch die Nutzung der vorkonfigurierten Einstellungen zur Cachesteuerung und der Fähigkeit des verteilten Netzes, die Arbeitslast dynamisch zu verteilen, bietet IBM Cloud CDN mit Akamai die Möglichkeit, schnelle Skalierungen für geplante oder ungeplante große Benutzergruppen durchzuführen.

**Video-on-Demand** wird für die Verteilung segmentierter Streamingformate wie HLS, DASH, HDS und HSS optimiert. Live-Video-Streams werden gegenwärtig **nicht** unterstützt. Sie können die Funktion **Video-on-Demand** aktivieren, indem Sie die Option im Dropdown-Menü unter **Optimieren für** auf der Registerkarte 'Einstellungen' auswählen oder wenn Sie einen neuen Ursprungspfad erstellen. Sie können diese Funktion nur aktivieren, wenn Sie die Zustellung von Videodateien optimieren.

## Geografische Zugriffssteuerung (Geographical Access Control)
{: #geographical-access-control}

Bei der Funktion 'Geographical Access Control' handelt es sich um ein auf Regeln basierendes Verhalten, bei dem Sie den Parameter `access-type` ausgerichtet am Standort für eine Gruppe von Benutzern festlegen können. Es stehen zwei Typen von Verhalten zur Auswahl: **Allow** und **Deny**.

Der Zugriffstyp `Allow` ermöglicht es Ihnen, ausgerichtet am Regionstyp gezielt Datenverkehr mit ausgewählten Regionen zuzulassen. Durch das Zulassen von Datenverkehr für bestimmte Regionen wird implizit der Datenverkehr mit allen anderen Regionen blockiert. Sie können beispielsweise über `Allow` den Datenverkehr mit ausgewählten Kontinenten, z. B. mit Europa oder Ozeanien, zulassen und damit den Zugriff für die übrigen Kontinente blockieren.

Mit dem Verhalten `Deny` wird dagegen der Zugriff auf Ihren Service für die angegebene Gruppe blockiert, für alle übrigen, nicht angegebenen Regionen jedoch zugelassen. Wenn Sie als Zugriffstyp für Geographical Access Control das Verhalten `Deny` für Europa und Ozeanien festlegen, können Benutzer in diesen Kontinenten **nicht** auf Ihren Service zugreifen, während Benutzer in den übrigen Kontinenten über Zugriff verfügen.

Diese Funktion kann über die Seite **Einstellungen** der CDN-Konfiguration aufgerufen werden.

## Hot-Link-Schutz
{: #hotlink-protection}

Hot-Link-Schutz ist ein regelbasiertes Verhalten, mit dem Sie steuern können, ob bestimmte Websites über Ihr CDN auf Ihre Inhalte zugreifen dürfen oder nicht.  Der Browser enthält in der Regel einen `Referer`-Header, wenn eine HTTP-Anforderung von einem Link auf einer Webseite erstellt wird und wenn dieser Link auf eine ferne Ressource verweist. Der Link, den eine Website für den Zugriff auf ein Asset von einer anderen Website verwendet, wird als _Hot-Link_ bezeichnet.  Zwei Verhaltenstypen sind verfügbar: **ALLOW** und **DENY**.

Wenn `protectionType` auf `ALLOW` gesetzt ist:
* Wenn der Wert des `Referer`-Headers in einer an Ihr CDN gesendeten Anforderung mit einem der von Ihnen angegebenen `refererValues` übereinstimmt, stellt Ihr CDN den angeforderten Inhalt **bereit**.
* Andernfalls stellt Ihr CDN den Inhalt **nicht** bereit.

Wenn `protectionType` auf `DENY` gesetzt ist:
* Wenn der Wert des `Referer`-Headers in einer an Ihr CDN gesendeten Anforderung mit einem der von Ihnen angegebenen `refererValues` übereinstimmt, stellt Ihr CDN den angeforderten Inhalt **nicht bereit**.
* Andernfalls stellt Ihr CDN den Inhalt bereit.
