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

# Contenitore di input
{: #input-container}

Il Contenitore di input è una raccolta utilizzata sia dalla classe Mapping che da quella (Origin) Path. Fornisce un insieme congruente di attributi di input per entrambe le classi.

* `vendorName`: il nome di un provider {{site.data.keyword.cloud}} CDN valido.
* `oldPath`: utilizzato da updateOriginPath(). Questa proprietà memorizza il nome del percorso attuale o 'vecchio'.

I seguenti attributi sono comuni alle classi Mapping e (Origin) Path:
* `originType`: tipo dell'host di origine, attualmente 'HOST_SERVER' o 'OBJECT_STORAGE'.
* `origin`: l'indirizzo del server di origine (il nome host o l'indirizzo IPv4 del server di origine), l'endpoint da cui recuperare il contenuto oppure il nome del bucket dove è memorizzato il contenuto. Deve essere minore di 511 caratteri.
* `httpPort`: il numero della porta utilizzata per il protocollo HTTP. Akamai ha delle specifiche limitazioni sui numeri porta per le porte HTTP e HTTPS. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
* `httpsPort`: il numero della porta utilizzata per il protocollo HTTPS. Akamai ha delle specifiche limitazioni sui numeri porta per le porte HTTP e HTTPS. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
* `status`: lo stato dell'associazione o del percorso. Lo stato può essere CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED o ERROR.
* `path`: il percorso da cui verrà fornito il contenuto memorizzato in cache. Il percorso predefinito è /\* Quando viene utilizzato dalla API `updateOriginPath`, questo attributo fa riferimento al nuovo percorso da aggiungere.
* `performanceConfiguration`: specifiche per la configurazione delle prestazioni dell'associazione.
* `cacheKeyQueryRule`: per configurare la modalità di funzionamento della chiave della cache sono disponibili le seguenti opzioni:
  * `include-all`: include tutti gli argomenti della query
  * `ignore-all`: ignora tutti gli argomenti della query
  * `ignore: space separated query-args`: ignora questi specifici argomenti della query. Ad esempio, `ignore: query1 query2`
  * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, `include: query1 query2`
* `geoblockingRule`

I seguenti attributi sono specifici per la classe Mapping:

* `uniqueId`: un identificativo di 10 cifre generato dal sistema univoco per ogni associazione. Viene generato quando viene creata un'associazione.
* `domain`: il nome CDN primario. Viene indicato anche come nome host.
* `protocol`: il protocollo utilizzato per configurare i servizi. Può essere HTTP, HTTPS o una combinazione dei due, HTTP_AND_HTTPS.
* `cname`: il record di nome canonico che funge da alias del nome host. Può essere fornito dall'utente oppure generato dal sistema. Se viene fornito dall'utente, deve avere una lunghezza inferiore ai 64 caratteri alfanumerici e deve essere univoco. Se non viene fornito, ne viene generato uno quando viene creata l'associazione.
* `certificateType`: il tipo di certificato utilizzato da un'associazione. Può essere `WILDCARD_CERT` o `SHARED_SAN_CERT`. Il valore sarà 0 per le associazioni HTTP.
* `respectHeaders`: un valore booleano che, se impostato su 'true', fa in modo che le impostazioni TTL nell'origine sovrascrivano le impostazioni TTL di CDN.
* `header`: specifica le informazioni sull'intestazione host utilizzate dall'origine.

I seguenti attributi sono correlati a COS (Cloud Object Storage):  
* `bucketName`: il nome univoco del tuo bucket per l'Object Storage S3.  
* `fileExtension`: le estensioni file consentite.

Il seguente attributo è correlato alla configurazione di Hotlink Protection:
* `hotlinkProtection`: per ulteriori dettagli, consulta [Classe Hotlink Protection](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class).
