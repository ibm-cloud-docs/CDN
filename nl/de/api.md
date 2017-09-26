---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# API-Referenz

Weitere Informationen zur Konfiguration für PHP und SOAP finden Sie unter https://sldn.softlayer.com/article/PHP. 
 
## API für ein Konto
### verifyCdnAccountExists
Prüft, ob ein CDN-Konto für den Benutzer, der die API aufruft, für den angegebenen Anbieternamen (`vendorName`) besteht.

 * **Parameter**: `vendorName`
 * **Rückgabe**: `true`, wenn ein Konto vorhanden ist, andernfalls `false`
___

## API für Domänenzuordnung
### createDomainMapping
Diese Funktion erstellt anhand der bereitgestellten Eingaben eine Domänenzuordnung für den angegebenen Anbieter und ordnet sie der {{site.data.keyword.BluSoftlayer_notm}}-Konto-ID des Benutzers zu. Das CDN-Konto muss mit `createCustomerSubAccount` erstellt werden, damit diese API ausgeführt werden kann. Nachdem das CDN erfolgreich erstellt wurde, wird ein Element `defaultTTL` mit dem Wert 3600 Sekunden erstellt.

 * **Parameter**: Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`. Die Sammlung sollte die folgenden Elemente enthalten: `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType` und `path`.
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`. Die Sammlung stellt eine Kennung `uniqueId` bereit, die als Eingabe an nachfolgende API-Aufrufe gesendet werden muss, die sich auf die Zuordnung beziehen.
___ 
### deleteDomainMapping
Löscht die Domänenzuordnung basierend auf `uniqueId`. Die Domänenzuordnung muss einen der folgenden Statuswerte aufweisen: _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME\_CONFIGURATION_ oder _SSL\_CONFIGURATION_.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### verifyDomainMapping
Überprüft den Status des CDN und aktualisiert `status`, `cname` und/oder `vendorCname`, falls erforderlich. Gibt gegebenenfalls den bzw. die aktualisierten Wert(e) zurück. Die Domänenzuordnung muss einen der folgenden Statuswerte aufweisen: _RUNNING_, _CNAME\_CONFIGURATION_ oder _SSL\_CONFIGURATION_.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### startDomainMapping
Startet eine CDN-Domänenzuordnung basierend auf `uniqueId`. Die Domänenzuordnung muss den Status _STOPPED_ aufweisen, damit sie gestartet werden kann.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### stopDomainMapping
Stoppt eine CDN-Domänenzuordnung basierend auf `uniqueId`. Die Domänenzuordnung muss den Status _RUNNING_ aufweisen, damit das Stoppen eingeleitet werden kann.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### updateDomainMapping
Ermöglicht dem Benutzer das Aktualisieren der durch `uniqueId` identifizierten Zuordnung. Die folgenden Felder können geändert werden: `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path`. Wenn Ihr Pfad den Ursprungstyp 'Objektspeicher' (Object Storage) aufweist, können auch `bucketName` und `fileExtension` geändert werden. Die Domänenzuordnung muss den Status _RUNNING_ aufweisen, damit eine Aktualisierung erfolgen kann.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___
### listDomainMappings
Gibt eine Sammlung aller Domänen für einen bestimmten Kunden zurück.

 * **Parameter**: _keine_ 
 * **Rückgabe**: Eine Sammlung des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___ 
### listDomainMappingByUniqueId
Gibt eine Sammlung mit einem einzelnen Domänenobjekt zurück, basierend auf der Kennung `uniqueId` eines CDN.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung mit einzelnen Objekten des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`
___

## API für Bereinigung
### createPurge
Erstellt einen Bereinigungsdatensatz und fügt ihn in die Datenbank ein.

 * **Parameter**: `string` `uniqueId`, `string` `path`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### getPurgeHistoryPerMapping
Gibt den Bereinigungsverlauf für ein CDN basierend auf `uniqueId` und dem Status `saved` zurück. (Der Wert für `saved` ist standardmäßig auf _unsaved_ gesetzt.)

 * **Parameter**: `string` `uniqueId`, `int` `saved`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___
### saveOrUnsavePurgePath
Aktualisiert den Status des Pfadeintrags für Bereinigung, um anzugeben, ob der Bereinigungspfad gespeichert werden soll. Erstellt eine neue gespeicherte Bereinigung (`saved`), wenn ein Bereinigungspfad gespeichert ist. Löscht einen gespeicherten Bereinigungsdatensatz, wenn der Pfad nicht gespeichert (`unsaved`) ist.

 * **Parameter**: `string` `uniqueId`, `string` `path`, `int` `saved`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`
___

## API für Lebensdauer
### createTimeToLive
Erstellt ein neues Objekt `TimeToLive` und fügt es in die Datenbank ein.

 * **Parameter**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Aktualisiert ein vorhandenes Objekt `TimeToLive`. Wenn die Eingaben _oldTtl_ und _newTtl_ gleich sind, wird diese Funktion frühzeitig beendet.

 * **Parameter**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Löscht ein vorhandenes Objekt `TimeToLive` aus der Datenbank.

 * **Parameter**: `string` `uniqueId`, `string` `pathName`
 * **Rückgabe**: Eine Zeichenfolge mit dem Status des Löschvorgangs
___
### listTtl
Listet vorhandene Objekte `TimeToLive` basierend auf der Kennung `uniqueId` eines CDN auf.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Ein Array mit Objekten des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___

## API für Ursprung
### createOriginPath
Erstellt einen Ursprungspfad für ein vorhandenes CDN und für einen bestimmten Kunden. Der Ursprungspfad kann auf einem Hostserver oder dem Objektspeicher basieren. Die Domänenzuordnung muss den Status _RUNNING_ oder _CNAME\_CONFIGURATION_ aufweisen, damit der Ursprungspfad erstellt werden kann.  

 * **Parameter**: Ein Objekt `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`, das erwartet, dass die folgenden Eigenschaften festgelegt werden: `domainName`, `vendorName`, `path`, `originType` und `origin`. Wenn der Ursprungstyp 'Server' ist, muss außerdem `httpPort` und/oder `httpsPort` festgelegt werden. Wenn der Ursprungstyp 'Objektspeicher' (Object Storage) ist, muss auch `bucketName` angegeben werden sowie optional die Eigenschaft `fileExtension`.  
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

___ 
### updateOrigin
Aktualisiert einen vorhandenen Ursprungspfad für eine vorhandene Zuordnung und für einen bestimmten Kunden. Die Eigenschaft `originType` kann nicht geändert werden. Nur die folgenden Eigenschaften können geändert werden: `path`, `origin`, `httpPort` und `httpsPort`. Die Domänenzuordnung muss den Status _RUNNING_ oder _CNAME\_CONFIGURATION_ aufweisen, damit sie aktualisiert werden kann.

 * **Parameter**: Ein Objekt `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`, das erwartet, dass die folgenden Eigenschaften festgelegt werden: `domainName`, `vendorName`, `path`, `originType` und `origin`. Wenn der Ursprungstyp 'Server' ist, muss außerdem `httpPort` und/oder `httpsPort` festgelegt werden. Wenn der Ursprungstyp 'Objektspeicher (Object Storage) ist, muss auch `bucketName` sowie optional `fileExtension` angegeben werden.  
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___ 
### deleteOriginPath
Löscht einen vorhandenen Ursprungspfad für ein vorhandenes CDN und für einen bestimmten Kunden. Die Domänenzuordnung muss den Status _RUNNING_ oder _CNAME\_CONFIGURATION_ aufweisen, damit sie gelöscht werden kann.  

 * **Parameter**: `string` `uniqueId`, `string` `path`
 * **Rückgabe**: Eine Statusnachricht, wenn das Löschen erfolgreich ausgeführt wurde, andernfalls eine Ausnahmebedingung.

___
### listOriginPath
Listet den Ursprungspfad für eine vorhandene Zuordnung und für einen bestimmten Kunden auf.

 * **Parameter**: `string` `uniqueId`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`
___

## API für Metriken
### getCustomerUsageMetrics
Gibt die Gesamtzahl der vordefinierten Statistiken für die direkte Anzeige (ohne Grafik) für ein Kundenkonto über einen angegebenen Zeitraum zurück.

 * **Parameter**: `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingUsageMetrics
Gibt die Gesamtzahl der vordefinierten Statistiken für die direkte Anzeige für die angegebene Zuordnung zurück. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___ 
### getMappingHitsMetrics
Gibt die Gesamtzahl der Treffer mit einer bestimmten Häufigkeit über einen angegebenen Zeitraum pro Domänenzuordnung zurück. Als Häufigkeit kann 'day' (Tag), 'week' (Woche) oder 'month' (Monat) angegeben werden. Dabei liefert jedes Intervall einen Diagrammpunkt für eine Grafik. Die zurückgegebenen Daten werden basierend auf `startDate`, `endDate` und `frequency` geordnet. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Gibt die Gesamtzahl der Treffer mit einer bestimmten Häufigkeit über einen angegebenen Zeitraum zurück. Als Häufigkeit kann 'hour' (Stunde), 'day' (Tag), 'week' (Woche) oder 'month' (Monat) angegeben werden. Dabei liefert jedes Intervall einen Diagrammpunkt für eine Grafik. Die zurückgegebenen Daten müssen basierend auf `startDate`, `endDate` und `frequency` geordnet werden. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Gibt die Anzahl der Edge-Treffer pro Region für ein einzelnes CDN zurück. Die Regionen können bei jedem Anbieter variieren. Pro Zuordnung.

 * **Parameter**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Gibt die Gesamtzahl der vordefinierten Statistiken für die direkte Anzeige (ohne Grafik) für ein Kundenkonto über einen angegebenen Zeitraum zurück. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **Rückgabe**: Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
