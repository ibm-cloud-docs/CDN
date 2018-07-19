---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-28"

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
  * Unterstützung für HTTPS-Protokoll mit Wildcard- und DV-SAN-Zertifikat
  * Host-Header-Unterstützung
  * Nicht aktuelle Inhalte bereitstellen
  * Abfrageargumente im Cacheschlüssel
  * Inhaltskomprimierung
  * Optimierung für große Dateien
  * Video-on-Demand

Die vollständigen Beschreibungen zu diesen Funktionen finden Sie in [diesem Dokument](feature-description.html#feature-descriptions).

## Diagramm der Architektur

Im folgenden Diagramm veranschaulicht eine schematische Übersicht die dreischichtige Architektur von IBM Cloud CDN.

![Diagramm der Architektur](images/3-tier-architecture.png)
