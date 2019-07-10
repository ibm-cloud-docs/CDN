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

# CORS e richieste CORS tramite la tua CDN
{: #cors-and-cors-requests-through-your-cdn}

CORS (Cross Origin Resource Sharing) è un meccanismo utilizzato dai browser, principalmente per convalidare le autorizzazioni per l'accesso al contenuto da un'origine differente.

## Cos'è CORS?
{: #what-is-cors}

Quando carica una pagina web, un browser implementa la **Same-origin Policy**; questo significa che consente il recupero di contenuto solo dalla stessa origine della pagina web. Tuttavia, in alcuni casi, una pagina web potrebbe aver bisogno di accedere ad asset da più origini che ritengono attendibile tale sito web. È in questo contesto che entra in gioco CORS. 

Questo meccanismo di sicurezza esiste solo se un'applicazione ha o è un client HTTP e se tale applicazione implementa CORS. Quasi tutti i browser moderni, come Chrome, Firefox e Safari,  implementano CORS:

Per chiarire, **un'origine rispetto a CORS non deve necessariamente essere uguale a un'origine CDN**. Un'origine in CORS è definita da uno schema, un dominio e qualsiasi possibile numero porta URI. Ad esempio `https://www.example.com:1443` è un'origine diversa rispetto a `http://www.example.com`. Pertanto, una CDN può anche essere considerata un'origine CORS, dalla prospettiva di un browser.

## Come funziona CORS

CORS può gestire due tipi di richieste: _richieste semplici_ e _richieste con verifica preliminare_, che sono più complesse.

### Richieste semplici
{: #simple-requessts}

**Prima richiesta (accesso risorsa):**

![cors-simple](/images/cors-simple.png)

[Le richieste semplici ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/cors/#simple-cross-origin-request) tramite CORS sono richieste `GET` o `POST` dalla pagina web di un'origine che sta provando a ottenere l'accesso all'URL di un'altra origine per le risorse.

Quando viene fatta una richiesta di questo tipo, il browser imposta automaticamente le intestazioni di richiesta CORS. Innanzitutto, imposta l'origine della pagina web che sta facendo la richiesta nell'intestazione HTTP `Origin` automaticamente. La richiesta CORS può anche contenere una serie di [alcune intestazioni HTTP standard ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/cors/#simple-header), mantenendo al tempo stesso il suo stato come una semplice richiesta CORS, dalla prospettiva del browser.

Il server che riceve la richiesta CORS elaborerà la richiesta e potrebbe (o meno) inviare una serie di intestazioni di risposta CORS al browser, con il contenuto richiesto. Queste intestazioni di risposta CORS contengono valori che specificano se all'attuale pagina web è consentito l'accesso a tali risorse, se le intestazioni inviate sono accettabili per quella richiesta e così via.

Se non riesce a vedere la sua richiesta CORS soddisfatta dalle intestazioni di risposta CORS, il browser impedisce automatica l'accesso al contenuto e il suo caricamento. Altrimenti, vede che l'origine CORS sta concedendo l'autorizzazione a utilizzare la risorsa e consente l'accesso al contenuto richiesto e il suo caricamento.

### Richieste con verifica preliminare
{: #preflighted-requests}

**Prima richiesta (verifica preliminare):**

![cors-preflight](/images/cors-preflight.png)

Seconda richiesta (accesso risorsa):

![cors-after-preflight](/images/cors-after-preflight.png)

Per comunicazioni CORS più complesse tra il browser e un'origine CORS diversa dalla pagina web richiedente, sarà necessaria una [richiesta con verifica preliminare ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) prima dell'effettivo accesso alla risorsa. Alcune situazioni potrebbero richiedere delle richieste CORS con verifica preliminare, come ad esempio dei metodi HTTP che non sono metodi `GET` o `POST` o che utilizzano intestazioni HTTP non standard con la richiesta, anche se è una richiesta `GET` o `POST`, e così via.

Se è necessaria una richiesta con verifica preliminare, ecco qual è la successione degli eventi:

* Il browser invia una richiesta utilizzando il metodo HTTP OPTIONS al server con tutte le intestazioni di richiesta CORS previste. 
* Il server elabora tali intestazioni di richiesta CORS e può rispondere con delle intestazioni di risposta CORS che non contengono effettivi dati di contenuto. 
* Il browser controlla queste intestazioni di risposta CORS per assicurarsi che la richiesta CORS sia consentita. 
* Se il browser appura che la richiesta di risorsa prevista deve essere consentita dal browser, effettua una seconda richiesta al browser con il metodo HTTP previsto, sia `GET`, `POST`, `PUT`, ecc., con le stesse intestazioni di richiesta CORS.

Successivamente le comunicazioni tra il browser e l'origine CORS (diversa da quella della pagina web) procederanno come se si trattasse di una semplice richiesta CORS. Analogamente a una semplice richiesta CORS, il contenuto e le risorse sono accessibili e possono essere caricati se è consentita anche questa seconda richiesta CORS.

## Come configurare CORS presso la tua origine
{: #how-to-set-up-cors-at-your-origin}

Come mostrato nei diagrammi precedenti, CORS viene limitato dal client HTTP richiedente. Tuttavia, gli effetti dipendono dall'origine richiesta. Perché il tuo contenuto sia pronto per le richieste CORS, la tua origine deve essere configurata correttamente, per rispondere con le intestazioni di risposta CORS corrette e le autorizzazioni di accesso corrette.

Il seguente esempio mostra una configurazione CORS di base per un server Nginx:

```nginx
http {
    # some http context configs

    server {
        # some server context configs

        # URI path to some content
        location /my-static-content {
        
            # some location context configs
        
            # Handle simple requests
            #
            # Consider only "HTTP GET" requests (content fetching)
            if ($request_method = 'GET') {
                
                # Allows the browser to access data from this server during CORS,
                # only if the request comes from a webpage from the following origin
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
            }

            # Handle preflight requests
            if ($request_method = 'OPTIONS') {
                
                # Allows the browser to access data from this server during CORS,
                # only if the request comes from a webpage from the following origin
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
                
                # Allows only GET requests
                add_header 'Access-Control-Allow-Methods' 'GET';
                
                # Allows the following headers in the browser's request headers
                # that may have been added by anything other than what the browser had added automatically
                add_header 'Access-Control-Allow-Headers' 'pragma';
                
                # Specifies to the browser how long it should cache this preflight response
                add_header 'Access-Control-Max-Age' 1728000;
                
                # HTTP 204 response code means success,
                # but also that it should expect no content from this (preflight) response
                return 204;
            }

            # more location context configs
            
            # If it is not a complex CORS situation requiring a preflight response,
            # then finally attempt to serve the file whose path falls under this location block's URI path match
            #
            # If no such file is found, then present an HTTP 404 code (not found)
            try_files $uri =404;
        }

        # more server context configs
    }

    # more http context configs
}
```
{: screen}

Generalmente, il browser dovrebbe caricare il contenuto liberamente quando rileva un `Access-Control-Allow-Origin: *` nelle intestazioni di risposta CORS del server in base alla [specifica w3 relativa a tale valore jolly![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/cors/#security). Tuttavia, non tutti i browser supportano `Access-Control-Allow-Origin: *`.

Se il server deve supportare l'accesso da più pagine web, ciascuna fornita da un'origine differente, un singolo valore di origine per `Access-Control-Allow-Origin` deve essere generato dinamicamente per ogni singola richiesta. Il seguente è un esempio di base di un tale caso d'uso per un server Nginx:

```nginx
http {
    # some http context config

    ######
    # Input from $http_origin,          the value in Origin HTTP header
    # Output to $cors_allowed_origin,   a string
    #
    # Full string match on the Origin header value
    #
    # Attempt to find a match for value from $http_origin using regex on left column.
    # If a match is found, then evaluate $cors_allowed_origin to the same value as $http_origin
    # Else, default evaluate $cors_allowed_origin to an empty string, so browsers will disallow the CORS request
    ######
    map $http_origin $cors_allowed_origin {
        default '';
        ~^http(s)?://(www|www2|cdn|dev)\.example\.com$ $http_origin;
    }
    
    server {
        # some server context configs

        # URI path to some content
        location /my-static-content {
        
            # some location context configs
        
            # Handle simple requests
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' $cors_allowed_origin;
            }

            # Handle preflight requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '$cors_allowed_origin;
                add_header 'Access-Control-Allow-Methods' 'GET';
                add_header 'Access-Control-Allow-Headers' 'pragma';
                add_header 'Access-Control-Max-Age' 1728000;
                
                return 204;
            }

            # more location context configs

            try_files $uri =404;
        }

        # more server context configs
    }

    # more http context configs
}
```
{: screen}

L'esempio precedente utilizza la direttiva `map` per evitare un utilizzo eccessivo dell'istruzione di Nginx `if`. Ora, quando viene fatta una richiesta CORS a questo server e corrisponde a quel percorso URI, il server risponde con l'intestazione `Access-Control-Allow-Origin` che contiene il valore `http://www.example.com`, `https://cdn.example.com` o `http://dev.example.com`, ecc. quando il contenuto viene richiesto da `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com` e così via.

## Come configurare CORS per CDN
{: #how-to-set-up-cors-for-cdn}

![cors-through-cdn](/images/cors-through-cdn.png)
CDN è ampiamente trasparente alla configurazione CORS dell'origine, quindi non richiede una specifica configurazione CDN. Se non trova una risposta memorizzata in cache per la prima richiesta per del contenuto, l'edge CDN inoltra la richiesta all'host di origine. Se l'host di origine è configurato per gestire richieste CORS e tale richiesta ha l'intestazione `Origin`, dovrebbe rispondere a sua volta all'edge con un'intestazione CORS di `Access-Control-Allow-Origin` e il valore associato. La risposta complessiva, compresi tali intestazione e valore, sarà memorizzata in cache nella CDN. Eventuali richieste successive per l'oggetto allo stesso percorso URI vengono fornite dalla cache e includono il valore dell'intestazione `Access-Control-Allow-Origin` che era stato originariamente ricevuto dall'origine.

## Risoluzione dei problemi di CORS e delle richieste CORS
{: #troubleshooting-cors-and-cors-requests}

Se il tuo server di origine è configurato per CORS e non vedi l'intestazione `Access-Control-Allow-Origin` restituita alla richiesta del browser, è possibile che l'intestazione della risposta memorizzata in cache nella CDN fosse una richiesta che non aveva l'intestazione Origin nella richiesta. La CDN memorizza in cache le intestazioni di risposta dall'host di origine. Tuttavia, le intestazioni memorizzate nella cache sono basate sulla richiesta che ha attivato la richiesta all'origine. In tal caso, le intestazioni di risposta potrebbero non includere le intestazioni CORS. Elimina i dati della cache nella CDN per tale percorso utilizzando la funzionalità di eliminazione dei dati (Purge) della CDN e ritenta la richiesta dal client.

L'intestazione di risposta `Vary` dal server di origine può anche causare una modalità di funzionamento imprevista, in fase di recupero del contenuto tramite la tua CDN. Fatta salva una singola e molto specifica eccezione, la CDN non memorizza in cache il contenuto (e la relativa intestazione di risposta associata) dal tuo server di origine, se il server risponde con un'intestazione `Vary`. Attualmente, il nostro servizio rimuove l'intestazione Vary dall'origine per rendere l'oggetto memorizzabile in cache, se l'oggetto è uno dei seguenti tipi di file: `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. Se l'oggetto da memorizzare in cache non è uno di questi tipi di file, rimuovi l'intestazione `Vary` dalla risposta del server di origine per tale oggetto e prova di nuovo dopo aver utilizzato la funzionalità di eliminazione dei dati (Purge) della CDN.

## Ulteriori informazioni
{: #more-information}

* [https://www.w3.org/TR/cors/ ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://w3c.github.io/webappsec-cors-for-developers/)
