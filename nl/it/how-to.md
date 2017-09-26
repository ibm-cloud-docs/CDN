---

copyright:
  years: 2017
lastupdated: "2017-09-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gestisci il tuo CDN

Impara a gestire la tua configurazione CDN seguendo queste linee guida.
 
## Passaggio allo stato di esecuzione per il CDN

Una volta che hai creato un CDN, esso viene visualizzato nel tuo dashboard CDN. Qui puoi visualizzare il nome del CDN, l'origine, il provider e lo stato.  

 ![Schermata elenco di associazioni](images/mapping_list_cname.png)

1. Dopo che hai ordinato un CDN, dovrai configurare il **CNAME** con il tuo provider DNS. La maggior parte dei provider DNS può fornirti istruzioni sull'impostazione o sulla modifica del CNAME.
   
   * Durante questo periodo, lo stato del tuo CDN verrà mostrato come **Configurazione CNAME**. Contatta il tuo provider DNS per scoprire quando le modifiche diventeranno attive.

   ![Configurazione CNAME](images/cname-config.png)  
   
2. Dopo aver configurato il CNAME con il tuo provider DNS, puoi controllare lo stato in qualsiasi momento selezionando **Ottieni stato** dal menu di overflow alla destra dello stato CDN.
   
  ![CNAME getStatus](images/cname-getstatus.png)  
    
3. Una volta completato il concatenamento CNAME, lo stato dell'account cambia in *IN ESECUZIONE* e il CDN è pronto per l'uso.
   	  
Complimenti! Il tuo CDN è ora in esecuzione.  
	  
## Arresto del CDN
Un CDN può essere arrestato solo quando è in stato 'In esecuzione'. 

1. Fai clic su 'Arresta CDN' dal menu di overflow (3 punti verticali a destra dello stato CDN).
 ![Menu di overflow](images/stop_cdn.png)

2. Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler avviare il servizio. Seleziona **Conferma** per continuare.
  
3. Entro circa 5 - 15 secondi, lo stato dovrebbe cambiare in 'Arrestato'


## Avvio di CDN
Un CDN può essere avviato solo quando è in stato 'Arrestato'
1. Fai clic su 'Avvia CDN' dal menu di overflow, indicato come tre punti a destra della riga CDN.
 ![Menu di overflow](images/start_cdn.png)

2. Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler avviare il servizio. Seleziona **Conferma** per continuare.

3. Se l'azione è stata eseguita correttamente, viene visualizzata una casella di dialogo nell'angolo superiore destro dello schermo che ti permette di conoscere l'esito e l'ora dell'azione.
  
4. Ciò cambierà lo stato in 'Configurazione CNAME'

5. Fai clic su 'Ottieni stato' dal menu di overflow. Questa azione cambierà lo stato in 'In esecuzione' e il CDN diventerà operativo.

## Eliminazione di CDN

Per eliminare un CDN, completa la seguente procedura:
**NOTA**: selezionando `Elimina` dal menu di overflow si eliminerà solo il CDN e non il tuo account.

1. Fai clic su 'Elimina' dal menu di overflow.

 ![Menu di overflow - Elimina CDN](images/delete_cdn.png)

2. Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler eseguire l'eliminazione. Fai clic su **Elimina** per continuare.

3. Ciò cambierà lo stato in 'In fase di eliminazione'. Fai clic su 'Ottieni stato' dal menu di overflow. Questa azione rimuoverà la riga dall'elenco CDN.

## Impostazione del tempo di memorizzazione dei contenuti nella cache utilizzando "Time To Live"

Una volta che il tuo CDN è in esecuzione, puoi impostare il tempo di memorizzazione dei contenuti nella cache utilizzando Time To Live (TTL). Il Time To Live per un determinato percorso di file o directory indica per quanto tempo i contenuti debbano restare memorizzati nella cache. Quando hai creato l'Associazione CDN, è stato creato un TTL globale predefinito di 3600 secondi. 

1. Nella pagina CDN, seleziona il tuo CDN che ti porterà alla pagina **Panoramica**.

2. Puoi modificare il tempo utilizzando le frecce o immettendo un nuovo valore. Il valore di tempo è specificato in secondi. Ad esempio, 3600 secondi equivale a 1 ora. Il valore minimo per `timeToLive` è 30 secondi, mentre il valore massimo è 21474836471 secondi. Seleziona il pulsante **Salva** per impostare il tempo di memorizzazione dei contenuti nella cache. 

  ![Aggiunta di ttl](images/adding-path.png)

3. Dopo aver salvato, puoi **Modificare** o **Eliminare** l'impostazione TTL utilizzando le opzioni del menu di overflow. (**NOTA**: non è possibile modificare il percorso per TTL. Se il percorso associazione viene modificato, il percorso TTL verrà aggiornato automaticamente.)

  ![Modifica o elimina ttl](images/edit-delete-ttl-setting.png)  
	
  * Se il contenuto corrisponde a più regole, avrà la precedenza l'ultima configurazione aggiunta.
  
  * I valori TTL possono essere impostati solo per uno specifico nome file o directory. Le espressioni regolari non sono supportate perché potrebbero creare comportamenti imprevedibili.
	
## Aggiunta dei dettagli del percorso di origine

Quando il tuo CDN è in stato *Configurazione_CNAME* o *In esecuzione*, puoi aggiungere i dettagli del percorso di origine. Puoi scegliere di fornire il contenuto da più server di origine. Ad esempio, le foto possono essere fornite da un server diverso da quello dei video. L'origine può essere basata su un Server host o su Archiviazione oggetti.

1. Nella pagina CDN, seleziona il tuo CDN che ti porterà alla pagina **Panoramica**.
	
2. Seleziona la scheda **Origini** e fai clic sul pulsante **Aggiungi origine**.
	
3. *Devi* fornire un percorso. Seleziona **Server** o **Archiviazione oggetti**.
	
   ![Origini - Aggiungi origine](images/add-origin.png)
		
  * Se hai selezionato **Server**, immetti l'indirizzo IP del server o il _nome host_. Fornisci una porta HTTP, una porta HTTPS o entrambe, a seconda del protocollo che hai selezionato durante la creazione di CDN; deve corrispondere a quella dell'associazione.
	
  ![Aggiungi origine - Server](images/add-origin-server.png)
	
  * Se hai selezionato **Archiviazione oggetti**, fornisci l'endpoint, il nome bucket e la porta HTTPS. Facoltativamente, specifica le estensioni file che possono essere utilizzate nel servizio CDN. Se non si specifica nulla, sono consentite tutte le estensioni file.
	
  ![Aggiungi origine - Archiviazione oggetti](images/add-origin-object-storage.png)
		
4. Seleziona il pulsante **Aggiungi** per aggiungere il tuo percorso di origine.

  **Nota**: quando fornisci le estensioni file per un percorso di origine Archiviazione oggetti, l'impostazione TTL che ha lo stesso URL del percorso di origine sarà definita per includere tutti i file che hanno tali estensioni file specificate. Ad esempio, se crei un percorso di origine "/example" e specifichi l'estensione file "jpg png gif", il valore TTL del percorso TTL "/example" avrà un ambito che include tutti i file JPG/PNG/GIF nella directory "/example" e nelle sue sotto-directory.

5. Dopo l'aggiunta, puoi **Modificare** o **Eliminare** l'origine utilizzando le opzioni del menu di overflow.

  ![Modifica o elimina origine](images/edit-delete-origin.png)
	
## Eliminazione del contenuto memorizzato nella cache

Una volta che il tuo CDN è in esecuzione, puoi eliminare il contenuto memorizzato nella cache dal server del fornitore.  

1. Nella pagina CDN, seleziona il tuo CDN che ti porterà alla pagina **Panoramica**.
	
2. Seleziona la scheda **Elimina**.

	![Pagina Elimina](images/purge_tab.png)
	
3. Immetti la sintassi del percorso unix standard per indicare quale file vuoi eliminare, quindi seleziona il pulsante **Elimina**. Al momento, l'eliminazione è consentita solo per un singolo file.

4. Dopo l'eliminazione, l'attività viene elencata in **Attività di eliminazione**. Puoi selezionare **Ripeti eliminazione** o **Favorito** per il percorso utilizzando le opzioni del menu di overflow.

	![Attività di eliminazione](images/purge-activity.png)
	
   **NOTA:** se ci sono più di 15 eliminazioni, l'attività di eliminazione viene troncata automaticamente ogni 15 giorni.
	
## Aggiornamento dei dettagli della configurazione CDN

Una volta che il tuo CDN è in esecuzione, puoi aggiornare i dettagli della configurazione CDN.

1. Nella pagina CDN, seleziona il tuo CDN che ti porterà alla pagina **Panoramica**.

2. Seleziona la scheda **Impostazioni**. Vengono visualizzati i dettagli della tua configurazione CDN.

  Per **Archiviazione oggetti**, è possibile modificare i seguenti campi: 
   * Endpoint
   * Nome bucket
   * Porta HTTPS
   * Estensioni file consentite
   * Utilizza contenuto obsoleto
   * Rispetta intestazioni

  Per **Server**, è possibile modificare i seguenti campi: 
   * Indirizzo server di origine
   * Porta HTTP/HTTPS
   * Utilizza contenuto obsoleto
   * Rispetta intestazioni

3. Aggiorna i dettagli **Origine** o **Altre opzioni** laddove necessario, quindi fai clic sul pulsante **Salva** nell'angolo inferiore destro per aggiornare i dettagli della tua configurazione CDN.

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
