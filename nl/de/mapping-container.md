---

copyright:
  years: 2017
lastupdated: "2017-11-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Zuordnungscontainer  
Die Sammlung `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` enthält die Attribute, die von Ihren Zuordnungs-APIs genutzt werden. Alle Zuordnungs-APIs geben eine Sammlung dieses Typs zurück.

Klasse `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: Der Name eines gültigen IBM Cloud CDN-Anbieters.
* `uniqueId`: Eine systemgenerierte, 10-stellige Kennung, die für jede Zuordnung eindeutig ist. Wird bei Erstellung einer Zuordnung generiert.
* `domain`: Der primäre CDN-Name. Wird auch als `hostname` bezeichnet.
* `protocol`: Das Protokoll, das zur Einrichtung von Services verwendet wird. Dabei kann es sich um HTTP, HTTPS oder HTTP_AND_HTTPS (eine Kombination aus HTTP und HTTPS) handeln.
* `originType`: Der Typ des Ursprungshosts, gegenwärtig 'HOST_SERVER' oder 'OBJECT_STORAGE'.
* `originHost`: Die Ursprungsserveradresse (entweder der Hostname oder die IPv4-Adresse des Ursprungsservers), von dessen Endpunkt Inhalte oder der Name des Buckets, in dem Inhalte gespeichert sind, abgerufen werden sollen. Muss weniger als 511 Zeichen lang sein.
* `httpPort`: Die Nummer des Ports, der für das HTTP-Protokoll verwendet wird. Akamai besitzt bestimmte Einschränkungen in Bezug auf Portnummern für HTTP- und HTTPS-Ports. In den [häufig gestellten Fragen (FAQs)](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie Informationen zu den zulässigen Portnummern.
* `httpsPort`: Die Nummer des Ports, der für das HTTPS-Protokoll verwendet wird. Akamai besitzt bestimmte Einschränkungen in Bezug auf Portnummern für HTTP- und HTTPS-Ports. In den [häufig gestellten Fragen (FAQs)](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie Informationen zu den zulässigen Portnummern.
* `cname`: Der Eintrag des kanonischen Namens, der als Aliasname für den Hostnamen fungiert. Dieser Name kann vom Benutzer angegeben oder durch das System generiert werden. Wenn er vom Benutzer angegeben wird, muss er weniger als 64 alphanumerische Zeichen enthalten und er muss eindeutig sein. Wenn kein Name angegeben wird, wird bei Erstellung der Zuordnung ein Name generiert.
* `respectHeaders`: Ein boolescher Wert, der beim Wert 'true' veranlasst, dass die TTL-Einstellungen im Ursprungsserver die TTL-Einstellungen des CDN überschreiben.
* `header`: Gibt die Informationen für den Host-Header an, die vom Ursprungsserver verwendet werden.
* `status`: Der Status der Zuordnung. Der Status kann den Wert CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED oder ERROR haben.
* `bucketName`: Der Bucketname für Ihren S3-Objektspeicher.
* `fileExtension`: Die Dateierweiterungen, deren Speicherung im Cache zugelassen wird.
* `path`: Der Pfad zu Ihrem S3-Objektspeicher. Befindet sich in der Regel im Abschnitt für die Objektspeicher-URL oder -API Ihres Service.
* `cacheKeyQueryRule`: Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar:
  * `include-all`: Schließt alle Abfrageargumente ein. **Standardwert**.
  * `ignore-all`: Ignoriert alle Abfrageargumente.
  * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten`: Ignoriert diese bestimmten Abfrageargumente. Beispiel: "ignore: abfrage1 abfrage2"
  * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten`: Schließt diese bestimmten Abfrageargumente ein. Beispiel: "include: abfrage1 abfrage2"

Speziell zu beachten ist das Attribut `uniqueId`, dessen Wert generiert wird, wenn eine Zuordnung erstellt wird, und dann als Parameter für nachfolgende API-Aufrufe verwendet wird.

Beispiel: Der folgende Aufruf von `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid(d'750352919747xxx');  
```

gibt ein Objekt wie das folgende zurück:

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    [vendorName] => akamai
                    [uniqueId] => 750352919747xxx
                    [domain] => test.testingcdn.net
                    [protocol] => HTTP_AND_HTTPS
                    [originType] => HOST_SERVER
                    [originHost] => test.testingcdn.net
                    [httpPort] => 80
                    [httpsPort] => 443
                    [cname] => test.cdnedge.bluemix.net
                    [performanceConfiguration] =>
                    [certificateType] =>
                    [respectHeaders] => 1
                    [header] =>
                    [status] => RUNNING
                    [bucketName] =>
                    [fileExtension] =>
                    [path] => /
                    [cacheKeyQueryRule] => include-all
                )
```


