---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

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


# Guida di riferimento alle API CDN
{: #cdn-api-reference}

L'API (Application Programming Interface) dell'infrastruttura {{site.data.keyword.cloud}} (comunemente conosciuta come SLAPI), fornita da {{site.data.keyword.cloud}}, è l'interfaccia di sviluppo che fornisce agli sviluppatori e agli amministratori di sistema l'interazione diretta con il sistema di backend dell'infrastruttura {{site.data.keyword.cloud_notm}}.

La SLAPI implementa molte delle funzioni nel portale del cliente: se è possibile un'interazione nel portale del cliente, può anche essere eseguita nella SLAPI. Poiché puoi interagire con tutte le parti dell'ambiente dell'infrastruttura {{site.data.keyword.cloud_notm}} in modo programmatico, nella SLAPI, puoi utilizzare l'API per automatizzare le attività.

La SLAPI è un sistema RPC (Remote Procedure Call). Ogni chiamata comporta l'invio di dati tramite l'endpoint API e la ricezione dei dati strutturati come ritorno. Il formato utilizzato per inviare e ricevere i dati con la SLAPI dipende da quale implementazione dell'API scegli. La SLAPI al momento utilizza SOAP, XML-RPC o REST per la trasmissione dei dati.

Per ulteriori informazioni sulla SLAPI o sulle API del servizio CDN (Content Delivery Network) {{site.data.keyword.cloud_notm}}, consulta le seguenti risorse nella rete di sviluppo {{site.data.keyword.cloud_notm}}:

* [SLAPI Overview](https://softlayer.github.io/ )
* [Getting Started with SLAPI](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API Guide](https://softlayer.github.io/article/php/ )

----

Per iniziare, questa è una sequenza di chiamata API consigliata da seguire:
* `listVendors` - fornisce l'elenco dei fornitori supportati
* `verifyOrder` - verifica se l'ordine può essere effettuato
* `placeOrder` - crea l'account CDN con il fornitore specificato. Possono essere create fino a 10 associazioni CDN dopo una corretta chiamata placeOrder.
* `createDomainMapping` - crea le associazioni CDN
* `verifyDomainMapping` - modifica lo stato CDN in _RUNNING_

Puoi utilizzare altre API dopo aver seguito la precedente sequenza.

[Il codice di esempio è disponibile per tutti i passi in questa sequenza di chiamata.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

**Devi** utilizzare il nome utente API e la chiave API di un utente con l'autorizzazione `CDN_ACCOUNT_MANAGE` per la maggior parte delle chiamate API presentate in questo documento. Controlla l'utente Master del tuo account se hai bisogno che ti venga abilitata questa autorizzazione. (A ogni account cliente IBM Cloud è fornito un utente Master).
{: note}

----
## API per fornitore
{: #api-for-vendor}

### listVendors
Questa API consente all'utente di elencare i fornitori CDN supportati. Il `vendorName` è necessario per creare un account CDN e per un'introduzione all'ordinazione della tua CDN.

* **Parametri obbligatori**: Nessuno
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Vendor`

  Il Contenitore fornitore e un esempio di utilizzo possono essere visualizzati qui: [Contenitore fornitore](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## API per Account
{: #api-for-account}

### verifyCdnAccountExists
Verifica che esista un account CDN per l'utente che richiama l'API, per il `vendorName` fornito.

* **Parametri obbligatori**: `vendorName`: fornisce il nome di un provider CDN valido.
* **Restituzione**: `true` se esiste un account, altrimenti restituisce `false`.

----
## API per Associazione dominio
{: #api-for-domain-mapping}

### createDomainMapping
Utilizzando gli input forniti, questa funzione crea un'associazione dominio per il fornitore indicato e la unisce all'ID account dell'infrastruttura {{site.data.keyword.cloud_notm}} dell'utente. L'account CDN deve prima essere creato utilizzando `placeOrder` perché questa API funzioni (vedi un esempio della chiamata API `placeOrder` negli [Esempi di codice](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)). Una volta creato il CDN, viene creato un `defaultTTL` con un valore di 3600 secondi.

* **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

  [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

  I seguenti attributi fanno parte del Contenitore di input e possono essere forniti quando si aggiorna un'associazione dominio (gli attributi sono facoltativi se non diversamente indicato):
    * `vendorName`: **obbligatorio** Fornisce il nome di un provider di IBM Cloud CDN valido.
    * `origin`: **obbligatorio** Fornisce l'indirizzo del server di origine come una stringa.
    * `originType`: **obbligatorio** Il tipo di origine può essere `HOST_SERVER` o `OBJECT_STORAGE`.
    * `domain`: **obbligatorio** Fornisce il tuo nome host come una stringa.
    * `protocol`: **obbligatorio** I protocolli supportati sono `HTTP`, `HTTPS` o `HTTP_AND_HTTPS`.
    * `certificateType`: **obbligatorio** per il protocollo HTTPS. `SHARED_SAN_CERT` o `WILDCARD_CERT`
    * `path`: il percorso da cui verrà fornito il contenuto memorizzato in cache. Il percorso predefinito è `/*`
    * `httpPort` e/o `httpsPort`: (**obbligatorio** per il server host) Queste due opzioni devono corrispondere al protocollo desiderato. Se il protocollo è `HTTP`, `httpPort` deve essere impostato e `httpsPort` _non_ deve essere impostato. In modo analogo, se il protocollo è `HTTPS`, `httpsPort` deve essere impostato e `httpPort` _non_ deve essere impostato. Se il protocollo è `HTTP_AND_HTTPS`, _sia_ `httpPort` che `httpsPort` _devono_ essere impostati. Akamai ha delle specifiche limitazioni sui numeri porta. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
    * `header`: specifica le informazioni sull'intestazione host utilizzata dal server di origine
    * `respectHeader`: un valore booleano che, se impostato su `true`, farà in modo che le impostazioni TTL nell'origine sovrascrivano le impostazioni TTL di CDN.
    * `cname`: fornisce un alias al nome host. Sarà generato se non ne viene fornito uno.
    * `bucketName`: (**obbligatorio** solo per l'Object Storage) Il nome bucket per il tuo Object Storage S3.
    * `fileExtension`: (facoltativo per Object Storage) Le estensioni file di cui è consentita la memorizzazione in cache.
    * `cacheKeyQueryRule`: per configurare la modalità di funzionamento della chiave della cache sono disponibili le seguenti opzioni. Se non viene fornito alcun argomento `cacheKeyQueryRule`, per impostazione predefinita sarà "include-all"
      * `include-all` - include tutti gli argomenti della query **valore predefinito**
      * `ignore-all` - ignora tutti gli argomenti della query
      * `ignore: space separated query-args` - ignora questi specifici argomenti della query. Ad esempio, `ignore: query1 query2`
      * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, `include: query1 query2`

* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`.

  **NOTE**: la raccolta fornisce un valore `uniqueId`, che deve essere inviato come input per le successive chiamate API correlate all'associazione e al percorso di origine.

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
Elimina l'associazione dominio in base all'`uniqueId`. L'associazione dominio deve essere in uno dei seguenti stati: _RUNNING_ (In esecuzione), _STOPPED_ (Arrestato), _DELETED_ (Eliminato), _ERROR_ (Errore), _CNAME_CONFIGURATION_ (Configurazione CNAME) o _SSL_CONFIGURATION_ (Configurazione SSL).

* **Parametri obbligatori**: `uniqueId`: l'ID univoco dell'associazione da eliminare
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`
  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
Verifica lo stato della CDN e aggiorna lo stato (`status`) dell'associazione CDN se ha subito variazioni. Quando un'associazione CDN viene creata inizialmente, il suo stato viene presentato come _CNAME_CONFIGURATION_. A questo punto, devi aggiornare il record di DNS in modo che l'associazione CDN punti il nome host al CNAME. Controlla il tuo provider DNS se hai domande su come viene eseguito l'aggiornamento e su dopo quanto tempo ha effetto la modifica e viene diffusa su internet. Normalmente, dovrebbero essere necessari tra i 15 e 30 minuti. Dopo tale tempo, deve essere richiamata questa API `verifyDomainMapping` per verificare se la catena CNAME è completa. Se la catena CNAME è completa, lo stato dell'associazione CDN viene modificato in _RUNNING_.

Questa API può venire richiamata in qualsiasi momento per ottenere l'ultimo stato dell'associazione CDN. L'associazione dominio deve essere in uno dei seguenti stati: _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parametri obbligatori**: `uniqueId`: l'ID univoco dell'associazione che desideri verificare
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
Avvia un'associazione dominio CDN in base all'`uniqueId`. Per essere avviata, l'associazione dominio deve essere in uno stato _STOPPED_.

* **Parametri obbligatori**: `uniqueId`: l'ID univoco dell'associazione da avviare
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
Arresta un'associazione dominio CDN in base all'`uniqueId`. Per iniziare l'arresto, l'associazione dominio deve essere in uno stato _RUNNING_.

* **Parametri obbligatori**: `uniqueId`: l'ID univoco dell'associazione da arrestare
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
Abilita l'utente ad aggiornare le proprietà dell'associazione identificata dall'`uniqueId`. I seguenti campi possono essere modificati: argomenti `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header` e `cacheKeyQueryRule` e, se il tuo tipo di origine è Object Storage, possono essere modificati anche `bucketName` e `fileExtension`. Affinché un aggiornamento venga eseguito, l'associazione dominio deve essere in uno stato _RUNNING_.

* **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Puoi visualizzare tutti gli attributi nel Contenitore di input qui:
  [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

  I seguenti attributi fanno parte del Contenitore di input e devono essere forniti **obbligatoriamente** quando si aggiorna un'associazione dominio:
    * `vendorName`: fornisce il nome del provider CDN per questa associazione.
    * `path`: fornisce il percorso corrente per questa associazione
    * `origin`: fornisce l'indirizzo del server di origine come una stringa.
    * `originType`: il tipo di origine può essere `HOST_SERVER` o `OBJECT_STORAGE`.
    * `domain`: fornisce il tuo nome host.
    * `protocol`: i protocolli supportati sono `HTTP`, `HTTPS` o `HTTP_AND_HTTPS`.
    * `httpPort` e/o `httpsPort`: queste due opzioni devono corrispondere al protocollo desiderato. Se il protocollo è `HTTP`, `httpPort` deve essere impostato e `httpsPort` _non_ deve essere impostato. In modo analogo, se il protocollo è `HTTPS`, `httpsPort` deve essere impostato e `httpPort` _non_ deve essere impostato. Se il protocollo è `HTTP_AND_HTTPS`, _sia_ `httpPort` che `httpsPort` _devono_ essere impostati. Akamai ha delle specifiche limitazioni sui numeri porta. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
    * `header`: specifica le informazioni sull'intestazione host utilizzata dal server di origine
    * `respectHeader`: un valore booleano che, se impostato su `true`, farà in modo che le impostazioni TTL nell'origine sovrascrivano le impostazioni TTL di CDN.
    * `uniqueId`: generato dopo la creazione dell'associazione.
    * `cname`: fornisce il cname. Uno è stato generato quando l'associazione è stata creata, se non ne hai fornito uno.
    * `bucketName`: (**obbligatorio** solo per l'Object Storage) Il nome bucket per il tuo Object Storage S3.
    * `fileExtension`: (**obbligatorio** solo per Object Storage) Le estensioni file di cui è consentita la memorizzazione in cache.
    * `cacheKeyQueryRule`: le regole di modalità di funzionamento delle chiavi della cache possono essere aggiornate solo per le associazioni CDN create _dopo_ il 16/11/17. Per configurare la modalità di funzionamento della chiave della cache sono disponibili le seguenti opzioni:
      * `include-all` - include tutti gli argomenti della query **valore predefinito**
      * `ignore-all` - ignora tutti gli argomenti della query
      * `ignore: space separated query-args` - ignora questi specifici argomenti della query. Ad esempio, `ignore: query1 query2`
      * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, `include: query1 query2`
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
Restituisce una raccolta di tutte le associazioni dominio per l'attuale cliente.

* **Parametri obbligatori**: Nessuno
* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
Restituisce una raccolta con un singolo oggetto dominio in base all'`uniqueId` di una CDN.

* **Parametri obbligatori**: `uniqueId`: l'ID univoco dell'associazione da restituire
* **Restituzione**: una raccolta di singoli oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`

  [Visualizza il Contenitore associazione](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## API per origine
{: #apis-for-origin}

### createOriginPath
Crea un percorso di origine per una CDN esistente e per un particolare cliente. Il percorso di origine può essere basato su un server host o un Object Storage. Per creare il percorso di origine, l'associazione dominio deve essere in uno stato _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

  [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

  I seguenti attributi fanno parte del Contenitore di input e possono essere forniti quando si crea un percorso di origine (gli attributi sono facoltativi se non diversamente indicato):
    * `vendorName`: **obbligatorio** Fornisce il nome di un provider di IBM Cloud CDN valido.
    * `origin`: **obbligatorio** Fornisce l'indirizzo del server di origine come una stringa.
    * `originType`: **obbligatorio** Il tipo di origine può essere `HOST_SERVER` o `OBJECT_STORAGE`.
    * `domain`: **obbligatorio** Fornisce il tuo nome host come una stringa.
    * `protocol`: **obbligatorio** I protocolli supportati sono `HTTP`, `HTTPS` o `HTTP_AND_HTTPS`.
    * `path`: il percorso da cui verrà fornito il contenuto memorizzato in cache. Deve iniziare con il percorso associazione. Ad esempio, se il percorso associazione è `/test`, il tuo percorso di origine può essere `/test/media`
    * `httpPort` e/o `httpsPort`: **obbligatorio** Queste due opzioni devono corrispondere al protocollo desiderato. Se il protocollo è `HTTP`, `httpPort` deve essere impostato e `httpsPort` _non_ deve essere impostato. In modo analogo, se il protocollo è `HTTPS`, `httpsPort` deve essere impostato e `httpPort` _non_ deve essere impostato. Se il protocollo è `HTTP_AND_HTTPS`, _sia_ `httpPort` che `httpsPort` _devono_ essere impostati. Akamai ha delle specifiche limitazioni sui numeri porta. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
    * `header`: specifica le informazioni sull'intestazione host utilizzata dal server di origine
    * `uniqueId`: **obbligatorio** Generato dopo la creazione dell'associazione.
    * `cname`: fornisce un alias al nome host. Se non avevi fornito un nome univoco, ne è stato generato uno per te quando è stata creata l'associazione.
    * `bucketName`: (**obbligatorio** per l'Object Storage) Il nome bucket per il tuo Object Storage S3.
    * `fileExtension`: (facoltativo per Object Storage) Le estensioni file di cui è consentita la memorizzazione in cache.
    * `cacheKeyQueryRule`: per configurare la modalità di funzionamento della chiave della cache sono disponibili le seguenti opzioni:
      * `include-all` - include tutti gli argomenti della query **valore predefinito**
      * `ignore-all` - ignora tutti gli argomenti della query
      * `ignore: space separated query-args` - ignora questi specifici argomenti della query. Ad esempio, `ignore: query1 query2`
      * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, `include: query1 query2`

* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Visualizza il Contenitore percorso di origine](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
Aggiorna un percorso di origine per un'associazione esistente e per un particolare cliente. Il tipo di origine non può essere modificato con questa API. È possibile modificare le seguenti proprietà: gli argomenti `path`, `origin`, `httpPort`, `httpsPort`, `header` e `cacheKeyQueryRule`. Per essere aggiornata, l'associazione dominio deve essere in uno stato _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
  Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

  [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

  I seguenti attributi fanno parte del Contenitore di input e possono essere forniti quando si aggiorna un percorso di origine (gli attributi sono facoltativi se non diversamente indicato):
    * `oldPath`: **obbligatorio** L'attuale percorso da modificare
    * `origin`: (**obbligatorio** se in fase di aggiornamento) Fornisce l'indirizzo del server di origine come una stringa.
    * `originType`: **obbligatorio** Il tipo di origine può essere `HOST_SERVER` o `OBJECT_STORAGE`.
    * `path`: **obbligatorio** Nuovo percorso da aggiungere. Relativo al percorso di associazione.
    * `httpPort` e/o `httpsPort`: (**obbligatorio** per il server host, se in fase di aggiornamento) Queste due opzioni devono corrispondere al protocollo desiderato. Se il protocollo è `HTTP`, `httpPort` deve essere impostato e `httpsPort` _non_ deve essere impostato. In modo analogo, se il protocollo è `HTTPS`, `httpsPort` deve essere impostato e `httpPort` _non_ deve essere impostato. Se il protocollo è `HTTP_AND_HTTPS`, _sia_ `httpPort` che `httpsPort` _devono_ essere impostati. Akamai ha delle specifiche limitazioni sui numeri porta. Consulta le [Domande frequenti (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) per un elenco dei numeri porta consentiti.
    * `uniqueId`: **obbligatorio** L'ID univoco dell'associazione a cui appartiene questa origine
    * `bucketName`: (**obbligatorio** solo per l'Object Storage) Il nome bucket per il tuo Object Storage S3.
    * `fileExtension`: (**obbligatorio** solo per Object Storage) Le estensioni file di cui è consentita la memorizzazione in cache.
    * `cacheKeyQueryRule`: (**obbligatorio** se in fase di aggiornamento) Le regole di modalità di funzionamento delle chiavi della cache possono essere aggiornate solo per i percorsi di origine creati _dopo_ il 16/11/17. Per configurare la modalità di funzionamento delle chiavi della cache sono disponibili le seguenti opzioni:
      * `include-all` - include tutti gli argomenti della query **valore predefinito**
      * `ignore-all` - ignora tutti gli argomenti della query
      * `ignore: space separated query-args` - ignora questi specifici argomenti della query. Ad esempio, `ignore: query1 query2`
      * `include: space separated query-args`: include questi specifici argomenti della query. Ad esempio, `include: query1 query2`

* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Visualizza il Contenitore percorso di origine](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
Elimina un percorso di origine esistente per una CDN esistente e per uno specifico cliente. Per essere eliminata, l'associazione dominio deve essere in uno stato _RUNNING_ o _CNAME_CONFIGURATION_.

* **Parametri obbligatori**:
  * `uniqueId`: l'ID univoco dell'associazione a cui appartiene questo percorso di origine
  * `path`: percorso da eliminare

* **Restituzione**: un messaggio di stato se l'eliminazione è riuscita; altrimenti, viene generata un'eccezione.

----
### listOriginPath
Elenca i percorsi di origine per un'associazione esistente in base all'`uniqueId`.

* **Parametri obbligatori**:
  * `uniqueId`: fornisce l'ID univoco dell'associazione per cui vuoi elencare i percorsi di origine.
* **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`

  [Visualizza il Contenitore percorso di origine](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## API per Eliminazione
{: #api-for-purge}

### Classe contenitore per l'eliminazione:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
Crea un record di eliminazione e lo inserisce nel database.

* **Parametri**:
  * `uniqueId`: ID univoco dell'associazione per cui verrà creata l'eliminazione
  * `path`: il percorso dell'eliminazione da creare

* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### getPurgeHistoryPerMapping
Restituisce la cronologia delle eliminazioni per una CDN in base all'`uniqueId` e allo stato `saved`. (Per impostazione predefinita, il valore `saved` è impostato su _UNSAVED_ (non salvato).

* **Parametri**:
  * `uniqueId`: l'ID univoco dell'associazione per cui richiamare la cronologia delle eliminazioni
  * `saved`: restituisce le eliminazioni che sono state salvate (_SAVED_) o non salvate (_UNSAVED_)

* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
### saveOrUnsavePurgePath
Aggiorna lo stato della voce di percorso di eliminazione per indicare se questo percorso di eliminazione deve essere salvato. Se un percorso di eliminazione viene salvato, crea una nuova eliminazione `saved`. Se il percorso è `unsaved`, elimina un record di eliminazione salvata.

* **Parametri**:
  * `uniqueId`: l'ID univoco dell'associazione a cui appartiene l'eliminazione
  * `path`: il percorso dell'eliminazione da salvare o non salvare
  * `saved`: _SAVED_ o _UNSAVED_

* **Restituzione**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`

----
## API per Time to Live
{: #api-for-time-to-live}

### Variabili classe TimeToLive:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
Crea un nuovo oggetto `TimeToLive` e lo inserisce nel database.

 * **Parametri**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **Restituzione**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### updateTtl
Aggiorna un oggetto `TimeToLive` esistente. Se gli input _oldTtl_ e _newTtl_ sono uguali, termina prima.

 * **Parametri**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **Restituzione**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`
___
### deleteTtl
Elimina un oggetto `TimeToLive` esistente dal database.

 * **Parametri**: `string` `uniqueId`, `string` `pathName`
 * **Restituzione**: una stringa con lo stato dell'eliminazione
___
### listTtl
Elenca gli oggetti `TimeToLive` esistenti in base all'`uniqueId` di una CDN.

 * **Parametri**: `string` `uniqueId`
 * **Restituzione**: un array di oggetti di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`

 ----
## API per Metriche
{: #api-for-metrics}

[Visualizza il Contenitore di metriche](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
Restituisce il numero totale di statistiche predeterminate per la visualizzazione diretta (senza grafici) per l'account di un cliente, per un determinato periodo di tempo.

 * **Parametri**:
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingUsageMetrics
Restituisce il numero totale di statistiche predeterminate per la visualizzazione diretta per l'associazione fornita. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsMetrics
Restituisce il numero totale di riscontri a una certa frequenza per un determinato intervallo di tempo, per associazione dominio. La frequenza può essere 'day', 'week' e 'month', dove ogni intervallo è un punto per il grafico. I dati di restituzione sono ordinati in base a `startDate`, `endDate` e `frequency`. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingHitsByTypeMetrics
Restituisce il numero totale di riscontri a una certa frequenza per un determinato intervallo di tempo. La frequenza può essere 'day', 'week' e 'month', dove ogni intervallo è un punto per il grafico. I dati di restituzione devono essere ordinati in base a `startDate`, `endDate` e `frequency`. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthMetrics
Restituisce il numero di riscontri edge per una singola CDN. Le regioni possono differire per ogni fornitore. Per associazione.

 * **Parametri**:  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`
___
### getMappingBandwidthByRegionMetrics
Restituisce il numero totale di statistiche predeterminate per la visualizzazione diretta (senza grafici) per un'associazione fornita, per un determinato periodo di tempo. Il valore di `frequency` è 'aggregate' per impostazione predefinita.

 * **Parametri**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **Restituzione**: una raccolta di oggetti di tipo `SoftLayer_Container_Network_CdnMarketplace_Metrics`

----
## API per Controllo dell'accesso geografico
{: #api-for-geographical-access-control}

### createGeoblocking
Crea una nuova regola di Controllo dell'accesso geografico e restituisce la regola appena creata.

  * **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

    [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

    I seguenti attributi fanno parte del Contenitore di input e sono **obbligatori** quando si crea una nuova regola di Controllo dell'accesso geografico.
    * `uniqueId`: ID univoco dell'associazione per assegnare la regola
    * `accessType`: specifica se la regola consentirà (`ALLOW`) o negherà (`DENY`) il traffico alla regione indicata
    * `regionType`: il tipo di regione a cui applicare la regola di Controllo dell'accesso geografico, `CONTINENT` o `COUNTRY_OR_REGION`
    * `regions`: un array che elenca le ubicazioni a cui si applicherà l'`accessType`

      Vedi la pagina [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) per visualizzare un elenco di possibili regioni.

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizza la classe di blocco geografico (geo-blocking)](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
Aggiorna una regola di Controllo dell'accesso geografico esistente per un'associazione di dominio esistente e restituisce la regola aggiornata.

  * **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

    [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

    I seguenti attributi fanno parte del Contenitore di input e possono essere forniti quando si aggiorna una regola di Controllo dell'accesso geografico (i parametri sono facoltativi se non diversamente indicato):
    * `uniqueId`: ID univoco **obbligatorio** dell'associazione a cui appartiene la regola da aggiornare
    * `accessType`: specifica se la regola consentirà (`ALLOW`) o negherà (`DENY`) il traffico alla regione indicata.
    * `regionType`: il tipo di regione a cui applicare la regola, `CONTINENT` o `COUNTRY_OR_REGION`
    * `regions`: un array che elenca le ubicazioni a cui si applicherà l'`accessType`

      Vedi la pagina [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) per visualizzare un elenco di possibili regioni.

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizza la classe di blocco geografico (geo-blocking)](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
Rimuove una regola di Controllo dell'accesso geografico esistente da un'associazione di dominio esistente.

  * **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

    [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

    Il seguente attributo fa parte del Contenitore di input ed è **obbligatorio** quando si elimina una regola di Controllo dell'accesso geografico:
    * `uniqueId`: fornisce l'ID univoco dell'associazione a cui appartiene la regola da eliminare.

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizza la classe di blocco geografico (geo-blocking)](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
Richiama la modalità di funzionamento di Controllo dell'accesso geografico di un'associazione dal database.

  * **Parametri**:
    * `uniqueId`: l'ID univoco dell'associazione a cui appartiene la regola.

  * **Restituisce**: un oggetto di tipo
     `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`

    [Visualizza la classe di blocco geografico (geo-blocking)](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
Restituisce un elenco dei tipi e delle regioni consentiti per la creazione di regole di Controllo dell'accesso geografico.

  * **Parametri**:
    * `uniqueId`: l'ID univoco della tua associazione di dominio.

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type`

    [Visualizza la classe di blocco geografico (geo-blocking)](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## API per Hotlink Protection
{: #api-for-hotlink-protection}

### createHotlinkProtection
Crea una nuova Hotlink Protection e restituisce la modalità di funzionamento appena creata.

  * **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

    [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

    I seguenti attributi sono parte del Contenitore di input e sono **obbligatori** quando si crea una nuova Hotlink Protection:
    * `uniqueId`: ID univoco dell'associazione a cui assegnare la modalità di funzionamento
    * `protectionType`: specifica se consentire ("ALLOW") o rifiutare ("DENY") l'accesso al tuo contenuto quando una pagina web fa una richiesta di contenuto con un valore di intestazione Referer corrispondente a uno dei termini nei refererValues specificati.
    * `refererValues`: un elenco separato da spazi singoli di termini di corrispondenza di URL Referer per cui diventerà effettiva la modalità di funzionamento `protectionType`

      Vedi la pagina [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) per visualizzare un elenco di valori Hotlink Protection validi.

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Visualizza la classe Hotlink Protection](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
Aggiorna una modalità di funzionamento Hotlink Protection esistente per un'associazione di dominio esistente e restituisce la modalità di funzionamento aggiornata.

  * **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

    [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

    I seguenti attributi sono parte del Contenitore di input e sono **obbligatori** quando si aggiorna una Hotlink Protection esistente:
    * `uniqueId`: l'ID univoco dell'associazione a cui appartiene la modalità di funzionamento esistente
    * `protectionType`: specifica se consentire ("ALLOW") o rifiutare ("DENY") l'accesso al tuo contenuto quando una pagina web fa una richiesta di contenuto con un valore di intestazione Referer corrispondente a uno dei termini nei refererValues specificati. 
    * `refererValues`: un elenco separato da spazi singoli di termini di corrispondenza di URL Referer per cui diventerà effettiva la modalità di funzionamento `protectionType`

      Vedi la pagina [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) per visualizzare un elenco di valori Hotlink Protection validi.

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Visualizza la classe Hotlink Protection](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
Rimuove una modalità di funzionamento Hotlink Protection esistente da un'associazione di dominio esistente.

  * **Parametri**: una raccolta di tipo `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`.
    Puoi visualizzare tutti gli attributi nel Contenitore di input qui di seguito:

    [Visualizza il Contenitore di input](/docs/infrastructure/CDN?topic=CDN-input-container)

    I seguenti attributi sono parte del Contenitore di input e sono **obbligatori** quando si crea una nuova Hotlink Protection:
    * `uniqueId`: ID univoco dell'associazione da cui rimuovere la modalità di funzionamento

  * **Restituisce**: null

----
### getHotlinkProtection
Richiama la modalità di funzionamento Hotlink Protection corrente di un'associazione.

  * **Parametri**:
    * `uniqueId`: l'ID univoco dell'associazione a cui appartiene la modalità di funzionamento

  * **Restituisce**: un oggetto di tipo `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`

    [Visualizza la classe Hotlink Protection](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
