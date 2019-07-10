---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Contenitore associazione
{: #mapping-container}

La raccolta `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` contiene gli attributi utilizzati dalle nostre API Mapping. Ciascuna delle API Mapping restituisce una raccolta di questo tipo.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: il nome di un provider {{site.data.keyword.cloud}} CDN valido.
* `uniqueId`: un identificativo di 10 cifre generato dal sistema univoco per ogni associazione. Viene generato quando viene creata un'associazione.
* `domain`: il nome CDN primario. Indicato anche come `nome host`.
* `protocol`: il protocollo utilizzato per configurare i servizi. Può essere HTTP, HTTPS o una combinazione dei due, HTTP_AND_HTTPS.
* `originType`: tipo dell'host di origine, attualmente 'HOST_SERVER' o 'OBJECT_STORAGE'.
* `originHost`: l'indirizzo del server di origine (il nome host o l'indirizzo IPv4 del server di origine), che è l'endpoint da cui recuperare il contenuto oppure il nome del bucket dove è memorizzato il contenuto. Deve avere una lunghezza inferiore ai 511.
* `httpPort`: il numero della porta utilizzata per il protocollo HTTP. Akamai ha delle specifiche limitazioni sui numeri porta per le porte HTTP e HTTPS. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
* `httpsPort`: il numero della porta utilizzata per il protocollo HTTPS. Akamai ha delle specifiche limitazioni sui numeri porta per le porte HTTP e HTTPS. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
* `certificateType`: il tipo di certificato utilizzato da un'associazione. Può essere `WILDCARD_CERT` o `SHARED_SAN_CERT`. Il valore sarà 0 per le associazioni HTTP.
* `cname`: il record di nome canonico che funge da alias del nome host. Può essere fornito dall'utente oppure generato dal sistema. Se viene fornito dall'utente, deve avere una lunghezza inferiore ai 64 caratteri alfanumerici e deve essere univoco. Se non viene fornito, ne viene generato uno quando viene creata l'associazione.
* `respectHeaders`: un valore booleano che, se impostato su 'true', fa in modo che le impostazioni TTL nell'origine sovrascrivano le impostazioni TTL di CDN.
* `header`: specifica le informazioni sull'intestazione host utilizzate dal server di origine.
* `status`: lo stato dell'associazione. Lo stato può essere CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED o ERROR.
* `bucketName`: il nome bucket per l'Object Storage S3.
* `fileExtension`: le estensioni file di cui è consentita la memorizzazione in cache.
* `percorso`: Il percorso al tuo Object Storage S3. Si trova di solito nella sezione API o URL dell'archiviazione oggetti del tuo servizio.
* `cacheKeyQueryRule`: per configurare la modalità di funzionamento della chiave della cache sono disponibili le seguenti opzioni:
  * `include-all`: include tutti gli argomenti della query **Valore predefinito**
  * `ignore-all`: ignora tutti gli argomenti della query
  * `ignore: space separated query-args`: ignora questi specifici argomenti della query. Ad esempio, "ignore: query1 query2"
  * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, "include: query1 query2"

Nota in particolare l'`uniqueId`, che viene generato quando un'associazione viene creata e utilizzata come un parametro per le chiamate API successive.

Ad esempio, questa chiamata a `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

restituirà un oggetto simile a questo:

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
            [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
            [cname] => api-testing.cdnedge.bluemix.net
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
            [performanceConfiguration] => General web delivery
            [protocol] => HTTP
            [respectHeaders] => 1
            [serveStale] => 1
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

Le chiamate a `stopDomainMapping` e `startDomainMapping` restituirà lo stesso oggetto di associazione, con lo stato `status` che rappresenta la differenza.

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
