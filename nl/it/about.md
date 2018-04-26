---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Informazioni sulla CDN (Content Delivery Network)

Una CDN (Content Delivery Network) è una raccolta di server edge distribuiti in varie parti del paese o del mondo. Il contenuto web viene fornito da un server edge che si trova nella zona geografica più vicina al cliente che richiede il contenuto. Questa tecnica consente ai tuoi utenti finali di ricevere il contenuto con meno ritardo e offre un'esperienza complessiva migliore ai tuoi clienti.

## Come funziona una CDN?

Una CDN raggiunge il suo scopo memorizzando in cache il contenuto web sui server edge in tutto il mondo. Quando un cliente richiede del contenuto web, la richiesta di contenuto viene indirizzata al server edge geograficamente più vicino a tale cliente. Riducendo la distanza che il contenuto deve percorrere, il CDN offre un rendimento ottimizzato, una latenza ridotta e prestazioni più elevate. Utilizzando CDN (Content Delivery Netwok) IBM Cloud con Akamai, i provider di contenuto possono ottenere una fornitura efficiente del contenuto richiesto da tutto il mondo con una configurazione minima.

Le funzioni chiave del servizio CDN (Content Delivery Network) IBM Cloud includono:
  * Supporto dell'origine server host
  * Supporto dell'origine Object Storage
  * Supporto per più origini con percorsi distinti
  * Associazioni di CDN basate sui percorsi
  * Eliminazione del contenuto presente nella cache
  * TTL (Time to Live)
  * Metriche con viste grafiche
  * Supporto HTTPS con il certificato wildcard
  * Supporto dell'intestazione host (Host Header)
  * Utilizzo di contenuto obsoleto
  * Funzione "Ignore Query Args in Cache Key" (Ignora argomenti query nella chiave della cache)
  * Compressione del contenuto
  * Ottimizzazione dei file di grandi dimensioni
  * Video on-demand

## Descrizioni delle funzioni

### Supporto dell'origine server host

La CDN (Content Delivery Network) IBM Cloud può essere configurata per fornire contenuto da un'origine server host fornendo il nome host, il protocollo, il numero porta e, facoltativamente, il percorso dell'origine da cui fornire il contenuto. Il percorso predefinito è '/\*'. Il protocollo può essere HTTP, HTTPS o entrambi. Akamai supporta solo degli specifici numeri porta. Consulta [Domande frequenti (FAQ)](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per i numeri porta o gli intervalli di porte supportati. Attualmente, HTTPS è supportato utilizzando il certificato wildcard, il che significa che il servizio sarà accessibile solo mediante la terminazione CNAME con `.cdnedge.bluemix.net`. Per ulteriori informazioni, consulta la descrizione della funzione [HTTPS con certificato wildcard](about.html#https-with-wildcard-certificate-).

### Supporto dell'origine Object Storage

La CDN IBM Cloud può essere configurata per fornire contenuto da un endpoint Object Storage fornendo _endpoint_, _nome bucket_, _protocollo_ e _porta_. Facoltativamente, è possibile specificare un elenco di estensioni file per consentire la memorizzazione in cache dei soli file che hanno tali estensioni. Tutti gli oggetti nel bucket devono essere impostati con un accesso in lettura anonimo o in lettura pubblico. Questa esercitazione su [come configurare l'Object Storage IBM Bluemix con CDN](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) fornisce informazioni più dettagliate.

### Supporto per più origini con percorsi distinti

In specifici casi, potresti voler fornire del contenuto da un server di origine differente. Ad esempio, potresti volere che specifiche foto o specifici video vengano forniti da server di origine differenti. La CDN IBM Cloud consente di configurare più server di origine con più percorsi. Questo offre flessibilità per quanto riguarda come e dove sono memorizzati i dati. Il percorso fornito per il server di origine deve essere univoco rispetto alla CDN. Il server di origine stesso non deve necessariamente essere univoco.

### Associazioni di CDN basate sui percorsi

Il servizio di CDN IBM Cloud può essere limitato a uno specifico percorso di directory sul server di origine fornendo il percorso quando si crea la CDN. A un utente finale è consentito l'accesso solo ai contenuti in tale percorso di directory. Ad esempio, se una CDN www.example.com viene creata con il percorso '/videos', è accessibile solo tramite `www.example.com/videos/`.

### Eliminazione del contenuto presente nella cache

La CDN IBM Cloud consente di rimuovere in modo rapido e facile, o "eliminare", il contenuto memorizzato nella cache dai server edge. Attualmente, l'eliminazione è consentita solo per un percorso file. Attualmente, l'implementazione di eliminazione con Akamai elimina il file in circa 5 secondi. L'IU consente anche di visualizzare la cronologia delle eliminazioni e di contrassegnare degli specifici percorsi di eliminazione come preferiti.

### TTL (Time to Live)

Il TTL (Time To Live) indica la quantità di tempo (in secondi) per cui il server edge memorizzerà in cache il contenuto per lo specifico percorso file o directory. Quando viene creata inizialmente una CDN, viene creato un TTL (Time To Live) globale per il percorso ('/\*') con un tempo predefinito di 3600 secondi. Il valore minimo per TTL è 30 secondi e il valore massimo è 2147483647 secondi. Per ciascuna voce, il percorso TTL deve essere univoco per la CDN. Se più percorsi corrispondono a uno specifico contenuto, per tale contenuto varrà la corrispondenza con il percorso configurato più di recente. Si considerino ad esempio due TTL: `/example/file`, creato per primo con un valore TTL (Time To Live) di 3000 secondi e `/example/*`, creato successivamente con un valore di 4000 secondi. Anche se `/example/file` è più specifico, `/example/*` era stato creato più di recente e, quindi, il TTL per `/example/file` sarà di 4000 secondi. Una volta create, le voci TTL possono essere modificate per il percorso e/o il tempo. Possono anche essere eliminate.

### Metriche con viste grafiche

Le metriche per una singola CDN sono fornite nella scheda Panoramica del portale del cliente per la specifica associazione di CDN. Dall'utilizzo della CDN sono calcolati due tipi di metriche: quelle che mostrano le metriche su un periodo di tempo come un grafico e quelle che vengono presentate come valori aggregati.

Per le metriche che visualizzano la modifica su un periodo di tempo come un grafico, puoi vedere tre grafici a linee e un grafico a torta. I tre grafici a linee sono: **Bandwidth** (Larghezza di banda), **Hits by Mapping** (Riscontri per associazione) e **Hits by Type** (Riscontri per tipo). Visualizzano l'attività su una base giornaliera nel corso dell'intervallo di tempo da te specificato. I grafici per **Bandwidth** (Larghezza di banda) e **** (Hits by Mapping) sono grafici a linea singola, mentre la suddivisione di **Hits by Type** (Riscontri per tipo) mostra una linea per ciascuno dei tipi di riscontro forniti. Il grafico a torta visualizza una suddivisione regionale della larghezza di banda per un'associazione di CDN su base percentuale.

Le metriche visualizzate per i valori aggregati includono l'**Bandwidth Usage** (Utilizzo della larghezza di banda) in GB, il **Total Hits** (Totale riscontri) al server edge della CDN e la **Hit Ratio** (Percentuale riscontri). Hit Ratio (Percentuale riscontri) indica la percentuale di volte che il contenuto viene distribuito dal server edge, non tramite la sua origine. La percentuale di riscontri attualmente viene presentata come una funzione di tutte le tue associazioni di CDN, non solo di quella visualizzata. 

Per impostazione predefinita, sia i numeri aggregati che i grafici visualizzano, per impostazione predefinita, le metriche per gli ultimi 30 giorni ma puoi modificare tale impostazione tramite il [Portale del cliente![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/). Entrambe le categorie possono visualizzare metriche per periodi di 7-, 15-, 30-, 60- o 90- giorni.

### Supporto HTTPS con il certificato wildcard

Il certificato wildcard è il modo più conveniente per consegnare in modo sicuro i contenuti web ai tuoi utenti finali. Questa funzione viene abilitata selezionando `HTTPS port` quando configuri la tua CDN o il percorso di origine. Dopo l'abilitazione dell'associazione di CDN per HTTPS, utilizzando il certificato wildcard il server edge contatta il server di origine tramite HTTPS. Se il server di origine è specificato come un nome host, il server edge utilizza per impostazione predefinita il nome host di origine come intestazioni SNI (Server Name Indication) per la negoziazione TLS (Transport Layer Security) con il server di origine. Tuttavia, se il server di origine è specificato come un indirizzo IP, il nome host della CDN sarà utilizzato come intestazione SNI. Il certificato di origine deve essere firmato da un'autorità di certificazione (CA) riconosciuta.

Quando si utilizza il certificato wildcard, un utente finale ha accesso al servizio **solo** utilizzando il CNAME. Ad esempio, se il dominio è `example.com` e il CNAME è `example.cdnedge.bluemix.net`, il tuo servizio CDN è accessibile **solo** tramite `https://example.cdnedge.bluemix.net`. Consulta le [Domande frequenti (FAQ)](faqs.html#what-should-be-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-cdn-protocols-) per capire le implicazioni dell'utilizzo di HTTPS con il certificato wildcard.

Come prassi ottimale del settore, Akamai ritiene attendibili solo i certificati root e non i certificati intermediari, poiché l'insieme di certificati intermediari attendibili cambia di frequente. Per motivi di sicurezza, Akamai include solo le autorità di certificazione root nel truststore Akamai. Pertanto, l'origine deve inviare l'intera catena di certificati, inclusi il certificato foglia e tutti i certificati intermedi (non includendo il certificato root) come parte dell'handshake SSL con il server edge. Puoi trovare i certificati attendibili Akamai [qui](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

### Supporto dell'intestazione host (Host Header)

Il server edge utilizza l'intestazione host (Host Header) quando comunica con l'host di origine. Questa funzione fornisce flessibilità alla modalità di configurazione del servizio web sull'host di origine. Se l'input dell'intestazione host non viene fornito, il servizio utilizza il nome host del server di origine come intestazione host HTTP predefinita se il server di origine viene specificato come nome host (piuttosto che come un indirizzo IP). Se l'intestazione host non viene fornita come input E il server di origine viene fornito come un indirizzo IP, il nome host CDN (detto anche nome dominio CDN) viene utilizzato come intestazione host HTTP predefinita.

### Respect Headers (Rispetta intestazioni)

L'opzione Respect Headers (Rispetta intestazioni) consente alla configurazione dell'intestazione HTTP nell'origine di sovrascrivere la configurazione della CDN. Questa funzione è attivata per impostazione predefinita, ma può essere disattivata.

### Utilizzo di contenuto obsoleto

Quando riceve una richiesta utente e il contenuto richiesto non è memorizzato nella cache, il server edge CDN si rivolge all'host di origine per recuperare il contenuto. Il contenuto viene quindi memorizzato in cache per la durata TTL (Time To Live) specificata per il contenuto. Se la richiesta utente viene ricevuta dopo la scadenza del TTL, il server edge si rivolge all'host di origine per recuperare il contenuto. Se il server di origine non è raggiungibile per qualche motivo (ad esempio l'host di origine è inattivo o si è verificato un problema di rete), il server edge fornisce il contenuto scaduto (obsoleto) alla richiesta. Questa funzione è supportata da Akamai e **non può** essere disattivata.

### Funzione "Ignore Query Args in Cache Key" (Ignora argomenti query nella chiave della cache)

I server edge Akamai memorizzano in cache il contenuto in un cosiddetto "Archivio della cache". Per utilizzare il contenuto dall'"Archivio della cache", i server edge utilizzano una "Chiave della cache". Di norma, una chiave della cache viene generata in base a una parte dell'URL dell'utente finale. In alcuni casi, l'URL contiene degli argomenti della funzione di query che sono diversi per i singoli utenti, ma il contenuto distribuito è lo stesso. Per impostazione predefinita, Akamai utilizza gli argomenti della funzione di query per generare la chiave della cache e, pertanto, per generare una chiave della cache univoca per ciascun utente. Questo metodo non è ottimale poiché fa in modo che il server edge contatti il server di origine per del contenuto che è già memorizzato nella cache utilizzando però una chiave della cache differente. La funzione **Ignore Query Args in Cache Key** (Ignora argomenti query nella chiave della cache) ti consente di specificare se gli argomenti di query devono essere ignorati quando si genera una chiave della cache. Questa funzione è valida per qualsiasi operazione di creazione (`create`) o di aggiornamento (`update`) di una configurazione di associazione di CDN, nonché per qualsiasi operazione di creazione (`create`) o aggiornamento (`update`) di un percorso di origine.

**Nota:** gli argomenti di query della chiave della cache possono essere configurati dalla scheda Impostazioni dopo la creazione di un'associazione di CDN. Per il percorso di origine, possono essere configurati durante un'operazione di creazione (`create`) o di aggiornamento (`update`) di un percorso di origine.

### Compressione del contenuto

La compressione del contenuto è abilitata nella CDN Akamai per impostazione predefinita per i seguenti tipi di contenuto:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Quando la compressione è gestita dal server edge, il contenuto deve essere almeno 10kB. In alcuni casi, la compressione viene gestita dal server di origine e, in tali casi, non c'è alcun limite alla dimensione dei file da comprimere. Se il contenuto è già compresso dal server di origine, non verrà compresso di nuovo. per abilitare la compressione del contenuto, l'intestazione della richiesta deve definire `Accept-Encoding: gzip`.

### Ottimizzazione dei file di grandi dimensioni

L'ottimizzazione dei file di grandi dimensioni consente alla rete CDN di ottimizzare la distribuzione di contenuto di dimensioni superiori ai 10MB. In questo modo, le prestazioni per i file di grandi dimensioni risultano migliorate e i tempi di latenza e download ridotti. Senza questa funzione, la CDN non può fornire file di dimensioni superiori a 1,8GB. Questa funzione consente anche di scaricare file più grandi di 1,8GB, fino a un massimo di 320GB.

Perché l'ottimizzazione dei file funzioni, le richieste di intervallo di byte **devono** essere abilitate sul server di origine. Akamai CDN utilizza una tecnica di memorizzazione in cache di oggetti parziali per questa ottimizzazione. Quando viene richiesto un file di grandi dimensioni, il server edge controlla se il file soddisfa i requisiti di dimensione. Ciò significa che i file più grandi di 10MB saranno richiesti dal server di origine in blocchi. Quando arriva al server edge, il blocco viene memorizzato in cache e fornito immediatamente all'utente. Il blocco successivo viene pre-recuperato in parallelo dal server edge, riducendo così la latenza. Questo processo continua finché non viene richiamato l'intero file oppure finché non viene terminata la connessione.  

Quando questa funzione è abilitata, fornire contenuto inferiore ai 10MB comporta un leggero costo in termini di prestazioni. Pertanto, questa funzione è consigliata solo per fornire file di grandi dimensioni. Un caso di utilizzo tipico consiste nel creare un nuovo percorso di origine nella configurazione della CDN e abilitare l'ottimizzazione di file di grandi dimensioni per tale percorso.

### Video on-demand

L'ottimizzazione delle prestazioni **Video on-demand** offre uno streaming di alta qualità su una gamma di tipi di rete. Avvalendosi della capacità della rete distribuita di distribuire il carico in modo dinamico, la CDN IBM Cloud con Akamai ti consente di eseguire rapidamente dei ridimensionamenti per vasti pubblici, che tu li abbia inclusi nei tuoi piani o meno.

**Video on-demand** è ottimizzato per la distribuzione di formati di streaming segmentati quali HLS, DASH, HDS e HSS. Lo streaming video live **non** è supportato, al momento. È possibile abilitare la funzione **Video On Demand** (Video on-demand) selezionando l'opzione dal menu a discesa in **Optimize for** (Ottimizza per) nella scheda Settings (Impostazioni) o mentre si crea un nuovo percorso di origine. Devi abilitare questa funzione solo quando ottimizzi la fornitura di file video.
