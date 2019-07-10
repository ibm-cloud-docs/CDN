---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: cross origin resource sharing, CORS, CORS request, same-origin policy, simple request, preflighted request

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CORS und CORS-Anforderungen über Ihr CDN
{: #cors-and-cors-requests-through-your-cdn}

Cross Origin Resource Sharing (CORS) ist ein Mechanismus, der von Browsern hauptsächlich verwendet wird, um die Berechtigungen für den Zugriff auf Inhalte aus einem anderen Ursprung zu überprüfen.

## Was ist CORS?
{: #what-is-cors}

Wenn ein Browser eine Webseite lädt, wird die Richtlinie **Same-Origin-Policy** erzwungen. Dies bedeutet, dass nur Inhalte aus demselben Ursprung wie die Webseite abgerufen werden können. In einigen Fällen benötigt eine Webseite jedoch Zugriff auf Assets aus mehreren Ursprüngen, die dieser Website vertrauen. Hier kommt CORS ins Spiel. 

Dieser Sicherheitsmechanismus ist nur vorhanden, wenn eine Anwendung über einen HTTP-Client verfügt oder ein HTTP-Client ist und wenn diese Anwendung CORS implementiert. Fast alle modernen Browser, z. B. Chrome, Firefox und Safari, implementieren CORS.

Ein Hinweis: **Ein Ursprung in Bezug auf CORS muss nicht mit einem CDN-Ursprung identisch sein**. Ein Ursprung in CORS wird durch ein URI-Schema, eine Domäne und eine beliebige mögliche Portnummer definiert. `https://www.example.com:1443` ist zum Beispiel ein anderer Ursprung als `http://www.example.com`. Daher kann ein CDN aus der Perspektive eines Browsers auch als CORS-Ursprung betrachtet werden.

## Funktionsweise von CORS

CORS kann zwei Typen von Anforderungen verarbeiten: _einfache Anforderungen_ und _Anforderungen mit Preflight_, die etwas komplexer sind.

### Einfache Anforderungen
{: #simple-requessts}

**Erste Anforderung (Ressourcenzugriff):**

![CORS - einfach](/images/cors-simple.png)

[Einfache Anforderungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/cors/#simple-cross-origin-request) über CORS sind entweder `GET`- oder `POST`-Anforderungen von der Webseite eines Ursprungs, die versuchen, Zugriff auf die URL eines anderen Ursprungs für Ressourcen zu erhalten.

Bei solchen Anforderungen setzt der Browser automatisch CORS-Anforderungsheader. In erster Linie setzt der Browser den Ursprung der Webseite, die die Anforderung sendet, im HTTP-Header `Origin` automatisch. Die CORS-Anforderung kann auch eine Gruppe von [bestimmten Standard-HTTP-Headern ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/cors/#simple-header) enthalten, während sie aus der Perspektive des Browsers ihren Status als einfache CORS-Anforderung beibehält.

Der Server, der die CORS-Anforderung empfängt, verarbeitet die Anforderung und sendet eine Gruppe von CORS-Antwortheadern mit den angeforderten Inhalten an den Browser zurück (oder sendet diese nicht). Diese CORS-Antwortheader enthalten Werte, die angeben, ob der aktuellen Webseite Zugriff auf diese Ressourcen erteilt wird, ob die gesendeten Header für diese Anforderung akzeptabel sind usw.

Stellt der Browser fest, dass die CORS-Antwortheader nicht seiner CORS-Anforderung entsprechen, verhindert er automatisch den Zugriff auf die Inhalte und das Laden der Inhalte. Stellt er andernfalls fest, dass der CORS-Ursprung die Berechtigung zur Verwendung der Ressource erteilt, lässt er den Zugriff auf die angeforderten Inhalte und das Laden dieser Inhalte zu.

### Anforderungen mit Preflight
{: #preflighted-requests}

**Erste Anforderung (Preflight):**

![CORS - Preflight](/images/cors-preflight.png)

Zweite Anforderung (Ressourcenzugriff):

![CORS nach Preflight](/images/cors-after-preflight.png)

Für eine komplexere CORS-Kommunikation zwischen dem Browser und einem CORS-Ursprung, der sich vom Ursprung der anfordernden Webseite unterscheidet, wäre eine [Preflight-Anforderung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) vor einem tatsächlichen Ressourcenzugriff erforderlich. In bestimmten Situationen werden Preflight-CORS-Anforderungen benötigt, z. B. HTTP-Methoden, die keine `GET`- oder `POST`-Methoden sind, oder es müssen vom Standard abweichende HTTP-Header mit der Anforderung verwendet werden, selbst wenn es sich um eine `GET`- oder `POST`-Anforderung handelt, usw.

Wenn eine Preflight-Anforderung erforderlich ist, sieht der Ereignisablauf wie folgt aus:

* Der Browser sendet eine Anforderung mit allen vorgesehenen CORS-Anforderungsheadern mit der HTTP-Methode OPTIONS an den Server. 
* Der Server verarbeitet diese CORS-Anforderungsheader und antwortet möglicherweise mit CORS-Antwortheadern, die keine eigentlichen Inhaltsdaten enthalten. 
* Der Browser überprüft diese CORS-Antwortheader, um sicherzustellen, dass die CORS-Anforderung zulässig ist. 
* Wenn der Browser feststellt, dass die beabsichtigte Ressourcenanforderung vom Server zugelassen werden müsste, sendet er eine zweite Anforderung mit der beabsichtigten HTTP-Methode (`GET`, `POST`, `PUT` etc.) an den Server, die dieselben CORS-Anforderungsheader enthält.

Danach wird die Kommunikation zwischen dem Browser und dem CORS-Ursprung (der sich vom Ursprung der Webseite unterscheidet) so fortgesetzt, als ob es sich um eine einfache CORS-Anforderung handeln würde. Ähnlich wie bei einer einfachen CORS-Anforderung sind Inhalte und Ressourcen zugänglich und können geladen werden, wenn diese zweite CORS-Anforderung ebenfalls zugelassen wird.

## Vorgehensweise zur Konfiguration von CORS an Ihrem Ursprung
{: #how-to-set-up-cors-at-your-origin}

Wie in den vorherigen Diagrammen dargestellt, wird CORS durch den anfordernden HTTP-Client eingeleitet. Die Auswirkungen hängen jedoch vom angeforderten Ursprung ab. Damit Ihre Inhalte für CORS-Anforderungen bereit sind, muss Ihr Ursprung korrekt konfiguriert sein, sodass mit den korrekten CORS-Antwortheadern und den korrekten Zugriffsberechtigungen geantwortet wird.

Das folgende Beispiel zeigt eine grundlegende CORS-Konfiguration für einen Nginx-Server:

```nginx
http {
    # Einige Konfigurationen für den HTTP-Kontext

    server {
        # Einige Konfigurationen für den Serverkontext

        # URI-Pfad zu Inhalt
        location /my-static-content {
        
            # Einige Konfigurationen für den Positionskontext
        
            # Einfache Anforderungen verarbeiten
            #
            # Nur "HTTP GET"-Anforderungen berücksichtigen (Abrufen von Inhalten)
            if ($request_method = 'GET') {
                
                # Dem Browser während des CORS nur dann Zugriff auf Daten von diesem Server gestatten,
                # wenn die Anforderung von einer Webseite aus dem folgenden Ursprung stammt
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
            }

            # Preflight-Anforderungen verarbeiten
            if ($request_method = 'OPTIONS') {
                
                # Dem Browser während des CORS nur dann Zugriff auf Daten von diesem Server gestatten,
                # wenn die Anforderung von einer Webseite aus dem folgenden Ursprung stammt
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
                
                # Nur GET-Anforderungen zulassen
                add_header 'Access-Control-Allow-Methods' 'GET';
                
                # Folgende Header in den Anforderungsheadern des Browsers zulassen, die über andere
                # Verfahren als die automatische Hinzufügung durch den Browser hinzugefügt wurden
                add_header 'Access-Control-Allow-Headers' 'pragma';
                
                # Für den Browser angeben, wie lange diese Preflight-Antwort zwischengespeichert werden soll
                add_header 'Access-Control-Max-Age' 1728000;
                
                # HTTP-Antwortcode 204 gibt die erfolgreiche Ausführung an, aber auch,
                # dass diese (Preflight-)Antwort keine Inhalte enthält
                return 204;
            }

            # Weitere Konfigurationen für den Positionskontext
            
            # Wenn es sich nicht um eine komplexe CORS-Situation handelt, die eine Preflight-Antwort erfordert:
            # Versuchen, die Datei bereitzustellen, deren Pfad mit dem URI-Pfad dieses Positionsblocks übereinstimmt
            #
            # Wenn die Datei nicht gefunden wird, den HTTP-Code 404 anzeigen (nicht gefunden)
            try_files $uri =404;
        }

        # Weitere Konfigurationen für den Serverkontext
    }

    # Weitere Konfigurationen für den HTTP-Kontext
}
```
{: screen}

Im Allgemeinen sollte der Browser Inhalte ohne Einschränkung laden, wenn er die Angabe `Access-Control-Allow-Origin: *` in den CORS-Antwortheadern des Servers entsprechend der [w3-Spezifikation hinsichtlich des entsprechenden Platzhalterwerts ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für extenen Link")](https://www.w3.org/TR/cors/#security) feststellt. Jedoch wird `Access-Control-Allow-Origin: *` nicht von allen Browsern unterstützt.

Wenn der Server den Zugriff von mehreren Webseiten unterstützen muss, die jeweils von einem anderen Ursprung bereitgestellt werden, sollte ein einzelner Ursprungswert für ` Access-Control-Allow-Origin` dynamisch pro Anforderung generiert werden. Im Folgenden sehen Sie ein grundlegendes Beispiel für einen solchen Anwendungsfall für einen Nginx-Server:

```nginx
http {
    # Einige Konfigurationen für den HTTP-Kontext

    ######
    # Eingabe von $http_origin,          dem Wert im HTTP-Header "Origin"
    # Ausgabe in $cors_allowed_origin,   eine Zeichenfolge
    #
    # Vollständige Zeichenfolgeübereinstimmung mit dem Wert im Origin-Header
    #
    # Übereinstimmung für den Wert aus $http_origin mithilfe von Regex in der linken Spalte suchen.
    # Wird eine Übereinstimmung gefunden, $cors_allowed_origin auf denselben Wert wie $http_origin auswerten
    # Ansonsten $cors_allowed_origin standardmäßig in eine leere Zeichenfolge auswerten, sodass Browser die CORS-Anforderung nicht zulassen
    ######
    map $http_origin $cors_allowed_origin {
        default '';
        ~^http(s)?://(www|www2|cdn|dev)\.example\.com$ $http_origin;
    }
    
    server {
        # Einige Konfigurationen für den Serverkontext

        # URI-Pfad zu Inhalt
        location /my-static-content {
        
            # Einige Konfigurationen für den Positionskontext
        
            # Einfache Anforderungen verarbeiten
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' $cors_allowed_origin;
            }

            # Preflight-Anforderungen verarbeiten
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '$cors_allowed_origin;
                add_header 'Access-Control-Allow-Methods' 'GET';
                add_header 'Access-Control-Allow-Headers' 'pragma';
                add_header 'Access-Control-Max-Age' 1728000;
                
                return 204;
            }

            # Weitere Konfigurationen für den Positionskontext

            try_files $uri =404;
        }

        # Weitere Konfigurationen für den Serverkontext
    }

    # Weitere Konfigurationen für den HTTP-Kontext
}
```
{: screen}

Im obigen Beispiel wird die Anweisung `map` verwendet, um eine Überfrachtung der Nginx-Anweisung `if` zu vermeiden. Wenn nun eine CORS-Anforderung an diesen Server gesendet wird und mit diesem URI-Pfad übereinstimmt, antwortet der Server mit dem Header `Access-Control-Allow-Origin`, der den Wert `http://www.example.com`, `https://cdn.example.com` oder `http://dev.example.com` usw. enthält, wenn der Inhalt von `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com` usw. angefordert wird.

## Vorgehensweise zur Konfiguration von CORS für CDN
{: #how-to-set-up-cors-for-cdn}

![CORS über CDN](/images/cors-through-cdn.png)
CDN ist weitgehend transparent für die CORS-Konfiguration des Ursprungs, daher ist keine bestimmte CDN-Konfiguration erforderlich. Wenn die CDN-Peripherie keine zwischengespeicherte Antwort für die erste Anforderung für einen Inhalt finden kann, leitet sie die Anforderung an den Ursprungshost weiter. Wenn der Ursprungshost für die Verarbeitung von CORS-Anforderungen konfiguriert ist und diese Anforderung den Header `Origin` hat, sollte der Host eine Antwort mit dem CORS-Header `Access-Control-Allow-Origin` und dem zugehörigen Wert an die Peripherie zurücksenden. Die gesamte Antwort einschließlich Header und Wert wird im CDN zwischengespeichert. Alle nachfolgenden Anforderungen für das Objekt in demselben URI-Pfad werden aus dem Cache bedient und umfassen den Wert des Headers `Access-Control-Allow-Origin`, der zu Anfang vom Ursprung empfangen wurde.

## Fehlerbehebung für CORS und CORS-Anforderungen
{: #troubleshooting-cors-and-cors-requests}

Wenn Ihr Ursprungsserver für CORS konfiguriert ist und der Header `Access-Control-Allow-Origin` nicht auf die Anforderung des Browsers zurückgegeben wird, ist es möglich, dass der im CDN zwischengespeicherte Antwortheader zu einer Anforderung gehörte, die nicht den Header "Origin" enthielt. Das CDN stellt Antwortheader vom Ursprungshost in den Cache. Die zwischengespeicherten Header basieren jedoch auf der Anforderung, die die Anforderung an den Ursprung ausgelöst hat. In diesem Fall enthalten die Antwortheader möglicherweise nicht die CORS-Header. Löschen Sie den Cache im CDN für diesen Pfad, indem Sie die CDN-Bereinigungsfunktion verwenden, und wiederholen Sie die Anforderung vom Client.

Der Antwortheader `Vary` von Ihrem Ursprungsserver kann auch zu unerwartetem Verhalten führen, wenn Inhalt über Ihr CDN abgerufen wird. Abgesehen von einer sehr spezifischen Ausnahme stellt das CDN den Inhalt (und den zugehörigen Antwortheader) von Ihrem Ursprungsserver nicht in den Cache, wenn der Server mit einem Header `Vary` antwortet. Zurzeit entfernt unser Service den Vary-Header des Ursprungs, damit das Objekt zwischengespeichert wird, wenn das Objekt einen der folgenden Dateitypen aufweist: `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. Weist das Objekt, das zwischengespeichert werden soll, einen anderen Dateityp auf, entfernen Sie den `Vary`-Header aus der Antwort des Ursprungsservers für dieses Objekt und wiederholen Sie den Vorgang, nachdem Sie die CDN-Bereinigungsfunktion verwendet haben.

## Weitere Informationen
{: #more-information}

* [https://www.w3.org/TR/cors/ ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://w3c.github.io/webappsec-cors-for-developers/)
