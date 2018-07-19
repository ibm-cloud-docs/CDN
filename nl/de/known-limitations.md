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

# Bekannte Einschränkungen

Die folgenden Einschränkungen gelten für den neuen CDN-Service des Anbieters Akamai:
* Die Bereinigung von Inhalten auf Verzeichnisebene oder in mehreren Dateien wird derzeit nicht unterstützt.
* Derzeit sind maximal 10 CDN für ein angegebenes {{site.data.keyword.BluSoftlayer_notm}}-Konto zulässig.
* Die Gesamtzahl der Ursprungs- und TTL-Einträge pro CDN ist auf 75 begrenzt.
* Für die Funktion der Option 'Nicht aktuelle Inhalte bereitstellen' ist immer **Ein** eingestellt.
* Der Typ eines HTTPS-Zertifikats kann nach seiner Erstellung nicht mehr geändert werden, zum Beispiel von einem Wildcard-Zertifikat in ein DN-SAN-Zertifikat.
* HTTPS mit DV-SAN-Zertifikat ist nur für neue CDNs verfügbar.
* Eine CDN-Instanz, die mit einem DV-SAN-Zertifikat erstellt wurde, kann nur gelöscht werden, wenn sie sich im Status RUNNING, CNAME_Configuration, CREATE_ERROR oder DELETE_ERROR befindet.
