---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Eingabecontainer
{: #input-container}

Der Eingabecontainer (Input Container) ist eine Attributsammlung, die sowohl von Zuordnungsklassen (Mapping) als auch von Pfadklassen (Ursprungspfadklassen - Path) genutzt wird. Er stellt eine konsistente Gruppe von Eingabeattributen für beide Klassen bereit.

* `vendorName`: Der Name eines gültigen {{site.data.keyword.cloud}} CDN-Anbieters.
* `oldPath`: Wird von updateOriginPath() verwendet. Diese Eigenschaft speichert den Namen des aktuellen oder des 'alten' Pfads.

Die folgenden Attribute gehören zur Mapping-Klasse und zur Path-Klasse (bzw. OriginPath-Klasse):
* `originType`: Der Typ des Ursprungshosts, gegenwärtig 'HOST_SERVER' oder 'OBJECT_STORAGE'.
* `origin`: Die Ursprungsserveradresse (entweder der Hostname oder die IPv4-Adresse des Ursprungsservers), von dessen Endpunkt Inhalte oder der Name des Buckets, in dem Inhalte gespeichert sind, abgerufen werden sollen. Muss weniger als 511 Zeichen lang sein.
* `httpPort`: Die Nummer des Ports, der für das HTTP-Protokoll verwendet wird. Akamai besitzt bestimmte Einschränkungen in Bezug auf Portnummern für HTTP- und HTTPS-Ports. In den [häufig gestellten Fragen (FAQs)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie eine der Liste der zulässigen Portnummern.
* `httpsPort`: Die Nummer des Ports, der für das HTTPS-Protokoll verwendet wird. Akamai besitzt bestimmte Einschränkungen in Bezug auf Portnummern für HTTP- und HTTPS-Ports. In den [häufig gestellten Fragen (FAQs)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) finden Sie eine der Liste der zulässigen Portnummern.
* `status`: Der Status der Zuordnung bzw. des Pfads. Der Status kann den Wert CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED oder ERROR haben.
* `path`: Der Pfad, aus dem der im Cache gespeicherte Inhalt zugestellt wird. Der Standardpfad ist /\*. Bei Verwendung durch die API `updateOriginPath` gibt dieses Attribut den neuen Pfad an, der hinzugefügt werden soll.
* `performanceConfiguration`: Spezifikationen für die Leistungskonfiguration der Zuordnung.
* `cacheKeyQueryRule`: Die folgenden Optionen sind zum Konfigurieren des Cacheschlüsselverhaltens verfügbar:
  * `include-all`: Schließt alle Abfrageargumente ein.
  * `ignore-all`: Ignoriert alle Abfrageargumente.
  * `ignore: durch Leerzeichen getrennte Liste von Abfrageargumenten`: Ignoriert diese bestimmten Abfrageargumente. Beispiel: `ignore: abfrage1 abfrage2`
  * `include: durch Leerzeichen getrennte Liste von Abfrageargumenten`: Schließt diese bestimmten Abfrageargumente ein. Beispiel: `include: abfrage1 abfrage2`
* `geoblockingRule`

Die folgenden Attribute sind für die Zuordnungsklasse (Mapping) spezifisch:

* `uniqueId`: Eine systemgenerierte, 10-stellige Kennung, die für jede Zuordnung eindeutig ist. Wird bei Erstellung einer Zuordnung generiert.
* `domain`: Der primäre CDN-Name. Wird auch als Hostname bezeichnet.
* `protocol`: Das Protokoll, das zur Einrichtung von Services verwendet wird. Dabei kann es sich um HTTP, HTTPS oder HTTP_AND_HTTPS (eine Kombination aus HTTP und HTTPS) handeln.
* `cname`: Der Eintrag des kanonischen Namens, der als Aliasname für den Hostnamen fungiert. Dieser Name kann vom Benutzer angegeben oder durch das System generiert werden. Wenn er vom Benutzer angegeben wird, muss er weniger als 64 alphanumerische Zeichen enthalten und er muss eindeutig sein. Wenn kein Name angegeben wird, wird bei Erstellung der Zuordnung ein Name generiert.
* `certificateType`: Typ des Zertifikats, das von einer Zuordnung verwendet wird. Zulässige Werte sind `WILDCARD_CERT` und `SHARED_SAN_CERT`. Für HTTP-Zuordnungen ist der Wert 0.
* `respectHeaders`: Ein boolescher Wert, der beim Wert 'true' veranlasst, dass die TTL-Einstellungen im Ursprungsserver die TTL-Einstellungen des CDN überschreiben.
* `header`: Gibt die Informationen für den Host-Header an, die vom Ursprungsserver verwendet werden.

Die folgenden Attribute beziehen sich auf Cloud Object Storage (COS):  
* `bucketName`: Der eindeutige Name Ihres Buckets für den S3-Objektspeicher.  
* `fileExtension`: Die Dateierweiterungen, die zugelassen werden.

Das folgende Attribut bezieht sich auf die Konfiguration des Hot-Link-Schutzes:
* `hotlinkProtection`: Weitere Informationen finden Sie in [Klasse für Hot-Link-Schutz](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class).
