---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Cachingdauer für den Browser mit dem Cachesteuerungsparameter `max-age` steuern

Bei Verwendung eines CDN ist für die einzelnen Ebenen, auf denen eine Zwischenspeicherung erfolgen kann, jeweils nur ein Cachingtyp verfügbar:
  * **Caching auf Browserebene** erfolgt, wenn Inhalt vom Browser für einen bestimmten Zeitraum zwischengespeichert wird.
  * **Caching auf Edge-Server-Ebene** erfolgt, wenn ein Edge-Server eines CDN Inhalte des Ursprungsservers für eine bestimmte, jedoch nicht unbedingt identische Zeit zwischenspeichert.

Die Länge des Zeitraums, in dem Inhalt zwischengespeichert wird, kann beim Caching auf Browserebene auf unterschiedliche Weise gesteuert werden. Die zu verwendende Methode hängt von den folgenden Faktoren ab:
  * Bei einer [Aktualisierung der CDN-Konfigurationsdetails](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details) wurde die Einstellung 'Header beibehalten' aktiviert oder soll aktiviert werden.
  * Der Ursprungsserver stellt einen `max-age`-Wert im Cache-Control-Header für einen bestimmten Inhalt bereit. 

Unabhängig von Änderungen bei diesen Faktoren ist es erforderlich, dass Ihr Ursprungsserver einen nicht leeren Cache-Control-Header für Ihren zielgruppenspezifischen Inhalt bereitstellt. Wird von Ihrem Ursprungsserver kein nicht leerer Cache-Control-Header für den Edge-Server bereitgestellt, wird vom Edge-Server kein eigener Cache-Control-Header für den Browser bereitgestellt.

Sendet der Edge-Server einen Cache-Control-Header an den Browser, entspricht der `max-age`-Wert für den betreffenden Cache-Control-Header der Zeit, die noch verbleibt, bis der Inhalt auf dem Edge-Server veraltet ist und einer erneuten Überprüfung durch den Ursprungsserver bedarf. 

## Header beibehalten: Aus
Ist die Option für das Beibehalten des Headers inaktiviert (Header beibehalten: **Aus**), können Sie die Cachingdauer auf Browserebene über die Konfiguration der TTL-Einstellungen des CDN steuern. Sie können z. B. für jede Datei oder jedes Dateiverzeichnis eine TTL-Regel für den zugehörigen Pfad und die gewünschte Cachingdauer erstellen.

Diese Konfiguration bewirkt Zweierlei:
  * Über die TTL-Regel wird dem Edge-Server mitgeteilt, dass der Inhalt für den angegebenen Zeitraum zwischengespeichert werden soll.
  * Wenn der Inhalt vom Edge-Server bereitgestellt wird, sendet der Edge-Server den eigenen Cache-Control-Header mit einem an der TTL-Dauer ausgerichteten `max-age`-Wert an den Browser.

## Header beibehalten: Ein
Ist die Option **Header beibehalten** aktiviert (**Ein**), können Sie sich gegen das Konfigurieren einer TTL-Regel entscheiden, mit der die Cachingdauer auf Browserebene für Ihren Inhalt gesteuert wird. In diesem Fall müssen Sie beim Ursprungsserver das Bereitstellen eines Cache-Control-Headers mit einem `max-age`-Wert für einen gegebenen Inhalt konfigurieren. Da das Beibehalten des Headers aktiviert ist, wird der `max-age`-Cachesteuerungswert des Ursprungsservers vom Edge-Server als TTL-Dauer für den betreffenden Inhalt verwendet. Auch wenn Sie bereits über einen TTL-Regeleintrag für Ihr CDN verfügen, der den betreffenden Inhalt einschließt, wird der `max-age`-Wert dennoch vom Edge-Server verwendet und möglicherweise auf Edge-Server-Ebene vorhandene Werte für die Cachingdauer werden für den betreffenden Inhalt außer Kraft gesetzt.

Diese Konfiguration bewirkt Zweierlei:
  * Über die Anweisung des Ursprungsservers für den `max-age`-Cachesteuerungswert wird dem Edge-Server mitgeteilt, dass der Inhalt für den angegebenen Zeitraum zwischengespeichert werden soll.
  * Wird der Inhalt vom Edge-Server bereitgestellt, sendet der Edge-Server den eigenen Cache-Control-Header mit einem `max-age`-Wert, der am `max-age`-Wert des Ursprungsheaders ausgerichtet ist, an den Browser.

Wenn Sie die im Beispiel angegebene zielgruppenspezifische Methode verwenden, sind keine weiteren Inhalte, die die betreffende TTL-Regel einschließt, betroffen.

Ist die Option **Header beibehalten** aktiviert (**Ein**), kann die Cachingdauer des Browsers über eine TTL-Regel gesteuert werden, wenn der Ursprungsserver keine Anweisung für den `max-age`-Wert im Cache-Control-Header sendet. Ist ein `max-age`-Wert angegeben, überschreibt dieser Wert den Zeitwert in der TTL-Regel möglicherweise versehentlich.

## Zusammenfassung

|Header beibehalten|Ursprungsserver stellt Cachesteuerung bereit|Cachingdauer für bestimmte Inhalte auf Edge-Server|Edge-Server stellt Cachesteuerung bereit|
|---|---|---|---|
|Ein|Ja, gibt `max-age`-Wert an.|Wird mit `max-age`-Wert von Ursprungsserver überschrieben|Ja, `max-age` ist die Zeit, die verbleibt, bis der Inhalt des Edge-Servers vom Ursprungsserver erneut geprüft werden muss.|
|Ein|Ja, gibt keinen `max-age`-Wert an.|Basiert auf TTL-Konfiguration des CDN|Ja, `max-age` ist die Zeit, die verbleibt, bis der Inhalt des Edge-Servers vom Ursprungsserver erneut geprüft werden muss.|
|Ein|Nein|Basiert auf TTL-Konfiguration des CDN|Nein|
|Aus|Ja, gibt `max-age`-Wert an.|Basiert auf TTL-Konfiguration des CDN|Ja, `max-age` ist die Zeit, die verbleibt, bis der Inhalt des Edge-Servers vom Ursprungsserver erneut geprüft werden muss.|
|Aus|Ja, gibt keinen `max-age`-Wert an.|Basiert auf TTL-Konfiguration des CDN|Ja, `max-age` ist die Zeit, die verbleibt, bis der Inhalt des Edge-Servers vom Ursprungsserver erneut geprüft werden muss.|
|Aus|Nein|Basiert auf TTL-Konfiguration des CDN|Nein|

## Weitere Informationen
* Informationen zur [Verwaltung des CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Cachesteuerung gemäß Abschnitt 14.9 von [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
