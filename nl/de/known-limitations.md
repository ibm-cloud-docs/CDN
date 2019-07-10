---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-30"

keywords: limitations, Akamai, multiple files, purge, hotlink, limit

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Bekannte Einschränkungen
{: #known-limitations}

Die folgenden Einschränkungen gelten für den CDN-Service des Anbieters Akamai:
* Die Bereinigung von Inhalten auf Verzeichnisebene oder in mehreren Dateien wird derzeit nicht unterstützt.
* Der Grenzwert für die Gesamtzahl der Ursprünge pro CDN beträgt 75.
* HTTPS mit DV-SAN-Zertifikat ist nur für neue CDNs verfügbar.
* Der Hot-Link-Schutz unterstützt keine URL-Abgleichbegriffe für `refererValues`, bei denen die Zeichengruppe vor dem ersten Punkt (`.`) die Zeichengruppe `://` enthält, z. B. `http://www.example.com` oder `www.example.com http://www.example.com`.
* HTTPS unter Verwendung von Wildcard-Zertifikaten wird zu diesem Zeitpunkt nicht unterstützt.
* Die STOP CDN-Funktionalität wird für Wildcard-Zertifikatsdomänen nicht unterstützt.

Die STOP CDN-Funktionalität ist für Wartungsfenster von maximal 7 Tagen vorgesehen. Nach 7 Tagen muss das CDN gestartet werden. Anderenfalls wird es inaktiviert, und der Datenverkehr zum CDN-CNAME wird nicht an den Ursprungsserver weitergeleitet.
{: important}
