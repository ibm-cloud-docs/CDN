---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Fehlerbehebung

Im Folgenden werden verschiedene Vorgehensweisen zum Beheben von Fehlern beschrieben, die bei Ihrem CDN auftreten. Wenn es erforderlich sein sollte, Unterstützung anzufordern, achten Sie darauf, dass Sie dabei die Referenznummer Ihres CDN angeben.

## Wie weiß ich, dass mein CDN ordnungsgemäß ausgeführt wird?
Führen Sie den folgenden `curl`-Befehl aus und ersetzen Sie dabei den Pfad `http://your.cdn.domain/uri` durch den entsprechenden Dateipfad in Ihrem CDN:

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

Weist die Ausgabe des `curl`-Befehls ein Format ähnlich dem folgenden Beispielformat auf, wird das CDN ordnungsgemäß ausgeführt:

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```

## Ich habe den Fehlercode 503 erhalten. Was ist die Ursache?

Erfahrungsgemäß ist der Fehlercode 503 in den meisten Fällen durch ein Problem mit einem Zertifikat in der SSL-Zertifikatskette bedingt.

Als Fehlernachricht wird möglicherweise die folgende Nachricht angezeigt: `503 Service Unavailable` (503 Service nicht verfügbar).  

In Verbindung mit dem Fehlercode 503 wird möglicherweise auch eine Nachricht ähnlich der folgenden angezeigt: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff (Bei der Verarbeitung Ihrer Anforderung ist ein Fehler aufgetreten. Referenz #30.3598c0ba.1521745157.87201fff)`. Als Referenznummer wird möglicherweise eine andere Nummer angezeigt. In diesem Fall wird die Referenznummer in der Fehlerzeichenfolge in einen SSL-Handshake-Fehler umgesetzt.

Beheben Sie diesen Fehler, indem Sie sicherstellen, dass das SSL-Zertifikat bzw. die SSL-Zertifikate des Ursprungsservers den folgenden Kriterien entsprechen:
  * Das Zertifikat **muss** von einer Zertifizierungsstelle stammen, die von Akamai als vertrauenswürdig anerkannt wird. Eine Liste der von Akamai als vertrauenswürdig anerkannten Zertifikate können Sie über diesen [Link](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates) aufrufen.
  * Das Zertifikat **muss** dem für das CDN konfigurierten *Host-Header* entsprechen.
  * Es darf sich **nicht** um ein selbst signiertes Zertifikat handeln.
  * Das Zertifikat darf **nicht** abgelaufen sein.

Rufen Sie die Seite [Hilfe und Unterstützung anfordern](getting-help.html#gettinghelp) auf, wenn Sie die Zertifikatskette des Ursprungsservers anhand der oben angegebenen Kriterien überprüft haben und der Fehler trotzdem weiterhin auftritt. Notieren Sie sich die Referenzfehlerzeichenfolge und geben Sie diese Zeichenfolge bei allen Mitteilungen an den Support an.

## Mein Hostname wird im Browser nicht geladen, wenn IBM Cloud Object Storage (COS) der Ursprungsobjektspeicher ist.

Wenn das IBM Cloud-CDN für die Verwendung von IBM COS als Objektspeicher konfiguriert ist, ist ein direkter Zugriff auf die Website nicht möglich. Sie müssen den vollständigen Anforderungspfad (z. B. `www.example.com/index.html`) in der Adressleiste des Browsers eingeben. Dieses Verhalten ist durch die Indexdokumenteinschränkung in IBM COS bedingt.

## Ich kann keine Verbindung über einen `curl`-Befehl oder einen Browser herstellen, wenn ich den Hostnamen mit HTTPS verwende.

Wurde Ihr CDN mit der Wildcard-Zertifikatsoption für HTTPS erstellt, muss die Verbindung über den Wert für CNAME, z. B. `https://www.exampleCname.cdnedge.bluemix.net`, hergestellt werden. Dies betrifft **alle** CDNs mit HTTPS, die vor dem 18 Juni 2018 erstellt wurden. Der Versuch, eine Verbindung mit dem Hostnamen herzustellen, führt zu einem Fehler.

## Welches Verhalten ist für die unterstützten Protokolle beim Laden des Werts für CNAME oder des Hostnamens zu erwarten?

Der folgenden Tabelle können Sie das erwartete Verhalten für die unterstützten Protokolle beim Laden des **Hostnamens** sowie beim Laden des Werts für **CNAME** über den Webbrowser entnehmen.

<table>
<caption caption-side=“top”>Tabelle zum erwarteten Verhalten</caption>
<thead>
<tr>
<th rowspan=2 scope="col">Browser-URL</th>
<th rowspan=2 scope="col">CDN mit HTTP-Protokoll (ausschließlich)</th>
<th colspan=2 scope="col">CDN mit HTTPS-Protokoll (ausschließlich)</th>
<th colspan=2 scope="col">CDN mit HTTP- und HTTPS-Protokoll</th>
</tr>
<tr>
<th scope="col"> Wildcard </th>
<th scope="col"> Gemeinsam genutztes SAN </th>
<th scope="col"> Wildcard </th>
<th scope="col"> Gemeinsam genutztes SAN </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> Laden erfolgreich</td>
<td> 301 Moved permanently (dauerhaft auf neue URL verschoben)</td>
<td> Laden erfolgreich</td>
<td> 301 Moved permanently (dauerhaft auf neue URL verschoben)</td>
<td> Laden erfolgreich</td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> Access denied (Zugriff verweigert)</td>
<td> Weiterleitung an IBM Cloud-Webseite</td>
<td> Laden erfolgreich</td>
<td> Weiterleitung an IBM Cloud-Webseite</td>
<td> Laden erfolgreich</td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 Moved permanently (dauerhaft auf neue URL verschoben)</td>
		<td> Laden erfolgreich</td>
		<td> 301 Moved permanently (dauerhaft auf neue URL verschoben)</td>
		<td> Laden erfolgreich</td>
		<td> 301 Moved permanently (dauerhaft auf neue URL verschoben)</td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> Weiterleitung an IBM Cloud-Webseite</td>
		<td> Laden erfolgreich</td>
		<td> 301 Moved permanently (dauerhaft auf neue URL verschoben)</td>
		<td> Laden erfolgreich</td>
		<td> Weiterleitung an IBM Cloud-Webseite</td>
</tr>
</tbody>
</table>

**Hinweise:**

Die Nachricht `301 Moved permanently` weist in der Regel darauf hin, dass Sie versuchen, ein CDN mit dem Protokoll `HTTPS` oder einer Kombination von HTTP- und HTTPS-Protokoll (`HTTP_AND_HTTPS`) unter Verwendung des Hostnamens zu erreichen. Aufgrund einer Einschränkung des HTTPS-Wildcardzertifikats **müssen** Sie den Wert für CNAME für den Zugriff auf Ihr CDN verwenden.

Wird bei Ihrem CDN **ausschließlich** das HTTP-Protokoll verwendet, wird bei dem Versuch, das CDN über den Wert für CNAME zu erreichen, die Fehlernachricht `301 Moved permanently` angezeigt. In diesem Fall ist der Zugriff auf das CDN _nur_ nur unter Verwendung des Hostnamens möglich.

Die Nachricht `Access denied` wird angezeigt, wenn Sie beim Zugriff auf ein CDN nicht das richtige Protokoll verwenden. Stellen Sie sicher, dass Sie für CDNs, die mit dem HTTP-Protokoll erstellt wurden, `http` verwenden und für CDNs, die mit dem HTTPS-Protokoll erstellt wurden, `https`.

Das Verhalten mit einer URL-Weiterleitung an die CDN-Webseite von IBM Cloud tritt in der Regel auf, wenn nicht die richtige URL für das Protokoll verwendet wird. Wurde Ihr CDN mit dem HTTPS-Protokoll oder einer Kombination von HTTP- und HTTPS-Protokoll (HTTPS_AND_HTTPS) erstellt, müssen Sie den Wert für CNAME für den Zugriff auf Ihr CDN verwenden. Verwenden Sie beispielsweise `https://examplecname.cdnedge.bluemix.net` für HTTPS-Zuordnungen und `http://examplecname.cdnedge.bluemix.net` oder `https://examplecname.cdnedge.bluemix.net` für HTTP_AND_HTTPS-Zuordnungen.

Die URL wird an die CDN-Webseite von IBM Cloud weitergeleitet, weil Protokoll und Domäne für das Protokoll des CDNs falsch sind. Ein CDN, das _ausschließlich_ für eine Verwendung mit dem HTTP-Protokoll erstellt wurde, kann _nur_ mithilfe des Hostnamens, z. B. `http://example.com`, erreicht werden. 
