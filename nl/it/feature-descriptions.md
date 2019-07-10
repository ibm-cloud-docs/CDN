---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

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
{:DomainName: data-hd-keyref="DomainName"}

# Descrizioni delle funzioni
{: #feature-descriptions}

Questa pagina evidenzia molte delle funzioni incluse con {{site.data.keyword.cloud}} CDN con tecnologia Akamai.

## Supporto dell'origine server host
{: #host-server-origin-support}

IBM Cloud CDN (Content Delivery Network) può essere configurata per fornire contenuto da un'origine server host fornendo il nome host, il protocollo, il numero porta e, facoltativamente, il percorso dell'origine da cui fornire il contenuto. Il percorso predefinito è `/*`. Il protocollo può essere HTTP, HTTPS o entrambi. Akamai supporta solo degli specifici numeri porta. Consulta [Domande frequenti (FAQ)](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per i numeri porta o gli intervalli di porte supportati.

## Supporto dell'origine Object Storage
{: #object-storage-file-support}

IBM Cloud CDN può essere configurata per offrire contenuto da un endpoint Object Storage fornendo l'endpoint, il nome bucket, il protocollo e la porta. Facoltativamente, è possibile specificare un elenco di estensioni file per consentire la memorizzazione in cache dei soli file che hanno tali estensioni. Tutti gli oggetti nel bucket devono essere impostati con un accesso in lettura anonimo o in lettura pubblico.

Questa esercitazione su [come configurare IBM Cloud Object Storage con CDN](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) fornisce informazioni più dettagliate.

## Supporto per più origini con percorsi distinti
{: #support-for-multiple-origins-with-distinct-paths}

In specifici casi, potresti voler fornire del contenuto da un server di origine differente. Ad esempio, potresti volere che specifiche foto o specifici video vengano forniti da server di origine differenti. IBM Cloud CDN consente di configurare più server di origine con più percorsi. Questo offre flessibilità per quanto riguarda come e dove sono memorizzati i dati. Il percorso fornito per il server di origine deve essere univoco rispetto alla CDN. Il server di origine stesso non deve necessariamente essere univoco.

## Associazioni di CDN basate sui percorsi
{: #path-based-cdn-mappings}

Il servizio di IBM Cloud CDN può essere limitato a uno specifico percorso di directory sul server di origine fornendo il percorso quando si crea la CDN. A un utente finale è consentito l'accesso solo ai contenuti in tale percorso di directory. Ad esempio, se viene creata una CDN `www.example.com` con il percorso `/videos`, è accessibile **solo** tramite `www.example.com/videos/*`.

## Eliminazione del contenuto presente nella cache
{: #purge-cached-content}

IBM Cloud CDN consente di rimuovere in modo rapido e facile, o "eliminare", il contenuto memorizzato nella cache dai server edge. Attualmente, l'eliminazione è consentita solo per un percorso file. Attualmente, l'implementazione di eliminazione con Akamai elimina il file in circa 5 secondi. L'IU consente anche di visualizzare la cronologia delle eliminazioni e di contrassegnare degli specifici percorsi di eliminazione come preferiti.

## TTL (Time to Live)
{: #time-to-live-ttl}

Il TTL (Time To Live) indica la quantità di tempo (in secondi) per cui il server edge conserverà il contenuto memorizzato nella cache per lo specifico percorso file o directory. Quando viene creata inizialmente una CDN, viene creato un TTL (Time To Live) globale per il percorso `/\*` con un tempo predefinito di 3600 secondi. Il valore minimo per TTL è 0 secondi e il valore massimo è 2147483647 secondi. Per ciascuna voce, il percorso TTL deve essere univoco per la CDN. Se più percorsi corrispondono a uno specifico contenuto, per tale contenuto varrà la corrispondenza con il percorso configurato più di recente. Si considerino ad esempio due TTL: `/example/file`, creato per primo con un valore TTL (Time To Live) di 3000 secondi e `/example/*`, creato successivamente con un valore di 4000 secondi. Anche se `/example/file` è più specifico, `/example/*` era stato creato più di recente e, quindi, il TTL per `/example/file` sarà di 4000 secondi. Una volta create, le voci TTL possono essere modificate per il percorso e/o il tempo. Possono anche essere eliminate.

## Metriche con viste grafiche
{: #metrics-with-graphical-views}

Le metriche per una singola CDN sono fornite nella scheda Panoramica del portale del cliente per la specifica associazione di CDN. Dall'utilizzo della CDN sono calcolati due tipi di metriche: quelle che mostrano le metriche su un periodo di tempo come un grafico e quelle che vengono presentate come valori aggregati.

Per le metriche che visualizzano la modifica su un periodo di tempo come un grafico, puoi vedere tre grafici a linee e un grafico a torta. I tre grafici a linee sono: **Bandwidth** (Larghezza di banda), **Hits by Mapping** (Riscontri per associazione) e **Hits by Type** (Riscontri per tipo). Visualizzano l'attività su una base giornaliera nel corso dell'intervallo di tempo da te specificato. I grafici per **Bandwidth** (Larghezza di banda) e **** (Hits by Mapping) sono grafici a linea singola, mentre la suddivisione di **Hits by Type** (Riscontri per tipo) mostra una linea per ciascuno dei tipi di riscontro forniti. Il grafico a torta visualizza una suddivisione regionale della larghezza di banda per un'associazione di CDN su base percentuale.

Le metriche visualizzate per i valori aggregati includono l'**Bandwidth Usage** (Utilizzo della larghezza di banda) in GB, il **Total Hits** (Totale riscontri) al server edge della CDN e la **Hit Ratio** (Percentuale riscontri). Hit Ratio (Percentuale riscontri) indica la percentuale di volte che il contenuto viene distribuito dal server edge, _non_ tramite la sua origine. La percentuale di riscontri attualmente viene presentata come una funzione di tutte le tue associazioni di CDN, non solo di quella visualizzata.

Per impostazione predefinita, sia i numeri aggregati che i grafici visualizzano, per impostazione predefinita, le metriche per gli ultimi 30 giorni ma puoi modificare tale impostazione tramite il [Portale del cliente![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/). Entrambe le categorie possono visualizzare metriche per periodi di 7-, 15-, 30-, 60- o 90- giorni.

## Supporto dell'intestazione host (Host Header)
{: #host-header-support}

Il server edge utilizza l'intestazione host (**Host Header**) quando comunica con l'host di origine. Questa funzione fornisce flessibilità alla modalità di configurazione del servizio web sull'host di origine. Specificamente, abilita un caso d'uso in cui un cliente ha più server web configurati sullo stesso host di origine. Se l'input dell'intestazione host non viene fornito, il servizio utilizza il nome host del server di origine come intestazione host HTTP predefinita se il server di origine viene specificato come nome host (piuttosto che come un indirizzo IP). Se l'intestazione host non viene fornita come input E il server di origine viene fornito come un indirizzo IP, il nome host CDN (detto anche nome dominio CDN) viene utilizzato come intestazione host HTTP predefinita.

## Supporto del protocollo HTTPS
{: #https-protocol-support}

La CDN può essere configurata per utilizzare il protocollo HTTPS per fornire il contenuto in modo sicuro agli utenti finali. Questa configurazione richiede che un certificato SSL debba essere impostato come parte della configurazione CDN. Per HTTPS sono disponibili due tipi di opzioni di certificato SSL: [Certificato jolly](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) e [Certificato SAN (Subject Alternative Name) DV (Domain Validation)](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport). All'interno di questa documentazione, questo tipo sarà indicato anche come _Certificato SAN_.

Il tipo di certificato SSL da utilizzare è una considerazione importante per la rete CDN HTTPS. L'impostazione della configurazione del certificato jolly è veloce, ma come lato negativo vi è il fatto che la CDN è accessibile solo tramite un CNAME. Il completamento del processo del certificato SAN richiede da 4 a 8 ore, ma offre la possibilità di utilizzare la CDN con il dominio CDN (ovvero, il nome host). Il certificato SAN richiede anche un ulteriore passo di [**Convalida del controllo del dominio**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) durante la configurazione. Nessun costo è associato all'utilizzo di uno di questi certificati. Fai riferimento al [documento relativo alla risoluzione dei problemi](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-) per comprendere l'implicazione della selezione di un determinato tipo di certificato.

L'host di origine deve inoltre avere il suo proprio certificato SSL per il nome host CDN e deve essere firmato da un'autorità di certificazione (CA) riconosciuta.

Come prassi ottimale del settore, Akamai ritiene attendibili solo i certificati root e non i certificati intermediari, poiché l'insieme di certificati intermediari attendibili cambia di frequente. Puoi trovare i certificati attendibili Akamai [sul Web in questa ubicazione](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Respect Headers
{: #respect-headers}

L'opzione **Respect Headers** (Rispetta intestazioni) consente alla configurazione dell'intestazione HTTP nell'origine di sovrascrivere la configurazione della CDN. Questa funzione è attivata per impostazione predefinita, ma può essere disattivata.

## Gestione di contenuto obsoleto
{: #serve-stale-content}

Quando riceve una richiesta utente e il contenuto richiesto non è memorizzato nella cache, il server edge CDN si rivolge all'host di origine per recuperare il contenuto. Il contenuto viene quindi memorizzato in cache per la durata TTL (Time To Live) specificata per il contenuto. Se la richiesta utente viene ricevuta dopo la scadenza del TTL, il server edge si rivolge all'host di origine per recuperare il contenuto. Se il server di origine non è raggiungibile per qualche motivo (ad esempio l'host di origine è inattivo o si è verificato un problema di rete), il server edge fornisce il contenuto scaduto (obsoleto) alla richiesta. Questa funzione è supportata da Akamai e **non può** essere disattivata.

## Ottimizzazione delle chiavi della cache
{: #cache-key-optimization}

I server edge Akamai memorizzano in cache il contenuto in un **Archivio della cache**. Per utilizzare il contenuto dall'**Archivio della cache**, i server edge utilizzano una **Chiave della cache**. Di norma, una **Chiave della cache** viene generata in base a una parte dell'URL di un utente finale. In alcuni casi, l'URL contiene degli argomenti della funzione di query che sono diversi per i singoli utenti, ma il contenuto distribuito è lo stesso. Per impostazione predefinita, Akamai utilizza gli argomenti della funzione di query per generare la chiave della cache e, pertanto, per generare una chiave della cache univoca per ciascun utente. Questo metodo non è ottimale poiché fa in modo che il server edge contatti il server di origine per del contenuto che è già memorizzato nella cache utilizzando però una chiave della cache differente. La funzione **Cache Key Optimization** ti consente di specificare quali argomenti della query includere o ignorare in fase di generazione di una chiave della cache. Questa funzione è valida per qualsiasi operazione di creazione (`create`) o di aggiornamento (`update`) di una configurazione di associazione di CDN, nonché per qualsiasi operazione di creazione (`create`) o aggiornamento (`update`) di un percorso di origine.

Il valore **Cache Key Optimization** può essere configurato dalla scheda **Settings** dopo la creazione di un'associazione CDN. Per il percorso di origine, questo valore può essere configurato durante le operazioni `create` o `update` di un percorso di origine.
{: note}

## Compressione del contenuto
{: #content-compression}

La **Compressione del contenuto** è abilitata nella CDN Akamai per impostazione predefinita per i seguenti tipi di contenuto:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Quando la compressione è gestita dal server edge, il contenuto deve essere almeno 10kB.  In alcuni casi, la compressione viene gestita dal server di origine e, in tali casi, non c'è alcun limite alla dimensione dei file da comprimere. Se il contenuto è già compresso dal server di origine, non verrà compresso di nuovo. Per abilitare la compressione del contenuto, l'intestazione della richiesta deve definire `Accept-Encoding: gzip`.

## Ottimizzazione dei file di grandi dimensioni
{: #large-file-optimization}

L'ottimizzazione dei file di grandi dimensioni consente alla rete CDN di ottimizzare la distribuzione di contenuto di dimensioni superiori ai 10MB. In questo modo, le prestazioni per i file di grandi dimensioni risultano migliorate e i tempi di latenza e download ridotti. Senza questa funzione, la CDN non può fornire file di dimensioni superiori a 1,8GB. Questa funzione consente anche di scaricare file più grandi di 1,8GB, fino a un massimo di 320GB.

Perché l'ottimizzazione dei file di grandi dimensioni funzioni, le richieste di intervallo di byte **devono** essere abilitate sul server di origine. Akamai CDN utilizza una tecnica denominata _memorizzazione in cache di oggetti parziali_ per questa ottimizzazione. Quando viene richiesto un file di grandi dimensioni, il server edge controlla se il file soddisfa i requisiti di dimensione. Ciò significa che i file più grandi di 10MB saranno richiesti dal server di origine in blocchi. Quando arriva al server edge, il blocco viene memorizzato in cache e fornito immediatamente all'utente. Il blocco successivo viene pre-recuperato in parallelo dal server edge, riducendo così la latenza. Questo processo continua finché non viene richiamato l'intero file oppure finché non viene terminata la connessione.

Quando questa funzione è abilitata, fornire contenuto inferiore ai 10MB comporta un leggero costo in termini di prestazioni. Pertanto, questa funzione è consigliata solo per fornire file di grandi dimensioni. Un caso di utilizzo tipico consiste nel creare un nuovo percorso di origine nella configurazione della CDN e abilitare l'ottimizzazione di file di grandi dimensioni per tale percorso.

## Video on-demand
{: #video-on-demand}

L'ottimizzazione delle prestazioni **Video on-demand** offre uno streaming di alta qualità su una gamma di tipi di rete. Avvalendosi delle impostazioni di controllo della cache preconfigurate e della capacità della rete distribuita di distribuire il carico in modo dinamico, la IBM Cloud CDN con Akamai ti consente di eseguire rapidamente dei ridimensionamenti per vasti pubblici, che tu li abbia inclusi nei tuoi piani o meno.

**Video on-demand** è ottimizzato per la distribuzione di formati di streaming segmentati quali HLS, DASH, HDS e HSS. Lo streaming video live **non** è supportato, al momento. È possibile abilitare la funzione **Video On Demand** (Video on-demand) selezionando l'opzione dal menu a discesa in **Optimize for** (Ottimizza per) nella scheda Settings (Impostazioni) o mentre si crea un nuovo percorso di origine. Devi abilitare questa funzione solo quando ottimizzi la fornitura di file video.

## Controllo dell'accesso geografico
{: #geographical-access-control}

Il Controllo dell'accesso geografico è una modalità di funzionamento basata sulle regole che ti consente di impostare il parametro `access-type` per un gruppo di utenti, sulla base della loro ubicazione geografica. Sono disponibili due tipi di modalità di funzionamento: **Allow** (Consenti) e **Deny** (Nega).

L'access-type (tipo di accesso) `Allow` (Consenti) ti permette di consentire specificamente del traffico a regioni selezionate, in base al tipo di regione. Consentire il traffico per specifiche regioni blocca implicitamente il traffico per tutte le altre. Potresti, ad esempio, scegliere di consentire (`Allow`) il traffico a continenti selezionati, come l'Europa e l'Oceania), il che blocca l'accesso per tutti gli altri continenti.

La modalità di funzionamento `Deny` (Nega), invece, blocca l'accesso al tuo servizio per il gruppo specificato, il che consente l'accesso per tutte le altre regioni non specificate. Ad esempio, se imposti l'access-type (tipo di accesso) del Controllo dell'accesso geografico su `Deny` (Nega) per i continenti Europa e Oceania, gli utenti in questi continenti **non** potranno utilizzare il tuo servizio mentre gli utenti in tutti gli altri continenti vi avranno accesso.

Questa funzione è accessibile dalla pagina **Settings** (Impostazioni) della configurazione della tua CDN.

## Hotlink Protection
{: #hotlink-protection}

Hotlink Protection è una modalità di funzionamento basata sulle regole che ti consente di controllare se ad alcuni siti web è consentito accedere al tuo contenuto dalla tua CDN.  Il browser di norma include un'intestazione `Referer` quando una richiesta HTTP viene effettuata da un link su una pagina web e quando tale link punta a un asset remoto. Il link utilizzato da un sito web per accedere a un asset da un altro sito web è chiamato _hotlink_. Sono disponibili due tipi di modalità di funzionamento: **ALLOW** (CONSENTI) e **DENY** (RIFIUTA).

Se il tuo `protectionType` è impostato su `ALLOW`:
* Se il valore dell'intestazione `Referer` in una richiesta inviata alla tua CDN corrisponde a uno dei tuoi `refererValues` specificati, la tua CDN **fornirà** il contenuto richiesto.
* Altrimenti, la tua CDN **non** fornirà il contenuto.

Se il tuo `protectionType` è impostato su `DENY`:
* Se il valore dell'intestazione `Referer` in una richiesta inviata alla tua CDN corrisponde a uno dei tuoi `refererValues` specificati, la tua CDN **non fornirà** il contenuto richiesto.
* Altrimenti, la tua CDN fornirà il contenuto.
