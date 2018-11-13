---

copyright:
  years: 2018
lastupdated: "2018-10-11"

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

 ![Schermata elenco di associazioni](images/mapping-list.png)


Se hai ordinato la tua CDN con HTTP o HTTPS con il certificato jolly, puoi procedere al passo 1.

Se hai creato una CDN con HTTPS con il certificato SAN DV, potrebbero essere necessari ulteriori passi per verificare il tuo dominio; questi passi sono descritti nella pagina [Completamento della convalida del controllo del dominio per HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

**Passo 1:**

Dopo che hai ordinato una CDN, dovrai configurare il **CNAME** con il tuo provider DNS. La maggior parte dei provider DNS può fornirti istruzioni sull'impostazione o sulla modifica del CNAME.

   * Durante questo periodo, lo stato della tua CDN verrà mostrato come **Configurazione CNAME**. Contatta il tuo provider DNS per scoprire quando le modifiche diventeranno attive.

   ![Configurazione CNAME](images/cname-config.png)  

**Passo 2:**

Dopo aver configurato il CNAME con il tuo provider DNS, puoi controllare lo stato in qualsiasi momento selezionando **Ottieni stato** dal menu Overflow alla destra dello stato CDN.

  ![CNAME getStatus](images/cname-getstatus.png)  

**Passo 3:**

Una volta completato il concatenamento CNAME, la selezione di **Ottieni stato** modificherà lo stato in *RUNNING* e la CDN è pronta per l'uso.

Complimenti! La tua CDN è ora in esecuzione. Da qui, la pagina [Gestisci la tua CDN](how-to.html#manage-your-cdn) include ulteriori informazioni sulle opzioni di configurazione, come [TTL (Time-to-live)](how-to.html#setting-content-caching-time-using-time-to-live-), [Eliminazione del contenuto memorizzato nella cache](how-to.html#purging-cached-content) e [Aggiunta dei dettagli del percorso di origine](how-to.html#adding-origin-path-details).

## Avvio di CDN

L'avvio della CDN informa il DNS di indirizzare il traffico dalla tua origine al server edge Akamai. Dopo che l'associazione è stata avviata, la cache del DNS potrebbe ancora indirizzare il traffico all'origine e quindi la funzionalità potrebbe non essere vista dal dominio immediatamente dopo l'avvio dell'associazione. Il tempo necessario per l'aggiornamento dipende dalla frequenza con cui viene aggiornata la cache DNS e varia a seconda del tuo provider DNS.

**NOTA**: una CDN può essere avviata solo quando è nello stato `Stopped` (Arrestato)  

**Passo 1:**

Fai clic su **Start CDN** dal menu Overflow, indicato come tre punti a destra della riga della CDN.

  ![Menu Overflow](images/start_cdn.png)

**Passo 2:**

Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler avviare il servizio. Seleziona **Confirm** per procedere.

**Passo 3:**

Se l'azione è stata eseguita correttamente, viene visualizzata una casella di dialogo nell'angolo superiore destro dello schermo che ti permette di conoscere l'esito e l'ora dell'azione.

**Passo 4:**

Questo passo modifica lo stato in `CNAME Configuration`

**Passo 5:**

Fai clic su **Get Status** dal menu Overflow. Questo passo modifica lo stato in `Running`. La tua CDN diventa operativa.

## Arresto della CDN

Una volta arrestata l'associazione, la ricerca DNS viene passata all'origine. Il traffico ignora i server edge CDN e il contenuto viene recuperato direttamente dall'origine. Dopo che un'associazione è stata arrestata, ci può essere un breve periodo di tempo in cui il tuo contenuto non è accessibile. Ciò è dovuto al fato che la cache DNS potrebbe continuare a indirizzare il traffico ai server edge Akamai. Tuttavia, durante questo lasso di tempo, il server edge Akamai negherà il traffico per il dominio. La durata di questo periodo dipende dalla frequenza con cui viene aggiornata la cache DNS e varia a seconda del tuo provider DNS.

**NOTE**: 
* L'arresto della CDN **NON** è consigliato per una CDN configurata con un certificato SAN HTTPS poiché il traffico HTTPS potrebbe non funzionare quando passi la CDN nuovamente allo stato `Running` (In esecuzione). 
* Una CDN può essere arrestata solo quando è in uno stato di `Running` (In esecuzione).

**Passo 1:**

Fai clic su 'Stop CDN' dal menu Overflow (3 punti verticali a destra dello stato CDN).
 ![Menu Overflow](images/stop_cdn.png)

**Passo 2:**

Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare l'arresto del servizio. Seleziona **Confirm** per procedere.

**Passo 3:**

Entro circa 5 - 15 secondi, lo stato dovrebbe cambiare in 'Stopped' (Arrestato)

## Eliminazione di CDN

Per eliminare una CDN, attieniti a questa procedura:

**NOTA**: la selezione di `Delete` dal menu Overflow elimina solo la CDN; non elimina il tuo account.

**Passo 1:**

Fai clic su 'Delete' dal menu Overflow.

 ![Menu Overflow - Elimina CDN](images/delete_cdn.png)

**Passo 2:**

Viene visualizzata una finestra di dialogo più grande che ti chiede di confermare di voler eseguire l'eliminazione. Fai clic su **Delete** per procedere.

**NOTA**: se la tua CDN è configurata mediante HTTPS con il certificato SAN DV, potrebbero essere necessarie fino a 5 ore per completare il processo di eliminazione.

  ![Elimina con avvertenza](images/delete-with-warning.png)

**Passo 3:**

Dopo aver completato i passi 1 e 2, lo stato della tua CDN sarà `Deleting`. Una volta completato il processo di eliminazione, fai nuovamente clic su 'Get Status' dal menu Overflow per rimuovere la riga dall'elenco di CDN. Se il processo di eliminazione non è stato completato, questa azione non avrà alcun effetto.
