---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: origin, path, container, collection, attributes, query, arguments, class, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Pfadcontainer (Ursprungspfadcontainer)
{: #path-origin-container}

Die Sammlung `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` enthält die Attribute, die von den Pfad-APIs (bzw. Ursprungspfad-APIs) genutzt werden. Alle Pfad-APIs geben eine Sammlung dieses Typs zurück.

**Klasse** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: Die eindeutige ID der Zuordnung, zu der dieser Ursprungspfad gehört.  
* `path`: Der Pfad relativ zur Domäne, über den dieser Ursprung erreicht werden kann.  
* `originType`: Der Typ des Ursprungshosts, gegenwärtig ‘HOST\_SERVER’ oder ‘OBJECT\_STORAGE’.  
* `httpPort`: Die Nummer des Ports, der für das HTTP-Protokoll verwendet wird.  
* `httpsPort`: Die Nummer des Ports, der für das HTTPS-Protokoll verwendet wird.  
* `status`: Der Status des Pfads (Ursprungspfads). Gültige Statuswerte: _CREATING_ (Wird erstellt), _STARTING_ (Wird gestartet), _RUNNING_ (Aktiv), _UPDATING_ (Wird aktualisiert), _STOPPING_ (Wird gestoppt) und _DELETING_ (Wird gelöscht).
* `fileExtension`: Die Dateierweiterungen, deren Speicherung im Cache zugelassen wird.  
* `header`: Gibt die Informationen für den Host-Header an, die vom Ursprungsserver verwendet werden.
* `cacheKeyQueryRule`: Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar:
  * `include-all`: Schließt alle Abfrageargumente ein. **Standardwert**.
  * `ignore-all`: Ignoriert alle Abfrageargumente.
  * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten`: Ignoriert diese bestimmten Abfrageargumente. Beispiel: "ignore: abfrage1 abfrage2"
  * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten`: Schließt diese bestimmten Abfrageargumente ein. Beispiel: "include: abfrage1 abfrage2"
