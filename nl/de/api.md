---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

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


# CDN - API-Referenz
{: #cdn-api-reference}

Die von {{site.data.keyword.cloud}} bereitgestellte {{site.data.keyword.cloud}} Application Programming Interface (allgemein bezeichnet als SLAPI) ist die Entwicklungsschnittstelle, über die Anwendungsentwickler und Systemadministratoren direkt mit dem Back-End-System von {{site.data.keyword.cloud_notm}} interagieren können.

Die SLAPI implementiert viele der Funktionen im Kundenportal: Wenn eine Interaktion im Kundenportal möglich ist, kann sie auch in der SLAPI erreicht werden. Da Sie mit allen Teilen der {{site.data.keyword.cloud_notm}}-Infrastrukturumgebung über das Programm interagieren können, können Sie über die SLAPI Tasks automatisieren.

Die SLAPI ist ein RPC-System (RPC, Remote Procedure Call). Bei jedem Aufruf werden Daten an einen API-Endpunkt gesendet und als Antwort strukturierte Daten empfangen. Das verwendete Format zum Senden und Empfangen von Daten über die SLAPI hängt davon ab, welche Implementierung der API Sie verwenden. Die SLAPI verwendet derzeit SOAP, XML-RPC oder REST für die Datenübertragung.

Weitere Informationen zur SLAPI oder zu den APIs des Service 'Content Delivery Network' (CDN) von {{site.data.keyword.cloud_notm}} finden Sie in den folgenden Ressourcen im {{site.data.keyword.cloud_notm}} Development Network:

* [Übersicht über SLAPI](https://softlayer.github.io/ )
* [Einführung in SLAPI](https://softlayer.github.io/article/getting-started/ )
* [API 'SoftLayer_Product_Package'](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [Handbuch zur PHP-basierten SOAP-API](https://softlayer.github.io/article/php/ )

----

Nachfolgend finden Sie als Erstes die empfohlene Reihenfolge von API-Aufrufen:
* `listVendors`: Stellt die Liste der unterstützten Anbieter bereit.
* `verifyOrder`: Prüft, ob die Bestellung aufgegeben werden kann.
* `placeOrder`: Erstellt das CDN-Konto mit einem bestimmten Anbieter. Bis zu 10 CDN-Zuordnungen können nach einem erfolgreichen Aufruf 'placeOrder' erstellt werden.
* `createDomainMapping`: Erstellt die CDN-Zuordnungen.
* `verifyDomainMapping`: Ändert den CDN-Status in _RUNNING_.

Sie können die anderen APIs verwenden, nachdem Sie die vorherige Reihenfolge befolgt haben.

[Für jeden Schritt in dieser Aufrufreihenfolge steht Beispielcode zur Verfügung.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

Sie **müssen** den API-Benutzernamen und den API-Schlüssel eines Benutzers mit der Berechtigung `CDN_ACCOUNT_MANAGE` für die meisten der API-Aufrufe verwenden, die in diesem Dokument gezeigt werden. Stimmen Sie mit dem Masterbenutzer Ihres Kontos ab, ob diese Berechtigung für Sie aktiviert sein muss. (Jedes IBM Cloud-Kundenkonto wird mit einem Masterbenutzer bereitgestellt.)
{: note}

----
## API für Anbieter
{: #api-for-vendor}

### listVendors
Mit dieser API kann der Benutzer die unterstützten CDN-Anbieter auflisten. Der Anbietername (`vendorName`) wird zum Erstellen eines CDN-Kontos und zum Bestellen Ihres CDN benötigt.

* **Erforderliche Parameter:** Keine
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Vendor`.

  Informationen und ein Verwendungsbeispiel zum Anbietercontainer finden Sie unter [Vendor-Container](/docs/infrastructure/CDN?topic=CDN-vendor-container).

----
## API für Konto
{: #api-for-account}

### verifyCdnAccountExists
Prüft, ob ein CDN-Konto für den Benutzer vorhanden ist, der die API für den angegebenen Anbieternamen (`vendorName`) aufruft.

* **Erforderliche Parameter:** `vendorName` - Geben Sie den Namen eines gültigen CDN-Anbieters an.
* **Rückgabe:** `true`, wenn ein Konto vorhanden ist, andernfalls `false`.

----
## API für Domänenzuordnung
{: #api-for-domain-mapping}

### createDomainMapping
Diese Funktion erstellt anhand der bereitgestellten Eingaben eine Domänenzuordnung für den angegebenen Anbieter und ordnet sie der {{site.data.keyword.cloud_notm}}-Infrastruktur-Konto-ID des Benutzers zu. Das CDN-Konto muss zuerst mit `placeOrder` erstellt werden, damit diese API funktioniert. (Ein Beispiel für den Aufruf der API `placeOrder` finden Sie in den [Codebeispielen](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api).) Nachdem das CDN erfolgreich erstellt wurde, wird ein Element `defaultTTL` mit dem Wert 3600 Sekunden erstellt.

* **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Sie können alle Attribute im Eingabecontainer hier anzeigen:

  [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

  Die folgenden Attribute gehören zum Eingabecontainer und werden möglicherweise bereitgestellt, wenn eine Domänenzuordnung erstellt wird (Attribute sind optional, sofern nicht anders angegeben):
    * `vendorName`: **Erforderlich** - Geben Sie den Namen eines gültigen IBM Cloud CDN-Anbieters an.
    * `origin`: **Erforderlich** - Geben Sie die Adresse des Ursprungsservers in Form einer Zeichenfolge an.
    * `originType`: **Erforderlich** - Der Ursprungstyp kann `HOST_SERVER` oder `OBJECT_STORAGE` sein.
    * `domain`: **Erforderlich** - Geben Sie Ihren Hostnamen in Form einer Zeichenfolge an.
    * `protocol`: **Erforderlich** - Unterstützte Protokolle sind `HTTP`, `HTTPS` oder `HTTP_AND_HTTPS`.
    * `certificateType`: **Erforderlich** für HTTPS-Protokoll. `SHARED_SAN_CERT` oder `WILDCARD_CERT`
    * `path`: Der Pfad, aus dem der im Cache gespeicherte Inhalt zugestellt wird. Standardpfad ist `/*`
    * `httpPort` und/oder `httpsPort`: (**Erforderlich** für Host-Server) - Diese beiden Optionen müssen dem gewünschten Protokoll entsprechen. Für das Protokoll `HTTP` muss `httpPort` festgelegt werden und `httpsPort` darf _nicht_ festgelegt werden. Analog muss für das Protokoll `HTTPS` das Attribut `httpsPort` festgelegt werden und `httpPort` darf _nicht_ festgelegt werden. Für die Protokollangabe `HTTP_AND_HTTPS` _müssen_ _sowohl_ `httpPort` als auch `httpsPort` festgelegt werden. Akamai hat bestimmte Einschränkungen in Bezug auf Portnummern. In den [häufig gestellten Fragen (FAQs)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie Informationen zu den zulässigen Portnummern.
    * `header`: Gibt die Informationen für den Host-Header an, die vom Ursprungsserver verwendet werden.
    * `respectHeader`: Ein boolescher Wert, der beim Wert `true` veranlasst, dass die TTL-Einstellungen im Ursprungsserver die TTL-Einstellungen des CDN überschreiben.
    * `cname`: Gibt einen Aliasnamen für den Hostnamen an. Wird generiert, wenn kein Name angegeben wird.
    * `bucketName`: (**Erforderlich** nur für Object Storage) Der Bucketname für Ihren S3-Objektspeicher.
    * `fileExtension`: (Optional für Object Storage) Die Dateierweiterungen, deren Speicherung im Cache zugelassen wird.
    * `cacheKeyQueryRule`: Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar. Wenn keine Argumente für `cacheKeyQueryRule` angegeben werden, wird der Standardwert "include-all" verwendet.
      * `include-all` - Schließt alle Abfrageargumente ein. **Standardwert**.
      * `ignore-all` - Ignoriert alle Abfrageargumente.
      * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Ignoriert diese bestimmten Abfrageargumente. Beispiel: `ignore: abfrage1 abfrage2`
      * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Schließt diese bestimmten Abfrageargumente ein. Beispiel: `include: abfrage1 abfrage2`

* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  **Hinweis:** Die Sammlung stellt einen Wert `uniqueId` bereit, der als Eingabe für nachfolgende API-Aufrufe für Zuordnung (Mapping) und Ursprungspfad (Origin Path) gesendet werden muss.

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Löscht die Domänenzuordnung basierend auf `uniqueId`. Die Domänenzuordnung muss einen der folgenden Statuswerte aufweisen: _RUNNING_ (Aktiv), _STOPPED_ (Gestoppt), _DELETED_ (Gelöscht), _ERROR_ (Fehler), _CNAME_CONFIGURATION_ oder _SSL_CONFIGURATION_ (CNAME-Konfiguration).

* **Erforderliche Parameter:**: `uniqueId` - Die eindeutige ID der Zuordnung, die gelöscht werden soll.
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Prüft den Status des CDN und aktualisiert den Status (Attribut `status`) der CDN-Zuordnung, wenn er geändert wurde. Wenn eine CDN-Zuordnung zu Anfang erstellt wird, weist sie den Status _CNAME_CONFIGURATION_ auf. An diesem Punkt müssen Sie den DNS-Eintrag aktualisieren, sodass die CDN-Zuordnung den Hostnamen auf den CNAME verweist. Wenden Sie sich an Ihren DNS-Anbieter, falls Sie Fragen zum Aktualisierungsprozess und dazu haben, wie lange es dauert, bis die Änderung im Internet verbreitet wird. Normalerweise sollte dies 15 bis 30 Minuten dauern. Anschließend muss mit dieser API `verifyDomainMapping` überprüft werden, ob die CNAME-Verkettung abgeschlossen ist. Wenn die CNAME-Verkettung abgeschlossen ist, wird der Status der CDN-Zuordnung in _RUNNING_ (Aktiv) geändert.

Diese API kann jederzeit aufgerufen werden, um den aktuellen Status der CDN-Zuordnung abzurufen. Die Domänenzuordnung muss einen der folgenden Statuswerte aufweisen: _RUNNING_ (Aktiv) oder _CNAME_CONFIGURATION_ (CNAME-Konfiguration).

* **Erforderliche Parameter:**: `uniqueId` - Die eindeutige ID der Zuordnung, die geprüft werden soll.
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Startet eine CDN-Domänenzuordnung basierend auf `uniqueId`. Die Domänenzuordnung muss den Status _STOPPED_ aufweisen, damit sie gestartet werden kann.

* **Erforderliche Parameter:** `uniqueId` - Die eindeutige ID der Zuordnung, die gestartet werden soll.
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Stoppt eine CDN-Domänenzuordnung basierend auf `uniqueId`. Die Domänenzuordnung muss den Status _RUNNING_ aufweisen, damit das Stoppen eingeleitet werden kann.

* **Erforderliche Parameter:** `uniqueId` - Die eindeutige ID der Zuordnung, die gestoppt werden soll.
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
Ermöglicht dem Benutzer das Aktualisieren der durch `uniqueId` identifizierten Zuordnung. Die folgenden Felder können geändert werden: `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header` und die Argumente für `cacheKeyQueryRule`. Beim Ursprungstyp 'Object Storage' können außerdem die Felder `bucketName` und `fileExtension` geändert werden. Die Domänenzuordnung muss den Status _RUNNING_ aufweisen, damit eine Aktualisierung erfolgen kann.

* **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Sie können alle Attribute im Eingabecontainer hier anzeigen:
  [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

  Die folgenden Attribute gehören zum Eingabecontainer und **müssen** bei der Aktualisierung einer Domänenzuordnung angegeben werden:
    * `vendorName`: Geben Sie den Namen des CDN-Anbieters für diese Zuordnung an.
    * `path`: Geben Sie den aktuellen Pfad für diese Zuordnung an.
    * `origin`: Geben Sie die Adresse des Ursprungsservers in Form einer Zeichenfolge an.
    * `originType`: Der Ursprungstyp kann `HOST_SERVER` oder `OBJECT_STORAGE` sein.
    * `domain`: Geben Sie Ihren Hostnamen an.
    * `protocol`: Unterstützte Protokolle sind `HTTP`, `HTTPS` oder `HTTP_AND_HTTPS`.
    * `httpPort` und/oder `httpsPort`: Diese beiden Optionen müssen dem gewünschten Protokoll entsprechen. Für das Protokoll `HTTP` muss `httpPort` festgelegt werden und `httpsPort` darf _nicht_ festgelegt werden. Analog muss für das Protokoll `HTTPS` das Attribut `httpsPort` festgelegt werden und `httpPort` darf _nicht_ festgelegt werden. Für die Protokollangabe `HTTP_AND_HTTPS` _müssen_ _sowohl_ `httpPort` als auch `httpsPort` festgelegt werden. Akamai hat bestimmte Einschränkungen in Bezug auf Portnummern. In den [häufig gestellten Fragen (FAQs)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie Informationen zu den zulässigen Portnummern.
    * `header`: Gibt die Informationen für den Host-Header an, die vom Ursprungsserver verwendet werden.
    * `respectHeader`: Ein boolescher Wert, der beim Wert `true` veranlasst, dass die TTL-Einstellungen im Ursprungsserver die TTL-Einstellungen des CDN überschreiben.
    * `uniqueId`: Wird nach der Erstellung der Zuordnung generiert.
    * `cname`: Geben Sie den CNAME an. Es wurde ein CNAME generiert, als die Zuordnung erstellt wurde, sofern Sie keinen angegeben hatten.
    * `bucketName`: (**Erforderlich** nur für Object Storage) Der Bucketname für Ihren S3-Objektspeicher.
    * `fileExtension`: (**Erforderlich** nur für Object Storage) Die Dateierweiterungen, deren Speicherung im Cache zugelassen wird.
    * `cacheKeyQueryRule`: Die Regeln für das Cacheschlüsselverhalten können nur für CDN-Zuordnungen aktualisiert werden, die _nach_ dem 16.11.2017 erstellt wurden. Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar:
      * `include-all` - Schließt alle Abfrageargumente ein. **Standardwert**.
      * `ignore-all` - Ignoriert alle Abfrageargumente.
      * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Ignoriert diese bestimmten Abfrageargumente. Beispiel: `ignore: abfrage1 abfrage2`
      * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Schließt diese bestimmten Abfrageargumente ein. Beispiel: `include: abfrage1 abfrage2`
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Gibt eine Sammlung aller Domänenzuordnungen für den aktuellen Kunden zurück.

* **Erforderliche Parameter:** Keine
* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Gibt eine Sammlung mit einem einzelnen Domänenobjekt basierend auf der Kennung `uniqueId` eines CDN zurück.

* **Erforderliche Parameter:** `uniqueId` - Die eindeutige ID der Zuordnung, die zurückgegeben werden soll.
* **Rückgabe:** Eine Einzelobjektsammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Zuordnungscontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## APIs für Ursprung
{: #apis-for-origin}

### createOriginPath
Erstellt einen Ursprungspfad für ein vorhandenes CDN und für einen bestimmten Kunden. Der Ursprungspfad kann auf einem Host-Server oder auf Object Storage basieren. Zum Erstellen des Ursprungspfads muss die Domänenzuordnung den Status _RUNNING_ oder _CNAME_CONFIGURATION_ aufweisen.

* **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Sie können alle Attribute im Eingabecontainer hier anzeigen:

  [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

  Die folgenden Attribute gehören zum Eingabecontainer und werden möglicherweise bereitgestellt, wenn ein Ursprungspfad erstellt wird (Attribute sind optional, sofern nicht anders angegeben):
    * `vendorName`: **Erforderlich** - Geben Sie den Namen eines gültigen IBM Cloud CDN-Anbieters an.
    * `origin`: **Erforderlich** - Geben Sie die Adresse des Ursprungsservers in Form einer Zeichenfolge an.
    * `originType`: **Erforderlich** - Der Ursprungstyp kann `HOST_SERVER` oder `OBJECT_STORAGE` sein.
    * `domain`: **Erforderlich** - Geben Sie Ihren Hostnamen in Form einer Zeichenfolge an.
    * `protocol`: **Erforderlich** - Unterstützte Protokolle sind `HTTP`, `HTTPS` oder `HTTP_AND_HTTPS`.
    * `path`: Der Pfad, aus dem der im Cache gespeicherte Inhalt zugestellt wird. Muss mit dem Zuordnungspfad beginnen. Beispiel: Wenn der Zuordnungspfad `/test` ist, kann der Ursprungspfad `/test/media` sein.
    * `httpPort` und/oder `httpsPort`: **Erforderlich** - Diese beiden Optionen müssen dem gewünschten Protokoll entsprechen. Für das Protokoll `HTTP` muss `httpPort` festgelegt werden und `httpsPort` darf _nicht_ festgelegt werden. Analog muss für das Protokoll `HTTPS` das Attribut `httpsPort` festgelegt werden und `httpPort` darf _nicht_ festgelegt werden. Für die Protokollangabe `HTTP_AND_HTTPS` _müssen_ _sowohl_ `httpPort` als auch `httpsPort` festgelegt werden. Akamai hat bestimmte Einschränkungen in Bezug auf Portnummern. In den [häufig gestellten Fragen (FAQs)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie Informationen zu den zulässigen Portnummern.
    * `header`: Gibt die Informationen für den Host-Header an, die vom Ursprungsserver verwendet werden.
    * `uniqueId`: **Erforderlich** - Wird nach der Erstellung der Zuordnung generiert.
    * `cname`: Gibt einen Aliasnamen für den Hostnamen an. Wenn Sie keinen eindeutigen CNAME angegeben haben, wurde beim Erstellen der Zuordnung ein solcher Name für Sie generiert.
    * `bucketName`: (**Erforderlich** für Object Storage) Der Bucketname für Ihren S3-Objektspeicher.
    * `fileExtension`: (Optional für Object Storage) Die Dateierweiterungen, deren Speicherung im Cache zugelassen wird.
    * `cacheKeyQueryRule`: Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar:
      * `include-all` - Schließt alle Abfrageargumente ein. **Standardwert**.
      * `ignore-all` - Ignoriert alle Abfrageargumente.
      * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Ignoriert diese bestimmten Abfrageargumente. Beispiel: `ignore: abfrage1 abfrage2`
      * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Schließt diese bestimmten Abfrageargumente ein. Beispiel: `include: abfrage1 abfrage2`

* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Ursprungspfadcontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Aktualisiert einen vorhandenen Ursprungspfad für eine vorhandene Zuordnung und für einen bestimmten Kunden. Der Ursprungstyp kann mit dieser API nicht geändert werden. Die folgenden Eigenschaften können geändert werden: `path`, `origin`, `httpPort` und `httpsPort`, `header` und Argumente für `cacheKeyQueryRule`. Zum Aktualisieren muss die Domänenzuordnung den Status _RUNNING_ oder _CNAME_CONFIGURATION_ aufweisen.

* **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Sie können alle Attribute im Eingabecontainer hier anzeigen:

  [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

  Die folgenden Attribute gehören zum Eingabecontainer und werden möglicherweise bereitgestellt, wenn ein Ursprungspfad aktualisiert wird (Attribute sind optional, sofern nicht anders angegeben):
    * `oldPath`: **Erforderlich** - Der aktuelle Pfad, der geändert werden soll.
    * `origin`: (**Erforderlich** bei Aktualisierung) - Geben Sie die Adresse des Ursprungsservers in Form einer Zeichenfolge an.
    * `originType`: **Erforderlich** - Der Ursprungstyp kann `HOST_SERVER` oder `OBJECT_STORAGE` sein.
    * `path`: **Erforderlich** - Der neue Pfad, der hinzugefügt werden soll. Gilt relativ zum Zuordnungspfad.
    * `httpPort` und/oder `httpsPort`: (**Erforderlich** für Host-Server bei Aktualisierung) - Diese beiden Optionen müssen dem gewünschten Protokoll entsprechen. Für das Protokoll `HTTP` muss `httpPort` festgelegt werden und `httpsPort` darf _nicht_ festgelegt werden. Analog muss für das Protokoll `HTTPS` das Attribut `httpsPort` festgelegt werden und `httpPort` darf _nicht_ festgelegt werden. Für die Protokollangabe `HTTP_AND_HTTPS` _müssen_ _sowohl_ `httpPort` als auch `httpsPort` festgelegt werden. Akamai hat bestimmte Einschränkungen in Bezug auf Portnummern. In den [häufig gestellten Fragen (FAQs)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie Informationen zu den zulässigen Portnummern.
    * `uniqueId`: **Erforderlich** - Die eindeutige ID der Zuordnung, zu der dieser Ursprungspfad gehört.
    * `bucketName`: (**Erforderlich** nur für Object Storage) Der Bucketname für Ihren S3-Objektspeicher.
    * `fileExtension`: (**Erforderlich** nur für Object Storage) Die Dateierweiterungen, deren Speicherung im Cache zugelassen wird.
    * `cacheKeyQueryRule`: (**Erforderlich** bei Aktualisierung) Regeln für das Cacheschlüsselverhalten können nur für Ursprungspfade aktualisiert werden, die _nach_ dem 16.11.2017 erstellt wurden. Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar:
      * `include-all` - Schließt alle Abfrageargumente ein. **Standardwert**.
      * `ignore-all` - Ignoriert alle Abfrageargumente.
      * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Ignoriert diese bestimmten Abfrageargumente. Beispiel: `ignore: abfrage1 abfrage2`
      * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten` - Schließt diese bestimmten Abfrageargumente ein. Beispiel: `include: abfrage1 abfrage2`

* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Ursprungspfadcontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Löscht einen vorhandenen Ursprungspfad für ein vorhandenes CDN und für einen bestimmten Kunden. Zum Löschen muss die Domänenzuordnung den Status _RUNNING_ oder _CNAME_CONFIGURATION_ aufweisen.

* **Erforderliche Parameter:**:
  * `uniqueId`: Die eindeutige ID der Zuordnung, zu der dieser Ursprungspfad gehört.
  * `path`: Der Pfad, der gelöscht werden soll.

* **Rückgabe:** Eine Statusnachricht, wenn das Löschen erfolgreich war. Andernfalls wird eine Ausnahmebedingung ausgelöst.

----
### listOriginPath
Listet die Ursprungspfade für eine vorhandene Zuordnung auf Basis der eindeutigen ID (`uniqueId`) auf.

* **Erforderliche Parameter:**:
  * `uniqueId`: Geben Sie die eindeutige ID der Zuordnung an, für die Sie Ursprungspfade auflisten wollen.
* **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Ursprungspfadcontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## API für Bereinigung
{: #api-for-purge}

### Containerklasse für Bereinigung:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
Erstellt einen Bereinigungsdatensatz und fügt ihn in die Datenbank ein.

* **Parameter:**
  * `uniqueId`: Die eindeutige ID der Zuordnung, für die die Bereinigung erstellt wird.
  * `path`: Der Pfad der Bereinigung, die erstellt werden soll.

* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
Gibt den Bereinigungsverlauf für ein CDN auf der Basis der eindeutigen ID (`uniqueId`) und des Speicherstatus (`saved`) zurück. (Der Wert von `saved` wird standardmäßig auf _UNSAVED_ gesetzt.)

* **Parameter:**
  * `uniqueId`: Die eindeutige ID der Zuordnung, für die der Bereinigungsverlauf abgerufen werden soll.
  * `saved`: Gibt Bereinigungen zurück, die gespeichert (_SAVED_) oder nicht gespeichert (_UNSAVED_) sind.

* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
Aktualisiert den Status des Bereinigungspfadeintrags, um anzugeben, ob dieser Bereinigungspfad gespeichert werden soll. Erstellt eine neue gespeicherte (`saved`) Bereinigung, wenn ein Bereinigungspfad gespeichert wird. Löscht den Eintrag einer gespeicherten Bereinigung, wenn der Pfad nicht gespeichert (`unsaved`) wird.

* **Parameter:**
  * `uniqueId`: Die eindeutige ID der Zuordnung, zu der die Bereinigung gehört.
  * `path`: Der Pfad der Bereinigung, die gespeichert oder nicht gespeichert werden soll.
  * `saved`: _SAVED_ (gespeichert) oder _UNSAVED_ (nicht gespeichert).

* **Rückgabe:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
## API für Lebensdauer
{: #api-for-time-to-live}

### Variablen der Klasse TimeToLive:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
Erstellt ein neues Objekt `TimeToLive` und fügt es in die Datenbank ein.

 * **Parameter:** `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Rückgabe:** Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Aktualisiert ein vorhandenes Objekt `TimeToLive`. Wenn die Eingaben _oldTtl_ und _newTtl_ gleich sind, wird diese Funktion frühzeitig beendet.

 * **Parameter:** `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Rückgabe:** Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Löscht ein vorhandenes Objekt `TimeToLive` aus der Datenbank.

 * **Parameter:** `string` `uniqueId`, `string` `pathName`
 * **Rückgabe:** Eine Zeichenfolge mit dem Status des Löschvorgangs
___
### listTtl
Listet vorhandene Objekte `TimeToLive` basierend auf der Kennung `uniqueId` eines CDN auf.

 * **Parameter:** `string` `uniqueId`
 * **Rückgabe:** Ein Array mit Objekten des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`

 ----
## API für Metriken
{: #api-for-metrics}

[Metrikcontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
Gibt die Gesamtzahl der vordefinierten Statistiken für die direkte Anzeige (ohne Grafik) für ein Kundenkonto über einen angegebenen Zeitraum zurück.

 * **Parameter:**
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingUsageMetrics
Gibt die Gesamtzahl der vordefinierten Statistiken für die direkte Anzeige für die angegebene Zuordnung zurück. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter:**
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsMetrics
Gibt die Gesamtzahl der Treffer mit einer bestimmten Häufigkeit über einen angegebenen Zeitraum pro Domänenzuordnung zurück. Als Häufigkeit kann 'day' (Tag), 'week' (Woche) oder 'month' (Monat) angegeben werden. Dabei liefert jedes Intervall einen Diagrammpunkt für eine Grafik. Die zurückgegebenen Daten werden basierend auf `startDate`, `endDate` und `frequency` geordnet. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter:**
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Gibt die Gesamtzahl der Treffer mit einer bestimmten Häufigkeit über einen angegebenen Zeitraum zurück. Als Häufigkeit kann 'day' (Tag), 'week' (Woche) oder 'month' (Monat) angegeben werden. Dabei liefert jedes Intervall einen Diagrammpunkt für eine Grafik. Die zurückgegebenen Daten müssen basierend auf `startDate`, `endDate` und `frequency` geordnet werden. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter:**
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Gibt die Anzahl der Edge-Treffer für ein einzelnes CDN zurück. Die Regionen können bei jedem Anbieter variieren. Pro Zuordnung.

 * **Parameter:**  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Gibt die Gesamtzahl der vordefinierten Statistiken für die direkte Anzeige (ohne Grafik) für eine bestimmte Zuordnung über einen angegebenen Zeitraum zurück. Der Wert für `frequency` lautet standardmäßig 'aggregate'.

 * **Parameter:**
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Rückgabe:** Eine Sammlung mit Objekten des Typs `SoftLayer_Container_Network_CdnMarketplace_Metrics`

----
## API für Geographical Access Control
{: #api-for-geographical-access-control}

### createGeoblocking
Erstellt eine neue Regel für Geographical Access Control und gibt die neu erstellte Regel zurück.

  * **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Sie können alle Attribute im Eingabecontainer hier anzeigen:

    [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

    Die folgenden Attribute gehören zum Eingabecontainer und sind beim Erstellen einer neuen Geographical Access Control-Regel **erforderlich**:
    * `uniqueId`: Die eindeutige ID der Zuordnung, zu der die Regel zugewiesen werden soll.
    * `accessType`: Gibt an, ob über die Regel Datenverkehr zu der angegebenen Region zugelassen (`ALLOW`) oder blockiert (`DENY`) wird.
    * `regionType`: Der Typ der Region, auf die die Geographical Access Control-Regel angewendet werden soll. Mögliche Typen sind `CONTINENT` und `COUNTRY_OR_REGION`.
    * `regions`: Eine Wertegruppe mit einer Auflistung der Standorte, auf die das Attribut `accessType` angewendet wird.

      Eine Liste der zur Auswahl stehenden Regionen finden Sie auf der Seite [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class).

  * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Aktualisiert eine vorhandene Geographical Access Control-Regel für eine vorhandene Domänenzuordnung und gibt die aktualisierte Regel zurück.

  * **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Sie können alle Attribute im Eingabecontainer hier anzeigen:

    [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

    Die folgenden Attribute gehören zum Eingabecontainer und werden bei der Aktualisierung einer Geographical Access Control-Regel möglicherweise bereitgestellt (Parameter sind optional, sofern nicht anders angegeben):
    * `uniqueId`: **Erforderliche** eindeutige ID der Zuordnung, zu der die zu aktualisierende Regel gehört.
    * `accessType`: Gibt an, ob über die Regel Datenverkehr zu der angegebenen Region zugelassen (`ALLOW`) oder blockiert (`DENY`) wird.
    * `regionType`: Der Typ der Region, auf die die Regel angewendet werden soll. Mögliche Typen sind `CONTINENT` und `COUNTRY_OR_REGION`.
    * `regions`: Eine Wertegruppe mit einer Auflistung der Standorte, auf die das Attribut `accessType` angewendet wird.

      Eine Liste der zur Auswahl stehenden Regionen finden Sie auf der Seite [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class).

  * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Entfernt eine vorhandene Geographical Access Control-Regel für eine vorhandene Domänenzuordnung.

  * **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Sie können alle Attribute im Eingabecontainer hier anzeigen:

    [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

    Das folgende Attribut gehört zum Eingabecontainer und ist beim Löschen einer Geographical Access Control-Regel **erforderlich**:
    * `uniqueId`: Geben Sie die eindeutige ID der Zuordnung an, zu der die zu löschende Regel gehört.

  * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Ruft das Geographical Access Control-Verhalten einer Zuordnung in der Datenbank ab.

  * **Parameter:**
    * `uniqueId`: Die eindeutige ID der Zuordnung an, zu der die Regel gehört.

  * **Rückgabe**: Ein Objekt des Typs
`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Gibt eine Liste der Typen und Regionen zurück, die für die Erstellung von Geographical Access Control-Regeln infrage kommen.

  * **Parameter:**
    * `uniqueId`: Die eindeutige ID der Domänenzuordnung.

  * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## API für Hot-Link-Schutz
{: #api-for-hotlink-protection}

### createHotlinkProtection
Erstellt einen neuen Hot-Link-Schutz und gibt das neu erstellte Verhalten zurück.

  * **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Sie können alle Attribute im Eingabecontainer hier anzeigen:

    [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

    Die folgenden Attribute gehören zum Eingabecontainer und sind beim Erstellen eines neuen Hot-Link-Schutzes **erforderlich**.
    * `uniqueId`: Die eindeutige ID der Zuordnung, der das Verhalten zugewiesen werden soll.
    * `protectionType`: Gibt an, ob der Zugriff auf Inhalte zugelassen oder verweigert werden soll, wenn eine Webseite Inhalt anfordert und dabei einen Referrer-Header-Wert verwendet, der einem der mit 'refererValues' angegebenen Begriffe entspricht.
    * `refererValues`: Eine Liste, die einzelne Leerzeichen als Trennzeichen verwendet und Begriffe für den Abgleich mit Referrer-URLs enthält, für die das Verhalten `protectionType` wirksam wird.

      Auf der Seite [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) finden Sie eine Liste gültiger Werte für den Hot-Link-Schutz.

  * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Aktualisiert ein vorhandenes Verhalten für den Hot-Link-Schutz für eine vorhandene Domänenzuordnung und gibt das aktualisierte Verhalten zurück.

  * **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Sie können alle Attribute im Eingabecontainer hier anzeigen:

    [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

    Die folgenden Attribute sind Teil des Eingabecontainers und beim Aktualisieren eines vorhandenen Hot-Link-Schutzes **erforderlich**.
    * `uniqueId`: Die eindeutige ID der Zuordnung, zu der das vorhandene Verhalten gehört.
    * `protectionType`: Gibt an, ob der Zugriff auf Inhalte zugelassen oder verweigert werden soll, wenn eine Webseite Inhalt anfordert und dabei einen Referrer-Header-Wert verwendet, der einem der mit 'refererValues' angegebenen Begriffe entspricht. 
    * `refererValues`: Eine Liste, die einzelne Leerzeichen als Trennzeichen verwendet und Begriffe für den Abgleich mit Referrer-URLs enthält, für die das Verhalten `protectionType` wirksam wird.

      Auf der Seite [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) finden Sie eine Liste gültiger Werte für den Hot-Link-Schutz.

  * **Rückgabe**: Ein Objekt des Typs `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Entfernt ein vorhandenes Verhalten für den Hot-Link-Schutz aus einer bestehenden Domänenzuordnung.

  * **Parameter:** Eine Sammlung des Typs `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Sie können alle Attribute im Eingabecontainer hier anzeigen:

    [Eingabecontainer anzeigen](/docs/infrastructure/CDN?topic=CDN-input-container)

    Die folgenden Attribute gehören zum Eingabecontainer und sind beim Erstellen eines neuen Hot-Link-Schutzes **erforderlich**.
    * `uniqueId`: Die eindeutige ID der Zuordnung, von der das Verhalten entfernt werden soll.

  * **Rückgabe**: null

----
### getHotlinkProtection
Ruft das aktuelle Verhalten für den Hot-Link-Schutz für eine Zuordnung ab.

  * **Parameter:**
    * `uniqueId`: Die eindeutige ID der Zuordnung, zu der die Bereinigung gehört.

  * **Rückgabe**: Ein Objekt des Typs
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Klasse für geografische Zugriffsblockierung anzeigen](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
