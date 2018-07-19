---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gestisci la tua CDN

## Impostazione del tempo di memorizzazione dei contenuti nella cache utilizzando "Time To Live"

Una volta che la tua CDN è in esecuzione, puoi impostare il tempo di memorizzazione dei contenuti nella cache utilizzando TTL (Time To Live). Il Time To Live per un determinato percorso di file o directory indica per quanto tempo i contenuti debbano restare memorizzati nella cache. Quando hai creato l'Associazione CDN, è stato creato un TTL globale predefinito di 3600 secondi.

**Passo 1:**  

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Panoramica**.

**Passo 2:**  

Puoi modificare il tempo utilizzando le frecce o immettendo un nuovo valore. Il valore di tempo è specificato in secondi. Ad esempio, 3600 secondi equivale a 1 ora. Il valore minimo per `timeToLive` è 30 secondi, mentre il valore massimo è 2147483647 secondi. Seleziona il pulsante **Salva** per impostare il tempo di memorizzazione dei contenuti nella cache.

  ![Aggiunta di ttl](images/adding-path.png)

**Passo 3:**

Dopo aver salvato, puoi **Modificare** o **Eliminare** l'impostazione TTL utilizzando le opzioni del menu di overflow. (**NOTA**: non è possibile modificare il percorso per TTL. Se il percorso associazione viene modificato, il percorso TTL viene aggiornato automaticamente).

  ![Modifica o elimina ttl](images/edit-delete-ttl-setting.png)  

  * Quando il contenuto corrisponde a più regole, ha la precedenza la configurazione aggiunta più di recente.

  * I valori TTL possono essere impostati solo per uno specifico nome file o directory. Le espressioni regolari non sono supportate perché potrebbero creare comportamenti imprevedibili.

## Aggiunta dei dettagli del percorso di origine

Quando la tua CDN è in stato *Configurazione_CNAME* o *In esecuzione*, puoi aggiungere i dettagli del percorso di origine. Puoi scegliere di fornire il contenuto da più server di origine. Ad esempio, le foto possono essere fornite da un server diverso da quello dei video. L'origine può essere basata su un Server host o su Object Storage.

**Nota:** il CDN fa una trasformazione URL per il server di origine. Ad esempio, se viene aggiunta l'origine `xyz.example.com` con il percorso `/example/*`, quando un utente apre l'URL `www.example.com/example/*` il server edge CDN richiama il contenuto da `xyz.example.com/*`.

**Passo 1:**

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Panoramica**.  

**Passo 2:**

Seleziona la scheda **Origini** e fai clic sul pulsante **Aggiungi origine**. Questo passo apre una nuova finestra di dialogo, dove puoi configurare la tua origine.  

   ![Origini - Aggiungi origine](images/add-origins.png)

**Passo 3:**

*Devi* fornire un percorso. Il percorso *deve* iniziare con il percorso del CDN come prefisso, se il CDN è stato creato con un percorso.  
  Ad esempio, se il CDN è stato creato con un percorso di `/examplePath`, il percorso per l'origine deve iniziare con il prefisso `/examplePath/`. Puoi facoltativamente fornire un intestazione host.  

   ![Origini - Aggiungi origine](images/add-origin-path.png)

**Passo 4:**

Seleziona **Server** o **Object Storage**.

  * Se hai selezionato **Server**, immetti l'indirizzo del server di origine come un indirizzo IPv4 o il _nome host_. Si consiglia di fornire il nome host e un FQDN (Fully Qualified Domain Name). A seconda del protocollo che hai selezionato durante la creazione di CDN, fornisci anche una porta HTTP, una porta HTTPS o entrambe. Se utilizzi una porta HTTPS, l'indirizzo del server di origine **deve** essere un _nome host_ e non un indirizzo IP.

       ![Aggiungi origine - Server](images/add-origin-server-default.png)

  * Se hai selezionato **Object Storage**, fornisci l'endpoint, il nome bucket e la porta HTTPS. Facoltativamente, specifica le estensioni file che possono essere utilizzate nel servizio CDN. Se non si specifica nulla, sono consentite tutte le estensioni file.

       ![Aggiungi origine - Object Storage](images/add-origin-object-storage.png)

  * Le opzioni **Ottimizzazione** e **Chiave della cache** sono uguali per le configurazioni di server e Object Storage.

    * Scegli le opzioni di **Ottimizzazione** dal menu a discesa. **Fornitura web generale** è l'opzione predefinita, oppure puoi scegliere le ottimizzazioni **File di grandi dimensioni** o **Video on-demand**. **Fornitura web generale** consente alla CDN di fornire contenuto fino a 1,8GB, mentre l'ottimizzazione **File di grandi dimensioni** consente download di file da 1,8GB a 320GB. **Video on-demand** ottimizza la tua CDN per la fornitura di formati di streaming segmentati. Le descrizioni delle funzioni per l'[Ottimizzazione dei file di grandi dimensioni](about.html#large-file-optimization) e [Video on-demand](about.html#video-on-demand) forniscono ulteriori informazioni.

        ![Opzioni di configurazione delle prestazioni](images/performance-config-options.png)

    * Scegli le opzioni **Chiave della cache** dal menu a discesa. L'opzione predefinita è **Includi tutto**. Se selezioni **Includi specificato** o **Ignora specificato**, **devi** immettere le stringhe di query da includere o ignorare, separate da uno spazio. Ad esempio, immetti `uuid=123456` per una singola stringa di query oppure `uuid=123456 issue=important` per due stringhe di query.  Puoi saperne di più sugli [Argomenti di query della chiave della cache](about.html#cache-key-query-args) nella descrizione della funzione.

        ![Opzioni della chiave della cache](images/cache-key-options.png)

**NOTA**: le opzioni protocollo e porta visualizzate dall'IU corrisponderanno a quanto selezionato quando è stata ordinata la CDN. Ad esempio, se è stata selezionata la **Porta HTTP** come parte dell'ordinazione di una CDN, come parte di Aggiungi origine viene visualizzata solo l'opzione **Porta HTTP**.

**Passo 5:**

Seleziona il pulsante **Aggiungi** per aggiungere il tuo percorso di origine.

  **Nota**: quando fornisci le estensioni file per un percorso di origine Object Storage, l'impostazione TTL che ha lo stesso URL del percorso di origine è definita con un ambito per includere tutti i file che hanno tali estensioni file specificate. Ad esempio, se crei un percorso di origine `/example` e specifichi l'estensione file "jpg png gif", il valore TTL del percorso TTL `/example` avrà un ambito che include tutti i file JPG/PNG/GIF nella directory `/example` e nelle sue sottodirectory.

**Passo 6:**

Dopo l'aggiunta, puoi **Modificare** o **Eliminare** l'origine utilizzando le opzioni del menu di overflow.

  ![Modifica o elimina origine](images/edit-delete-origin.png)

## Eliminazione del contenuto memorizzato nella cache

Una volta che la tua CDN è in esecuzione, puoi eliminare il contenuto memorizzato nella cache dal server del fornitore.

**Passo 1:**

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Panoramica**.

**Passo 2:**

Seleziona la scheda **Elimina**.

   ![Pagina Elimina](images/purge_tab.png)

**Passo 3:**

Immetti la sintassi del percorso unix standard per indicare quale file vuoi eliminare, quindi seleziona il pulsante **Elimina**. L'eliminazione è consentita solo per un singolo file in questo momento. Consulta la pagina [Regole e convenzioni di denominazione](rules-and-naming-conventions.html#what-are-the-rules-for-the-path-string-for-purge-) per ulteriori dettagli su quale sintassi è consentita per il percorso di eliminazione.

**Passo 4:**

Dopo l'eliminazione, l'attività viene elencata in **Attività di eliminazione**. Puoi selezionare **Ripeti eliminazione** o **Favorito** per il percorso utilizzando le opzioni del menu di overflow.

   ![Attività di eliminazione](images/purge-activity.png)

   **NOTA:** se ci sono più di 15 eliminazioni, l'attività di eliminazione viene troncata automaticamente ogni 15 giorni.

## Aggiornamento dei dettagli della configurazione CDN

Una volta che la tua CDN è in esecuzione, puoi aggiornare i dettagli della configurazione CDN.

**Passo 1:**

Nella pagina CDN, seleziona la tua CDN; verrà aperta la pagina **Panoramica**.

**Passo 2:**

Seleziona la scheda **Impostazioni**. Vengono visualizzati i dettagli della tua configurazione CDN.

   ![Scheda Impostazioni](images/settings-tab.png)  
   **NOTA**: vedrai il certificato SSL solo se la tua CDN è stata configurata con HTTPS.

Per **Server**, è possibile modificare i seguenti campi:
  * Intestazione host
  * Indirizzo server di origine
  * Porta HTTP/HTTPS
  * Utilizzo di contenuto obsoleto
  * Respect Headers (Rispetta intestazioni)
  * Opzioni di ottimizzazione
  * Cache-query    

Per **Object Storage**, è possibile modificare i seguenti campi:
  * Intestazione host
  * Endpoint
  * Nome bucket
  * Porta HTTPS
  * Estensioni file consentite
  * Utilizzo di contenuto obsoleto
  * Respect Headers (Rispetta intestazioni)
  * Opzioni di ottimizzazione
  * Cache-query

**Passo 3:**

Aggiorna i dettagli **Origine** o **Altre opzioni** laddove necessario, quindi fai clic sul pulsante **Salva** nell'angolo inferiore destro per aggiornare i dettagli della tua configurazione CDN.

   ![Pulsante Salva](images/save-button.png)

## Configura IBM Cloud Object Storage per CDN

Per utilizzare gli oggetti memorizzati in IBM Cloud Object Storage, devi impostare il valore della proprietà "acl" (ossia, l'elenco di controllo degli accessi) per ogni oggetto nel tuo bucket per l'accesso "public-read".

Consulta la sezione Strumenti in IBM Cloud Object Storage Developer Center (https://developer.ibm.com/cloudobjectstorage/) per installare eventuali client o strumenti necessari. Questa guida presume che tu abbia installato l'interfaccia della riga di comando AWS ufficiale, che è compatibile con l'API IBM Cloud Object Storage S3.

Il codice di esempio riportato di seguito mostra come impostare l'accesso "public-read" per tutti gli oggetti presenti nel tuo bucket, utilizzando l'interfaccia riga di comando.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```
