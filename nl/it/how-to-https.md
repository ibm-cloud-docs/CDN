---

copyright:
  years: 2018
lastupdated: "2018-06-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Completamento della convalida del controllo del dominio per HTTPS

## Passi iniziali per la convalida del controllo del dominio

**Passo 1:**

Dopo aver ordinato la tua CDN con un certificato SAN DV, inizia il processo di richiesta del certificato. Durante questo processo, la CDN IBM Cloud richiede un certificato da Akamai. Una volta che un certificato diventa disponibile, Akamai invia una richiesta all'Autorità di certificazione (CA).

  * Durante questo tempo, lo stato della CDN verrà visualizzato come **Requesting Certificate**.

    ![Stato Requesting Certificate](images/requesting-cert.png)

**Passo 2:**

Una volta che la CA riceve la richiesta, emetterà una verifica di convalida del dominio.

  * Quando ciò accade, lo stato della tua CDN cambierà in **Domain validation needed**.

    ![Stato Domain Validation Needed](images/domain-validation-needed.png)

**Passo 3:**

Fai clic sul nome della CDN che deve essere convalidata. Si apre la pagina di panoramica, dove puoi vedere lo stato generale della tua CDN. Nella parte superiore della pagina c'è un avviso che ti ricorda che è necessaria la convalida del dominio. Seleziona il pulsante **View domain validation** per aprire una finestra che mostra le informazioni sulla verifica necessarie per completare il processo di convalida.

   ![Stato Domain Validation Needed](images/view-domain-validation.png)

**Passo 4:** una volta completato uno dei passi di convalida dalla sezione su come indirizzare una verifica di convalida del dominio, la tua CDN passa allo stato **Deploying certificate**. Durante questo tempo, Akamai distribuirà il tuo certificato convalidato ai relativi server edge. La distribuzione di un certificato può richiedere da 2 a 4 ore.

  * Al termine di questo processo, tutti i domini, indipendentemente dal metodo di convalida utilizzato, passano a uno stato **CNAME Configuration**.

Ulteriori informazioni sul completamento della configurazione di CNAME e sulla supervisione della tua CDN sono disponibili nella pagina [Passaggio allo stato di esecuzione](basic-functions.html#get-to-running).


## Verifica di convalida del dominio

La verifica di convalida del dominio dimostra che sei il proprietario del dominio. Di seguito sono riportati tre modi in cui puoi indirizzare la verifica della convalida del dominio.

**NOTA**: se non rispondi alla richiesta di convalida del dominio entro 48 ore, la tua richiesta scadrà e dovrai ricominciare il processo di ordine.

### CNAME

Se il tuo record CNAME è stato aggiunto al provider DNS prima di ordinare la tua CDN, non devi fare nient'altro. La convalida del dominio viene gestita automaticamente da IBM Cloud, Akamai e dalla CA. La convalida può richiedere da 2 a 4 ore.

  * Se non hai ancora configurato il tuo CNAME con il tuo provider DNS, devi farlo in questo momento. La maggior parte dei provider DNS può fornirti istruzioni sull'impostazione o sulla modifica del CNAME.

   ![CNAME per la convalida del dominio](images/domain-validation-cname.png)

**NOTA**: questo metodo è consigliato **SOLO** se la tua CDN **non** pubblica il traffico in tempo reale. Se il tuo dominio pubblica traffico in tempo reale, si consiglia di utilizzare il metodo Standard per convalidare il tuo dominio.

---
### Standard

Se per la convalida del dominio scegli il metodo Standard, la finestra di convalida dominio mostra un URL di verifica (**Challenge URL**) e una risposta di verifica (**Challenge response**). Per completare il processo di convalida del dominio, devi aggiungere la risposta di verifica fornita in **Challenge response** al tuo server di origine. Ciò consente alla CA di richiamare la **risposta di verifica** dal tuo server di origine utilizzando l'URL specificato nell'**URL di verifica**. Una volta che il tuo server di origine è configurato correttamente, la convalida del dominio può richiedere da 2 a 4 ore.

   ![Verifica di convalida del dominio Standard](images/domain-validation-standard.png)

Per completare correttamente la convalida del dominio tramite il metodo Standard, devi configurare il tuo server di origine in un determinato modo. Di seguito sono descritte le procedure di esempio per i server Apache e Nginx.

**Situazione di esempio**
* Server di origine: `www.example.com`
* Dominio CDN: `cdn.example.com`
* URL di verifica: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Risposta di verifica: `examplechallenge`

#### Configurazione di Apache

  * **Passo 1:** accedi alla macchina su cui è in esecuzione il server Apache2.

  * **Passo 2:** crea il file di risposta di verifica per la risposta alla richiesta di verifica in `.well-known/acme-challenge/` nella directory relativa al contenuto del tuo sito web.  L'ubicazione predefinita per il contenuto del sito web Apache2 è `/var/www/html/`. Per questo esempio, la risposta di verifica verrà posizionata nella directory `/var/www/html/.well-known/acme-challenge/`.

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Passo 3:** se necessario, apri il file di configurazione del server Apache2. `/etc/apache2/apache2.conf` e `/etc/apache2/sites-enabled/` sono le ubicazioni predefinite per i file di configurazione.

  * **Passo 4:** se necessario, aggiungi il tuo dominio CDN come ulteriore **ServerAlias** all'host virtuale per la tua origine.

  * **Passo 5:** se hai dovuto modificare la configurazione del tuo server Apache2, riavvia il server Apache2 con tempi di inattività minimi utilizzando il seguente comando:

      ```
      apachectl -k graceful
      ```

  * **Passo 6:** crea un record A nel tuo DNS tra il dominio CDN e l'indirizzo IP del server di origine.

#### Configurazione di Nginx

  * **Passo 1:** accedi alla macchina su cui è in esecuzione il server Nginx.

  * **Passo 2:** crea il file di risposta di verifica per la risposta alla richiesta di verifica in `.well-known/acme-challenge/` nella directory relativa al contenuto del tuo sito web.  L'ubicazione predefinita per il contenuto del sito web Nginx è `/usr/share/nginx/html/`.  Per questo esempio, la risposta di verifica verrà posizionata nella directory `/usr/share/nginx/html/.well-known/acme-challenge/`.
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **Passo 3:** se necessario, apri il file di configurazione del server Nginx. `/etc/nginx/nginx.conf` e `/etc/nginx/conf.d/` sono le ubicazioni predefinite per i file di configurazione.

  * **Passo 4:** se necessario, aggiungi il tuo dominio CDN come ulteriore **server_name** al blocco di server per la tua origine.

  * **Passo 5:** se hai dovuto modificare la configurazione del tuo server Nginx, riavvia il server Nginx con tempi di inattività minimi utilizzando il seguente comando:

      ```
      nginx -s reload
      ```

  * **Passo 6:** crea un record A nel tuo DNS tra il dominio CDN e l'indirizzo IP del server di origine.

#### Verifica che questo metodo standard per indirizzare la convalida del dominio sia pronto per la CA.

* Per verificare che questo metodo funzioni tramite `curl`, esegui tale comando per l'URL di verifica.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Per verificare che questo metodo funzioni tramite un browser, prova ad accedere all'URL di verifica dal tuo browser.

In entrambi i casi, dovresti essere in grado di richiamare la copia dell'oggetto file di verifica della convalida del dominio memorizzata sul tuo server di origine.

#### Pulizia per il metodo Standard

Dopo che la tua CDN ha raggiunto lo stato **Certificate deploying**:
1. Rimuovi il file `examplechallenge-fileobject`. (Facoltativo)
1. Rimuovi, se necessario, il ServerAlias (Apache2) o il server_name (Nginx) aggiunto dalla configurazione del server. (Facoltativo)
1. Rimuovi il record A tra il dominio e l'IP del server di origine.

---
### Reindirizzamento

Facendo clic sulla scheda **Redirect**, vengono visualizzate tutte le informazioni necessarie per indirizzare la convalida del dominio tramite reindirizzamento. Queste informazioni consentono alla CA di richiamare una copia della **risposta di verifica** da Akamai attraverso il tuo server di origine. Una volta che il tuo server è configurato correttamente, la convalida del dominio può richiedere da 2 a 4 ore.

   ![Reindirizzamento della verifica di convalida del dominio](images/domain-validation-redirect.png)

Per completare correttamente la convalida del dominio tramite il metodo Redirect, potresti dover configurare il server web in un determinato modo. Di seguito sono descritte le procedure di esempio per i server Apache e Nginx.

**Situazione di esempio**
* Server di origine: `www.example.com`
* Dominio CDN: `cdn.example.com`
* URL di verifica: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Reindirizzamento URL: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Configurazione del reindirizzamento Apache

  * **Passo 1:** accedi alla macchina su cui è in esecuzione il server Apache2.

  * **Passo 2:** apri il file di configurazione del server Apache2. `/etc/apache2/apache2.conf` e `/etc/apache2/sites-enabled/` sono le ubicazioni predefinite per i file di configurazione.

  * **Passo 3:** aggiungi un'istruzione di reindirizzamento nell'ubicazione appropriata all'interno dei file di configurazione. Se necessario, aggiungi il tuo dominio CDN come ulteriore **ServerAlias** all'host virtuale per la tua origine.
    ```
    Redirect http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Passo 4:** riavvia il server Apache2 con tempi di inattività minimi utilizzando il seguente comando:

    ```
    apachectl -k graceful
    ```

  * **Passo 5:**
crea un record A nel tuo DNS tra il dominio CDN e l'indirizzo IP del server di origine.

#### Configurazione del reindirizzamento Nginx

  * **Passo 1:** accedi alla macchina su cui è in esecuzione il server Nginx.

  * **Passo 2:** apri il file di configurazione del server Nginx. `/etc/nginx/nginx.conf` e `/etc/nginx/conf.d/` sono le ubicazioni predefinite per i file di configurazione.

  * **Passo 3:** esistono due metodi ugualmente validi per questo passo.

    * Opzione 1 (consigliata): aggiungi un blocco `location` con una direttiva `return` per eseguire il reindirizzamento nel blocco di `server` appropriato. Se necessario, aggiungi il tuo dominio CDN come ulteriore **server_name** al blocco di server per la tua origine.

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives
      # ...
    }
    ```

   * Opzione 2: aggiungi una direttiva `rewrite` all'interno del blocco di `server`. Se necessario, aggiungi il tuo dominio CDN come ulteriore **server_name** al blocco di server per la tua origine.

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives
      # ...
    }
    ```

  * **Passo 4:** riavvia il server Apache2 con tempi di inattività minimi utilizzando il seguente comando:

    ```
    nginx -s reload
    ```

  * **Passo 5:** crea un record A nel tuo DNS tra il dominio CDN e l'IP del server di origine.

#### Verifica che il reindirizzamento stia avvenendo

Il completamento di questi passi reindirizzerà solo il traffico per lo specifico URL di verifica al reindirizzamento URL. Puoi verificare che il reindirizzamento abbia funzionato correttamente tramite `curl` o il browser.

* Per verificare che il reindirizzamento funzioni tramite `curl`, esegui tale comando per l'URL di verifica.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Per verificare che il reindirizzamento funzioni tramite il browser, prova ad accedere all'URL di verifica dal tuo browser.

In entrambi i casi, dovresti essere in grado di richiamare la copia dell'oggetto file di verifica della convalida del dominio da Akamai nel dominio dcv.akamai.com, a cui è stata reindirizzata la richiesta originale.

#### Pulizia per il metodo Redirect

Dopo che la tua CDN ha raggiunto lo stato **Certificate deploying**:
1. Rimuovi le istruzioni di reindirizzamento o i blocchi dal file di configurazione. (Facoltativo)
1. Rimuovi, se necessario, il ServerAlias (Apache2) o il server_name (Nginx) aggiunto dalla configurazione del server. (Facoltativo)
1. Rimuovi il record A tra il dominio e l'IP del server di origine.
