---

copyright:
  years: 2018
lastupdated: "2018-06-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Passaggio allo stato di esecuzione per la tua CDN

Scopri come portare la tua CDN in uno stato di ESECUZIONE seguendo queste linee guida. Questo documento ti indica anche come avviare e arrestare la tua CDN.

## Passa allo stato di esecuzione

Una volta che hai creato una CDN, essa viene visualizzata nel tuo dashboard CDN. Qui puoi visualizzare il nome della CDN, l'origine, il provider e lo stato.  

 ![Schermata elenco di associazioni](images/mapping_list_cname.png)


Se hai ordinato la tua CDN con HTTP o HTTPS con il certificato wildcard, puoi procedere al passo 1.

Se hai creato una CDN con HTTPS con il certificato SAN DV, potrebbero essere necessari ulteriori passi per verificare il tuo dominio; questi passi sono descritti nella pagina [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

**Passo 1:**

Dopo che hai ordinato una CDN, dovrai configurare il **CNAME** con il tuo provider DNS. La maggior parte dei provider DNS può fornirti istruzioni sull'impostazione o sulla modifica del CNAME.

   * Durante questo periodo, lo stato della tua CDN verrà mostrato come **Configurazione CNAME**. Contatta il tuo provider DNS per scoprire quando le modifiche diventeranno attive.

   ![Configurazione CNAME](images/cname-config.png)  

**Passo 2:**

Dopo aver configurato il CNAME con il tuo provider DNS, puoi controllare lo stato in qualsiasi momento selezionando **Ottieni stato** dal menu di overflow alla destra dello stato CDN.

  ![CNAME getStatus](images/cname-getstatus.png)  

**Passo 3:**

Una volta completato il concatenamento CNAME, la selezione di **Ottieni stato** modificherà lo stato in *RUNNING* e la CDN è pronta per l'uso.

Complimenti! La tua CDN è ora in esecuzione. Da qui, la pagina [Gestisci la tua CDN](how-to.html#manage-your-CDN) include ulteriori informazioni sulle opzioni di configurazione, come [TTL (Time-to-live)](how-to.html#setting-content-caching-time-using-time-to-live-), [Eliminazione del contenuto memorizzato nella cache](how-to.html#purging-cached-content) e [Aggiunta dei dettagli del percorso di origine](how-to.html#adding-origin-path-details).

## Avvio di CDN

Una CDN può essere avviata solo quando è in stato 'Arrestato'  

**Passo 1:**

Fai clic su 'Avvia CDN' dal menu di overflow, indicato come tre punti a destra della riga CDN.

  ![Menu di overflow](images/start_cdn.png)

**Passo 2:**

Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler avviare il servizio. Seleziona **Conferma** per continuare.

**Passo 3:**

Se l'azione è stata eseguita correttamente, viene visualizzata una casella di dialogo nell'angolo superiore destro dello schermo che ti permette di conoscere l'esito e l'ora dell'azione.

**Passo 4:**

Questo passo cambia lo stato in 'Configurazione CNAME'

**Passo 5:**

Fai clic su 'Ottieni stato' dal menu di overflow. Questo passo cambia lo stato in 'In esecuzione' La tua CDN diventa operativa.

## Arresto della CDN

Una CDN può essere arrestata solo quando è in stato 'In esecuzione'.

**Passo 1:**

Fai clic su 'Arresta CDN' dal menu di overflow (3 punti verticali a destra dello stato CDN).
 ![Menu di overflow](images/stop_cdn.png)

**Passo 2:**

Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare l'arresto del servizio. Seleziona **Conferma** per continuare.

**Passo 3:**

Entro circa 5 - 15 secondi, lo stato dovrebbe cambiare in 'Arrestato'

## Eliminazione di CDN

Per eliminare una CDN, attieniti a questa procedura:

**NOTA**: la selezione di `Elimina` dal menu di overflow elimina solo la CDN; non elimina il tuo account.

**Passo 1:**

Fai clic su 'Elimina' dal menu di overflow.

 ![Menu di overflow - Elimina CDN](images/delete_cdn.png)

**Passo 2:**

Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler eseguire l'eliminazione. Fai clic su **Elimina** per procedere.

**NOTA**: se la tua CDN è configurata mediante HTTPS con il certificato SAN DV, potrebbero essere necessarie fino a 5 ore per completare il processo di eliminazione.

  ![Elimina con avvertenza](images/delete-with-warning.png)

**Passo 3:**

Dopo aver completato i passi 1 e 2, lo stato della tua CDN sarà `Deleting`. Al termine del processo di eliminazione, facendo di nuovo clic su 'Ottieni stato' dal menu di overflow si rimuoverà la riga dall'elenco di CDN. Se il processo di eliminazione non è stato completato, questa azione non avrà alcun effetto.
