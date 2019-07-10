---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Completamento della convalida del controllo del dominio per HTTPS con SAN DV
{: #completing-domain-control-validation-for-https-with-dv-san}

Il seguente diagramma descrive i vari stati a cui passerà la tua CDN dal momento in cui viene creata e fino al raggiungimento dello stato di esecuzione (running).

  ![Diagramma degli stati di SAN](images/state-diagram-san.png)

## Passi iniziali per la convalida del controllo del dominio
{: #initial-steps-to-domain-control-validation}

**Passo 1:**

Dopo aver ordinato la tua CDN con un certificato SAN DV, inizia il processo di richiesta del certificato. Durante questo processo, la {{site.data.keyword.cloud}} CDN richiede un certificato da Akamai. Una volta che un certificato diventa disponibile, Akamai invia una richiesta all'Autorità di certificazione (CA).

  * Durante questo tempo, lo stato della CDN viene visualizzato come **Requesting Certificate**.

    ![Stato Requesting Certificate](images/requesting-cert.png)

**Passo 2:**

Quando riceve la richiesta, l'autorità di certificazione (o CA, Certificate Authority) emette una verifica di convalida del dominio.

  * Quando ciò accade, lo stato della tua CDN cambierà in **Domain validation needed**.

    ![Stato Domain Validation Needed](images/domain-validation-needed.png)

**Passo 3:**

Fai clic sul nome della CDN che deve essere convalidata. Si apre la pagina di panoramica, dove puoi vedere lo stato generale della tua CDN. Nella parte superiore della pagina, viene visualizzato un avviso che ti ricorda che è necessaria la convalida del dominio. Seleziona il pulsante **View domain validation** per visualizzare una finestra che ti mostra le informazioni sulla verifica di cui hai bisogno per completare il processo di convalida.

   ![Stato Domain Validation Needed](images/view-domain-validation.png)

**Passo 4:** una volta che hai completato uno dei passi di convalida dalla sezione su come indirizzare una verifica di convalida del dominio, la tua CDN passa allo stato **Deploying certificate**. Durante questo tempo, Akamai distribuisce il tuo certificato convalidato ai relativi server edge. La distribuzione di un certificato può richiedere da 2 a 4 ore.

  * Al termine di questo processo, tutti i domini, indipendentemente dal metodo di convalida utilizzato, passano a uno stato **CNAME Configuration**.

Ulteriori informazioni sul completamento della tua configurazione di CNAME e sulla supervisione della tua CDN sono disponibili nella pagina [Passa allo stato di esecuzione](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running).


## DCV (Domain Control Validation, convalida del controllo del dominio)
{: #domain-control-validation}

Per fare in modo che il tuo nome dominio CDN venga aggiunto al certificato SAN, devi provare che hai il controllo amministrativo sul tuo dominio. Questo processo di prova viene indicato come indirizzamento alla DCV (Domain Control Validation). Devi eseguire l'indirizzamento alla DCV entro 48 ore. In caso contrario, la tua richiesta scade e devi iniziare di nuovo il processo di ordine. I tre diversi modi per eseguire l'indirizzamento alla DCV sono descritti nelle sezioni qui di seguito.


### CNAME
{: #cname}

Questo metodo è consigliato **SOLO** se la tua CDN **non** sta gestendo il traffico in tempo reale. Se il tuo dominio sta gestendo traffico in tempo reale, consigliamo di utilizzare il metodo standard (Standard) o di reindirizzamento (Redirect) per convalidare il tuo dominio.

Per utilizzare questo metodo, aggiungerai un record CNAME per il tuo dominio CDN nella tua configurazione DNS. Il valore CNAME da utilizzare è il CNAME da te utilizzato quando hai creato la CDN. Deve terminare con il dominio `cdnedge.bluemix.net`. Non devi eseguire alcuna altra azione. La DCV continuerà automaticamente da questo punto. La convalida può richiedere da 2 a 4 ore. Una volta distribuito il certificato, la tua CDN passa direttamente allo stato RUNNING (IN ESECUZIONE).

La maggior parte dei provider DNS può fornirti istruzioni sull'impostazione o sulla modifica del CNAME. Ecco un esempio di un tipico record CNAME:

| **Tipo di risorsa** | **Host** | **Punta a (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minuti |


---
### Standard
{: #standard}

Se per la convalida del dominio scegli il metodo Standard, la finestra di convalida del dominio mostra un URL di verifica (**Challenge URL**) e una risposta di verifica (**Challenge response**). Per completare il processo di convalida del dominio, aggiungi la **risposta di verifica** al tuo server di origine. Dopo che è stata aggiunta, la CA può richiamare la **risposta di verifica** dal tuo server di origine utilizzando l'URL specificato nell'**URL di verifica**. Una volta che il tuo server di origine è configurato correttamente, la convalida del dominio può richiedere da 2 a 4 ore.

   ![Verifica di convalida del dominio Standard](images/domain-validation-standard.png)

Per completare correttamente la convalida del dominio tramite il metodo Standard, devi configurare il server di origine in un determinato modo. Di seguito sono descritte le procedure di esempio per i server _Apache(TM)_ e _Nginx(TM)_.

**Situazione di esempio**
* Server di origine: `www.example.com`
* Dominio CDN: `cdn.example.com`
* URL di verifica: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Risposta di verifica: `examplechallenge`

#### Configurazione di Apache
{: #apache-configuration}

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
{: #nginx-configuration}

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
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* Per verificare che questo metodo funzioni tramite `curl`, esegui tale comando per l'URL di verifica.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* Per verificare che questo metodo funzioni tramite un browser, prova ad accedere all'URL di verifica dal tuo browser.

In entrambi i casi, dovresti essere in grado di richiamare la copia dell'oggetto file di verifica della convalida del dominio memorizzata sul tuo server di origine.

#### Pulizia per il metodo Standard
{: #clean-up-for-the-standard-method}

Dopo che la tua CDN ha raggiunto lo stato **Certificate deploying**:
1. Rimuovi il file `examplechallenge-fileobject`. (Facoltativo)
1. Rimuovi, se necessario, il ServerAlias (Apache2) o il server_name (Nginx) aggiunto dalla configurazione del server. (Facoltativo)
1. Rimuovi il record A tra il dominio e l'IP del server di origine.

---
### Reindirizzamento
{: #redirect}

Facendo clic sulla scheda **Redirect**, vengono visualizzate tutte le informazioni necessarie per indirizzare la convalida del dominio tramite reindirizzamento. Queste informazioni consentono alla CA di richiamare una copia della **risposta di verifica** da Akamai attraverso il tuo server di origine. Una volta che il tuo server è configurato correttamente, la convalida del dominio può richiedere da 2 a 4 ore.

   ![Reindirizzamento della verifica di convalida del dominio](images/domain-validation-redirect.png)

Per completare correttamente la convalida del dominio tramite il metodo Redirect, potresti dover configurare il server web in un determinato modo. Nelle sezioni qui di seguito sono descritte le procedure di esempio per i server Apache e Nginx.

**Situazione di esempio**
* Server di origine: `www.example.com`
* Dominio CDN: `cdn.example.com`
* URL di verifica: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* Reindirizzamento URL: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Configurazione del reindirizzamento Apache
{: #apache-redirect-configuration}

  * **Passo 1:** accedi alla macchina su cui è in esecuzione il server Apache2.

  * **Passo 2:** apri il file di configurazione del server Apache2. `/etc/apache2/apache2.conf` e `/etc/apache2/sites-enabled/` sono le ubicazioni predefinite per il file di configurazione.

  * **Passo 3:** aggiungi un'istruzione di reindirizzamento nell'ubicazione appropriata all'interno dei file di configurazione. Se necessario, aggiungi il tuo dominio CDN come ulteriore **ServerAlias** all'host virtuale per la tua origine.

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **Passo 4:** riavvia il server Apache2 con tempi di inattività minimi utilizzando il seguente comando:

    ```
    apachectl -k graceful
    ```

  * **Passo 5:**
crea un record A nel tuo DNS tra il dominio CDN e l'indirizzo IP del server di origine.

#### Configurazione del reindirizzamento Nginx
{: #nginx-redirect-confguration}

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

  * **Passo 4:** riavvia il server Nginx con tempi di inattività minimi utilizzando il seguente comando:

    ```
    nginx -s reload
    ```

  * **Passo 5:**
crea un record A nel tuo DNS tra il dominio CDN e l'indirizzo IP del server di origine.

#### Verifica che il reindirizzamento stia avvenendo
{: #verify-that-the-redirect-is-occurring}

Il completamento di questa procedura reindirizza _solo_ il traffico dall'URL di verifica specificato al reindirizzamento URL. Puoi verificare che il reindirizzamento abbia funzionato correttamente tramite `curl` o tramite il browser.

* Per verificare che il reindirizzamento funzioni tramite `curl`, esegui tale comando per l'URL di verifica.

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* Per verificare che il reindirizzamento funzioni tramite il browser, prova ad raggiungere l'URL di verifica dal tuo browser.

In entrambi i casi, dovresti essere in grado di richiamare la copia dell'oggetto file di verifica della convalida del dominio da Akamai al dominio `dcv.akamai.com`, a cui è stata reindirizzata la richiesta originale.

#### Pulizia per il metodo Redirect
{: #clean-up-for-the-redirect-method}

Dopo che la tua CDN ha raggiunto lo stato **Certificate deploying**:
1. Rimuovi le istruzioni di reindirizzamento o i blocchi dal file di configurazione. (Facoltativo)
1. Rimuovi, se necessario, il ServerAlias (Apache2) o il server_name (Nginx) aggiunto dalla configurazione del server. (Facoltativo)
1. Rimuovi il record A tra il dominio e l'IP del server di origine.
