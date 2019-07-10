---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

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

# Gestisci la tua CDN
{: #manage-your-cdn}

Questo documento descrive le attività comuni per gestire la tua CDN.

## Impostazione del tempo di memorizzazione dei contenuti nella cache utilizzando "Time To Live"
{: #setting-content-caching-time-using-time-to-live}

Una volta che la tua CDN è in esecuzione, puoi impostare il tempo di memorizzazione dei contenuti nella cache utilizzando TTL (Time To Live). Il Time To Live per un determinato percorso di file o directory indica per quanto tempo i contenuti debbano restare memorizzati nella cache. Quando hai creato l'associazione CDN, è stato creato un TTL globale predefinito di 3600 secondi (1 ora).

**Passo 1:**  

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Overview**.

**Passo 2:**  

Puoi modificare il tempo utilizzando le frecce o immettendo un nuovo valore. Il valore di tempo è specificato in secondi. Ad esempio, 3600 secondi equivale a 1 ora. Il valore più piccolo per `timeToLive` che è possibile scegliere è 0 secondi mentre quello più grande è 2147483647 secondi (circa 24855 giorni). Seleziona il pulsante **Save** per impostare il tempo di memorizzazione dei contenuti nella cache.

  ![Aggiunta di ttl](images/adding-path.png)

**Passo 3:**

Dopo aver salvato, puoi modificare (**Edit**) o eliminare (**Delete**) l'impostazione TTL utilizzando le opzioni del menu Overflow. (**NOTA**: non è possibile modificare il percorso per TTL. Se il percorso associazione viene modificato, il percorso TTL viene aggiornato automaticamente).

  ![Modifica o elimina ttl](images/edit-delete-ttl-setting.png)  

  * Quando il contenuto corrisponde a più regole, ha la precedenza la configurazione aggiunta più di recente.

  * I valori TTL possono essere impostati solo per uno specifico nome file o directory. Le espressioni regolari non sono supportate perché potrebbero creare una modalità di funzionamento imprevedibile.

## Aggiunta dei dettagli del percorso di origine
{: #adding-origin-path-details}

Quando la tua CDN è in stato *CNAME_Configuration* o *Running*, puoi aggiungere i dettagli del percorso di origine. Puoi scegliere di fornire il contenuto da più server di origine. Ad esempio, le foto possono essere fornite da un server diverso da quello dei video. L'origine può essere basata su un Server host o su Object Storage.

La CDN fa una trasformazione URL per il server di origine. Ad esempio, se viene aggiunta l'origine `xyz.example.com` con il percorso `/example/*`, quando un utente apre l'URL `www.example.com/example/*` il server edge CDN richiama il contenuto da `xyz.example.com/*`.
{: note}

**Passo 1:**

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Overview**.  

**Passo 2:**

Seleziona la scheda **Origins** e fai clic sul pulsante **Add Origin**. Questo passo apre una nuova finestra di dialogo, dove puoi configurare la tua origine.  

   ![Origini - Aggiungi origine](images/add-origins.png)

**Passo 3:**

*Devi* fornire un percorso. Puoi facoltativamente fornire un intestazione host.  

   ![Origini - Aggiungi origine](images/add-origin-path.png)

**Passo 4:**

Seleziona **Server** o **Object Storage**.

  * Se hai selezionato **Server**, immetti l'indirizzo del server di origine come un indirizzo IPv4 o il _nome host_. Si consiglia di fornire il nome host e un FQDN (Fully Qualified Domain Name). A seconda del protocollo che hai selezionato durante la creazione di CDN, fornisci anche una porta HTTP, una porta HTTPS o entrambe. Se utilizzi una porta HTTPS, l'indirizzo del server di origine **deve** essere un _nome host_ e non un indirizzo IP.

       ![Aggiungi origine - Server](images/add-origin-server-default.png)

  * Se hai selezionato **Object Storage**, fornisci l'endpoint, il nome bucket e la porta HTTPS. Facoltativamente, specifica le estensioni file che possono essere utilizzate nel servizio CDN. Se non si specifica nulla, sono consentite tutte le estensioni file.

       ![Aggiungi origine - Object Storage](images/add-origin-object-storage.png)

  * Le opzioni **Optimization** e **Cache Key** sono uguali per le configurazioni di server e Object Storage.

    * Scegli le opzioni di **Optimization** dal menu a discesa. **General web delivery** è l'opzione predefinita, oppure puoi scegliere le ottimizzazioni **Large file** o **Video on demand**. **General web delivery** consente alla CDN di fornire contenuto fino a 1,8GB, mentre l'ottimizzazione **Large file** consente download di file da 1,8GB a 320GB. **Video on demand** ottimizza la tua CDN per la fornitura di formati di streaming segmentati. Le descrizioni delle funzioni per l'[Ottimizzazione dei file di grandi dimensioni](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization) e [Video on-demand](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand) forniscono ulteriori informazioni.

        ![Opzioni di configurazione delle prestazioni](images/performance-config-options.png)

    * Scegli le opzioni **Cache Key** dal menu a discesa. L'opzione predefinita è **Include-all**. Se selezioni **Include specified** o **Ignore specified**, **devi** immettere le stringhe di query da includere o ignorare, separate da uno spazio. Ad esempio, immetti `uuid=123456` per una singola stringa di query oppure `uuid=123456 issue=important` per due stringhe di query.  Puoi saperne di più sugli [Argomenti di query della chiave della cache](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args) nella descrizione della funzione.

        ![Opzioni della chiave della cache](images/cache-key-options.png)

Le opzioni protocollo e porta visualizzate dall'IU corrisponderanno a quanto selezionato quando è stata ordinata la CDN. Ad esempio, se è stata selezionata la porta HTTP (**HTTP port**) come parte dell'ordinazione di una CDN, come parte di Add Origin viene visualizzata solo l'opzione **HTTP port**.
{: note}

**Passo 5:**

Seleziona il pulsante **Add** per aggiungere il tuo percorso di origine.

Quando fornisci le estensioni file per un percorso di origine Object Storage, l'impostazione TTL che ha lo stesso URL del percorso di origine è definita con un ambito per includere tutti i file che hanno tali estensioni file specificate. Ad esempio, se crei un percorso di origine `/example` e specifichi l'estensione file "jpg png gif", il valore TTL del percorso TTL `/example` avrà un ambito che include tutti i file JPG/PNG/GIF nella directory `/example` e nelle sue sottodirectory.
{: note}

**Passo 6:**

Dopo l'aggiunta, puoi modificare (**Edit**) o eliminare (**Delete**) l'origine utilizzando le opzioni del menu Overflow.

  ![Modifica o elimina origine](images/edit-delete-origin.png)

## Eliminazione del contenuto memorizzato nella cache
{: #purging-cached-content}

Una volta che la tua CDN è in esecuzione, puoi eliminare il contenuto memorizzato nella cache dal server del fornitore.

**Passo 1:**

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Overview**.

**Passo 2:**

Seleziona la scheda **Purge**.

   ![Pagina Purge](images/purge_tab.png)

**Passo 3:**

Immetti la sintassi del percorso unix standard per indicare quale file vuoi eliminare, quindi seleziona il pulsante **Purge**. L'eliminazione è consentita solo per un singolo file alla volta. Consulta la pagina [Regole e convenzioni di denominazione](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-) per ulteriori dettagli su quale sintassi è consentita per il percorso di eliminazione.

**Passo 4:**

Dopo l'eliminazione, l'attività viene elencata in **Purge Activity**. Puoi selezionare **Redo purge** o **Favorite** per il percorso utilizzando le opzioni del menu Overflow.

   ![Attività di eliminazione](images/purge-activity.png)

Se ci sono più di 15 eliminazioni, l'attività di eliminazione viene troncata automaticamente ogni 15 giorni.
{: note}

## Aggiornamento dei dettagli della configurazione CDN
{: #updating-cdn-configuration-details}

Una volta che la tua CDN è in esecuzione, puoi aggiornare i dettagli della configurazione CDN.

**Passo 1:**

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Overview**.

**Passo 2:**

Seleziona la scheda **Settings**. Vengono visualizzati i dettagli della tua configurazione CDN.

   ![Scheda Settings](images/settings-tab.png)  

Vedrai il certificato SSL solo se la tua CDN è stata configurata con HTTPS.
{: note}

Per **Server**, è possibile modificare i seguenti campi:
  * Host header
  * Origin server address
  * HTTP/HTTPS Port
  * Serve Stale Content
  * Respect Headers
  * Optimization options
  * Cache-query    

Per **Object Storage**, è possibile modificare i seguenti campi:
  * Host header
  * Endpoint
  * Bucket name
  * HTTPS Port
  * Allowed file extensions
  * Serve Stale Content
  * Respect Headers
  * Optimization options
  * Cache-query

**Passo 3:**

Aggiorna i dettagli **Origin** o **Other Options** laddove necessario, quindi fai clic sul pulsante **Save** nell'angolo inferiore destro per aggiornare i dettagli della tua configurazione CDN.

   ![Pulsante Save](images/save-button.png)
