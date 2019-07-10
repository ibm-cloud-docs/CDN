---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Risoluzione dei problemi
{: #troubleshooting}

In questo documento, troverai diversi modi per risolvere i problemi della tua CDN {{site.data.keyword.cloud}}. Se devi contattare il supporto, assicurati di fornire il tuo numero di riferimento della CDN.

## Come faccio a sapere se la mia CDN funziona?
{: #how-do-I-know-my-cdn-is-working}

Esegui il seguente comando `curl` sostituendo `http://your.cdn.domain/uri` con il percorso file rispettivo sulla tua CDN.

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

Se l'output del comando `curl` è simile al seguente formato di esempio, la CDN sta funzionando come previsto:

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
{: screen}

## Ho ricevuto un errore 503. Perché?
{: #i-received-a-503-error-why}

Il motivo più comune osservato per l'errore 503 è dovuto a un problema con un certificato nella catena di certificati SSL.

Questo è l'errore che potresti vedere: `503 Service Unavailable`.  

Insieme all'errore 503, puoi anche vedere un messaggio simile al seguente: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (il numero di riferimento effettivo può variare). In questo caso, il numero di riferimento nella stringa di errore si traduce in un errore di handshake SSL.

Per correggere il problema, assicurati che il certificato SSL del server di origine soddisfi i seguenti criteri:
  * Il certificato **deve** essere emesso da un'autorità di certificazione ritenuta attendibile da Akamai. Puoi visualizzare l'elenco di certificati ritenuti attendibili da Akamai a [questo link ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)
  * **Deve** corrispondere all'*Host header* (Intestazione host) configurata sulla CDN
  * **Non** deve essere autofirmato
  * **Non** deve essere scaduto

Se hai verificato la catena di certificati della tua origine utilizzando i criteri precedenti e continui a riscontrare lo stesso errore, consulta la nostra pagina [Come ottenere aiuto e supporto](/docs/infrastructure/CDN?topic=CDN-gettinghelp) . Prendi nota della stringa di errore di riferimento e includila in qualsiasi comunicazione con noi.

## Il mio nome host non viene caricato sul browser quando IBM COS (Cloud Object Storage) è l'origine.
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

Quando la tua {{site.data.keyword.cloud_notm}} CDN è configurata per utilizzare COS come archiviazione oggetti, l'accesso diretto al sito web non funzionerà. Devi specificare il percorso della richiesta completo nella barra dell'indirizzo del browser (ad esempio, `www.example.com/index.html`). Questa modalità di funzionamento è dovuta alla limitazione del documento di indice in IBM COS.

## Non riesco a stabilire una connessione tramite un comando `curl` o il browser utilizzando il nome host con HTTPS.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

Se la tua CDN è stata creata utilizzando HTTPS con un certificato jolly, la connessione deve essere effettuata utilizzando il CNAME, ad esempio `https://www.exampleCname.cdnedge.bluemix.net`. Ciò include **tutte** le CDN create con HTTPS prima del 18 giugno 2018. Provare a stabilire una connessione utilizzando il nome host darà come risultato un errore.

## Qual è la modalità di funzionamento prevista quando viene caricato il CNAME o il nome host sul tuo browser per i protocolli supportati?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

Questa tabella mostra la modalità di funzionamento prevista per i protocolli supportati quando si carica il **nome host** o il **CNAME** dal tuo browser web.

<table>
<caption caption-side=“top”>Tabella delle modalità di funzionamento previste</caption>
<thead>
<tr>
<th rowspan=2 scope="col">URL del browser</th>
<th rowspan=2 scope="col">CDN solo con il protocollo HTTP</th>
<th colspan=2 scope="col">CDN solo con il protocollo HTTPS</th>
<th colspan=2 scope="col">CDN con entrambi i protocolli HTTP e HTTPS</th>
</tr>
<tr>
<th scope="col"> Jolly </th>
<th scope="col"> SAN condivisa </th>
<th scope="col"> Jolly </th>
<th scope="col"> SAN condivisa </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> Caricamento riuscito </td>
<td> 301 Moved permanently </td>
<td> Caricamento riuscito </td>
<td> 301 Moved permanently </td>
<td> Caricamento riuscito </td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> Accesso negato </td>
<td> Esegue il reindirizzamento alla pagina web di IBM </td>
<td> Caricamento riuscito </td>
<td> Esegue il reindirizzamento alla pagina web di IBM </td>
<td> Caricamento riuscito </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 Moved permanently </td>
		<td> Caricamento riuscito </td>
		<td> 301 Moved permanently </td>
		<td> Caricamento riuscito </td>
		<td> 301 Moved permanently </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> Esegue il reindirizzamento alla pagina web di IBM </td>
		<td> Caricamento riuscito </td>
		<td> 301 Moved permanently </td>
		<td> Caricamento riuscito </td>
		<td> Esegue il reindirizzamento alla pagina web di IBM </td>
</tr>
</tbody>
</table>

**Messaggi di errore comuni:**

Un messaggio `301 Moved permanently` molto probabilmente indica che stai provando a raggiungere una CDN con un protocollo `HTTPS` o `HTTP_AND_HTTPS` utilizzando il nome host. A causa di una limitazione con il certificato jolly HTTPS, **devi** utilizzare il CNAME per l'accesso alla tua CDN.

Con un protocollo **solo** HTTP, riceverai il messaggio `301 Moved permanently` se provi a raggiungere la tua CDN utilizzando il CNAME: In questo caso, puoi ottenere l'accesso alla tua CDN _solo_ utilizzando il nome host.

Il messaggio `Access denied` viene generato quando provi a raggiungere una CDN utilizzando un protocollo non corretto. Assicurati che stai utilizzando `http` per le CDN create con il protocollo HTTP oppure `https` per le CDN create con il protocollo HTTPS.

**Un possibile errore di reindirizzamento:**

La modalità di funzionamento che vede un URL reindirizzato alla pagina web di {{site.data.keyword.cloud_notm}} CDN si osserva con maggiore frequenza quando l'URL non è corretto per il protocollo. Se la tua CDN viene creata con un protocollo di HTTPS o HTTPS_AND_HTTPS, devi utilizzare il CNAME per l'accesso alla tua CDN. Ad esempio, `https://examplecname.cdnedge.bluemix.net` per le associazioni HTTPS o `http://examplecname.cdnedge.bluemix.net` oppure `https://examplecname.cdnedge.bluemix.net` per le associazioni HTTP_AND_HTTPS.

In questo caso, l'URL viene reindirizzato alla pagina web di {{site.data.keyword.cloud_notm}} CDN perché sia il protocollo che il dominio non sono corretti per il protocollo della CDN. Una CDN creata con HTTP come _solo_ protocollo può essere raggiunta _solo_ tramite il nome host. Ad esempio, `http://example.com`.
