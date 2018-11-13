---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Häufig gestellte Fragen zu HTTPS

## Was ist der Unterschied zwischen HTTPS mit Wildcard-Zertifikat und HTTPS mit SAN-Zertifikat?

Bei Nutzung eines Wildcard-Zertifikats verwenden alle Kunden dasselbe Zertifikat, das in den CDN-Netzen des Anbieters bereitgestellt wird. Für den Zugriff auf den Service muss der CNAME mit dem IBM Suffix `.cdnedge.bluemix.net` verwendet werden. Beispiel: `https://www.example-cname.cdnedge.bluemix.net`.

Bei Verwendung eines SAN-Zertifikats wird ein einziges SAN-Zertifikat von mehreren Kundendomänen genutzt; hierzu werden ihre Domänennamen in den SAN-Einträgen hinzugefügt. Auf den Service kann dann mithilfe des Hostnamens zugegriffen werden, zum Beispiel in Form von `https://www.example.com`.

## Wie wird eine Validierung einer Domäne mit Weiterleitung durchgeführt?

Dies hängt vom Server ab. Die Prozedur zur Ausführung der Domänenvalidierung für Apache- und Nginx-Server finden Sie auf der Seite [Validierung der Domänensteuerung für HTTPS ausführen](how-to-https.html#redirect).

## Wie lange dauert die Validierung einer Domäne?

Eine Domänenvalidierung dauert normalerweise zwischen zwei und vier Stunden, die Dauer hängt jedoch von der für die Validierung gewählten Methode ab. Eine Domänenvalidierung mit CNAME-Validierung ist die schnellste Variante und dauert in der Regel weniger als eine Stunde. Eine Domänenvalidierung mit Standard- und Weiterleitungsmethode dauert in der Regel nach dem Absenden der Abfrage ungefähr vier Stunden.

## Wie lange dauert der Erstellungsprozess mit DV-SAN-Zertifikat?

Eine normale Anforderung zur Aktivierung von HTTPS dauert von der ersten Anforderung bis zur Ausführung im Durchschnitt drei bis neun Stunden.

## Wie lange dauert das Löschen einer CDN-Instanz mit einem DV-SAN-Zertifikat?

Das Löschen der CDN-Instanz macht es erforderlich, dass die Domäne aus dem Zertifikat auf allen Edge-Servern entfernt werden muss. Die Ausführung dieses Prozesses kann bis zu acht Stunden dauern.

## Verursacht die Nutzung eines DV-SAN-Zertifikats zusätzliche Kosten?

Nein. Die Konfigurationen der DV-SAN-Zertifikate werden im Vergleich zu HTTP oder HTTPS mit einem Wildcard-Zertifikat ohne Aufpreis bereitgestellt.

## Kann meine CDN-Instanz, die mit HTTPS und Wildcard-Zertifikat erstellt wurde, so aktualisiert werden, dass ein DV-SAN-Zertifikat verwendet wird?

Nein, die Zuordnung eines Wildcard-Zertifikats kann derzeit nicht in die Zuordnung eines SAN-Zertifikats geändert werden.

## Was ist eine Zertifizierungsstelle?

Eine Zertifizierungsstelle (Certificate Authority, CA) ist eine Entität, von der digitale Zertifikate ausgegeben werden.

## Welche Zertifizierungsstelle wird vom IBM Cloud CDN-Service zum Ausgeben eines DV-SAN-Zertifikats verwendet?

Vom IBM Cloud CDN-Service wird die Zertifizierungsstelle "Let's Encrypt" verwendet.

## Welche SSL-Zertifikate werden unterstützt?

Unterstützte SSL-Zertifikate sind Wildcard-Zertifikate und DV-SAN-Zertifikate (DV - Domain Validation, SAN - Subject Alternate Name). Ein SAN-Zertifikat wird von mehreren Kunden gemeinsam genutzt. Von IBM Cloud CDN wird nicht das Hochladen angepasster Zertifikate unterstützt.

## Ich erhalte eine E-Mail, in der ich aufgefordert werde, eine Abfrage für eine Domänenvalidierung zu starten. Was kann ich tun?

Eine Domänenvalidierung kann auf drei Arten durchgeführt werden: Mit CNAME, mit der Standardmethode oder direkt.

Details zur Ausführung dieser drei Varianten finden Sie unter [Validierung der Domänensteuerung für HTTPS ausführen](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation).

## Was geschieht, wenn ich die Abfrage für die Domänenvalidierung nicht starte?

Falls der Zuordnungsstatus länger als 48 Stunden DOMAIN_VALIDATION_PENDING lautet, wird die Erstellung der Zuordnung abgebrochen, und der Status der Zuordnung wechselt zu CREATE_ERROR. In diesem Status können Sie die Erstellung erneut versuchen oder die Zuordnung löschen.

## Muss bei Verwendung eines Wildcard-Zertifikats die Domäne validiert werden?

Nein, zum Abrufen des Inhalts vom Ursprungsserver brauchen Sie nur den CNAME. `https://www.example-cname.cdnedge.bluemix.net`

## Kann ich bei Verwendung eines SAN-Zertifikats trotzdem den CNAME verwenden, um auf den Service zuzugreifen?

Nein. Bei Nutzung eines SAN-Zertifikats können Sie nur die angepasste Domäne für den Zugriff auf den Inhalt vom Ursprungsserver verwenden. `https://www.example.com`

## Werden alle Domänen in einem einzigen Zertifikat hinzugefügt?

Nicht unbedingt. Die Zertifikatsauswahl wird von Akamai ausgeführt, um sicherzustellen, dass sich die Zertifikate im effizientesten Status befinden. Die Domänen werden in den unterschiedlichen Zertifikaten in einem ausgewogenen Verhältnis hinzugefügt; somit kann nicht garantiert werden, dass sich alle Domänen in demselben Zertifikat befinden.

## Warum wird "Wildcard" angezeigt, wenn ich `dig` verwende oder wenn ich versuche, auf Inhalt zuzugreifen und sich die CDN-Instanz im Status `Zertifikat wird angefordert`, `Domänenvalidierung anstehend` oder `Zertifikat wird bereitgestellt` befindet?

Während des Anforderungsprozesses für das DV-SAN-Zertifikat wird die DNS-Datensatzkette für Ihr CDN vorübergehend mit einem Wildcard-Zertifikat verkettet. Bis der Prozess abgeschlossen ist, wird der Inhalt temporär über dieses Wildcard-Zertifikat bereitgestellt. Sobald der Anforderungsprozess abgeschlossen ist, wird die DNA-Datensatzkette aktualisiert und eine Verkettung mit dem DV-SAN-Zertifikat der CDN-Instanz durchgeführt.
