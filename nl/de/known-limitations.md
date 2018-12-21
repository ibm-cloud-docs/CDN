---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Bekannte Einschränkungen

Die folgenden Einschränkungen gelten für den neuen CDN-Service des Anbieters Akamai:
* Die Bereinigung von Inhalten auf Verzeichnisebene oder in mehreren Dateien wird derzeit nicht unterstützt.
* Der Grenzwert für die Gesamtzahl der Ursprünge pro CDN beträgt 75. 
* HTTPS mit DV-SAN-Zertifikat ist nur für neue CDNs verfügbar.
* Der Hotlinkschutz unterstützt keine URL-Abgleichbegriffe für `refererValues`, bei denen die Zeichengruppe vor dem ersten Zeichen `.` die Zeichenfolge `://` enthält, z. B. `http://www.example.com` oder `www.example.com http://www.example.com`. 
