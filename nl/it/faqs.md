---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Domande frequenti

## Cos'è una CDN (Content Delivery Network)?

Una CDN (Content Delivery Network) è una raccolta di server edge distribuiti in varie parti del paese o del mondo. Il contenuto web viene fornito da un server edge che si trova nella zona geografica più vicina al cliente che richiede il contenuto. Questa tecnica consente agli utenti di ricevere il contenuto con meno ritardi di quanto possiamo ottenere consegnando il contenuto da una posizione centralizzata. Fornisce un'esperienza complessiva migliore per i tuoi clienti.

## Come funziona una CDN (Content Delivery Network)?

Una CDN raggiunge il suo scopo memorizzando in cache il contenuto web sui server edge in tutto il mondo. Quando un utente richiede dei contenuti web, la richiesta di contenuto viene indirizzata al server edge geograficamente più vicino a tale utente. Riducendo la distanza che il contenuto deve percorrere, il CDN offre un rendimento ottimizzato, una latenza ridotta e prestazioni più elevate. 

## Come viene creato il mio account di servizio CDN (Content Delivery Network) IBM Cloud?

Il tuo account viene creato durante il processo di ordine della CDN, quando fai clic su **Seleziona** dopo aver esplorato il menu "Selezione venditore".

## Cosa devo fare quando la mia CDN è in stato CNAME_CONFIGURATION?

Per la CDN basata su HTTP, aggiorna il tuo record DNS in modo che il tuo sito web punti al `CNAME` associato alla tua nuova associazione CDN. Per la CDN basata su HTTPS, questo aggiornamento del DNS **NON** è necessario.

**Nota**: l'attivazione dell'aggiornamento potrebbe richiedere fino a 15-30 minuti. Contatta il tuo provider DNS per ottenere una stima di tempo accurata.

## Che cosa mi verrà addebitato?

Ti viene addebitata solo la larghezza di banda utilizzata per l'istanza CDN (Content Delivery Network) IBM Cloud. Se il tuo CDN non utilizza alcuna larghezza di banda, non ti verranno addebitate spese. I prezzi della larghezza di banda variano a seconda della posizione regionale del server edge. Puoi vedere i prezzi per regione geografica nella sezione [Introduzione](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd) per questo servizio.

## Quando riceverò l'addebito?

La fatturazione di CDN (Content Delivery Netwok) IBM Cloud avviene in base al periodo di fatturazione stabilito nel tuo account {{site.data.keyword.BluSoftlayer_notm}}.

## Como posso visualizzare le metriche e l'utilizzo?

Puoi visualizzare le metriche e l'utilizzo nella pagina **Panoramica**, che può essere raggiunta selezionando la tua CDN dalla pagina **Content Delivery Networks**. **NOTA**: dopo che hai creato la tua CDN, potrebbero essere necessarie fino a 48 ore perché le metriche vengano visualizzate.

## Ho creato una CDN e c'era traffico di dati attraverso essa. Perché le mie metriche non vengono visualizzate?

Dopo che hai creato una CDN, ci vogliono 48 prima che le metriche vengano visualizzate.

## C'è un numero minimo di giorni per cui posso visualizzare le metriche? Esiste un massimo?

C'è un numero minimo e uno massimo di giorni per cui puoi visualizzare le metriche utilizzando la CDN (Content Delivery Network) IBM Cloud con Akamai. Le metriche possono essere raccolte per un minimo di 7 giorni e possono essere visualizzate per un massimo di 90 giorni. Per coloro che utilizzano l'API, si consiglia di utilizzare 89 giorni come massimo, per tenere conto delle differenze di fuso orario.

## Perché la percentuale di riscontri non è zero quando il totale di riscontri è zero?
La percentuale di riscontri rappresenta la percentuale di volte in cui il contenuto è stato distribuito dalla cache del server edge, piuttosto che dal server di origine. Viene calcolata nel seguente modo:

```
((Edge hits - Ingress hits)/Edge hits) * 100

dove:
I riscontri edge sono definiti come "Tutti i riscontri ai server edge dagli utenti finali"
I riscontri Ingress sono definiti come "I riscontri di origine o Ingress per il traffico dalla tua origine ai server edge Akamai"
```
 
Poiché la percentuale di riscontri viene calcolata a livello dell'account e non per CDN, essa sarà uguale per tutte le CDN nel tuo account. Questo spiega inoltre perché la percentuale di riscontri può essere diversa da zero quando il numero di riscontri edge per una CDN particolare è zero.

## Se seleziono `delete` dal menu di overflow CDN, viene eliminato il mio account?

No; verrà eliminato solo quel CDN. Il tuo account esiste ancora e puoi creare altri CDN.

## La memorizzazione nella cache utilizza il push o il pull?

La memorizzazione del contenuto nella cache viene effettuata utilizzando il modello _pull di origine_. Il pull di origine è un metodo con cui i dati vengono "estratti" dal server edge dall'interno del server di origine, a differenza del caricamento manuale del contenuto sul server edge. 

## Esiste un valore massimo per Time To Live? Un minimo?

Il valore massimo per Time To Live è 2.147.483.647 secondi, che equivale a circa 68 anni! Il valore minimo è 30 secondi.

## Esiste un limite sul numero di voci di origine e TTL?

Sì, il limite combinato è di 75 voci per ogni CDN.

## Esiste un limite sul numero di CDN che posso avere?

Sì, puoi avere un limite di 10 CDN per account.

## C'è un browser consigliato da utilizzare per la configurazione del servizio CDN?

Sì, Firefox e Chrome sono i browser consigliati. Ti consigliamo di utilizzare le loro versioni più recenti con la tua CDN (Content Delivery Network) IBM Cloud.

## Qual è lo scopo di fornire un percorso quando creo la mia CDN?

Fornire un percorso mentre crei la tua CDN ti consente di isolare i file che possono essere forniti tramite la CDN da uno specifico server di origine.

## Come faccio a sapere se la mia CDN funziona?
Esegui il seguente comando 'curl' sostituendo "origin.cdntesting.net/assets/ibm_3d.gif" con il percorso file rispettivo sulla tua origine: 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
Se l'output del comando corrisponde al seguente formato, il CDN funziona nel modo previsto: 
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## Il mio CDN è in uno stato di errore. Cosa faccio adesso?
Consulta la pagina ['Come ottenere aiuto e supporto'](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp) o apri un ticket nel [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/).

## Dove trovo il mio CNAME se non ne ho fornito uno? 
Fai clic sulla CDN per accedere alla pagina **Panoramica** nel portale. Nell'angolo destro puoi vedere una sezione **Dettagli** con le informazioni sul `CName`.

## Esiste un limite sul numero di richieste di eliminazione che possono essere attive contemporaneamente?
Sì. Ci possono essere solo 5 richieste di eliminazione attive alla volta.

## La mia richiesta di eliminazione per un determinato percorso file è in corso. Posso inviare una nuova richiesta per lo stesso percorso file?
No. Ci può essere una sola richiesta di eliminazione attiva per un determinato percorso di file alla volta.

## IPv6 (Internet Protocol version 6) è supportato con il servizio CDN (Content Delivery Netwok) IBM Cloud? Come funziona?
IPv6 (o supporto dual stack) è supportato dai server edge di Akamai. È progettato per aiutare i clienti solo con origine IPv4 ad accettare le connessioni dai client IPv6, eseguire la conversione da IPv6 e IPv4 ai margini e andare avanti all'origine con IPv4.

**NOTA:** la creazione di una CDN IBM Cloud utilizzando un indirizzo IPv6 come indirizzo del server di origine non è supportata.

## Quali sono le regole per il nome host?
La stringa di input del `Nome host` **deve**:
  * consistere in caratteri alfanumerici
  * essere inferiore ai 254 caratteri
  * terminare con un nome dominio di livello superiore valido
  * **non** deve terminare con 'cdnedge.bluemix.net` (tale termine viene utilizzato per i CNAME ed è riservato)

Consulta RFC 1035, sezione 2.3.4 per ulteriori dettagli.

## Quali sono le convenzioni di denominazione CNAME personalizzate?
La stringa di input `CNAME` deve rispettare le seguenti regole:
  * **deve** essere univoca (non può essere utilizzata da altre CDN IBM Cloud)
  * inferiore ai 64 caratteri alfanumerici
  * i caratteri speciali `! @ # $ % ^ & *` **non** sono consentiti
  * i caratteri di sottolineatura (`_`) **non** sono consentiti
  * i punti (`.`) **non** sono consentiti
  * i trattini `-` sono consentiti ma CNAME non può iniziare o terminare con un trattino

## Quali sono le regole per i nomi bucket?
I nomi bucket:
  * deve essere di almeno 1 carattere
  * non può essere più lungo di 199 caratteri
  * può contenere lettere (sono consentite sia lettere maiuscole che minuscole), numeri e trattini
  * **non** devono avere il formato di un indirizzo IP (ad esempio, 127.0.0.1)
  * **non può** iniziare con un punto (.)
  * può terminare con un punto (.)
  * Solo un punto (.) è consentito tra le etichette (ad esempio, my..ibmcloud.bucket non è consentito). 

**NOTA**: anche se lettere maiuscole e punti possono superare la convalida, consigliamo di utilizzare sempre nomi di bucket conformi a DNS.

## Quali sono le regole per la stringa percorso per l'origine?
Il percorso è facoltativo quando crei la tua CDN. Tuttavia, se fornito, il percorso **deve**:
  * avere una lunghezza inferiore ai 1000 caratteri
  * iniziare con '/'

## Per il comando **Aggiungi origine**, esistono delle regole da seguire per la stringa del percorso?
Per **Aggiungi origine**, il percorso è obbligatorio. Anche se la tua CDN è stata creata con un percorso, il percorso per **Aggiungi origine** deve iniziare con il percorso della CDN come prefisso. Ad esempio, se il percorso della CDN è stato specificato come `/storage`, per richiamare **Aggiungi origine** con un percorso denominato `/examplePath`, il percorso fornito deve essere `/storage/examplePath`'.

## In quale formato devo fornire le estensioni file per creare un tipo di origine Object Storage con questo servizio CDN?

Le estensioni file devono essere separate da virgole. Ad esempio, l'elenco "txt, jpg, xml" è valido. Tutte le estensioni particolari non possono superare i 10 caratteri.

## Esistono delle limitazioni sui numeri porta HTTP e HTTPS consentiti per Akamai?
Sì. Per il fornitore Akamai, sono consentiti solo i seguenti numeri porta:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 e 45002.

## Quale URL deve essere utilizzato per accedere ai dati nel CDN o nel percorso di origine? 
Il percorso di un'associazione CDN o per l'origine viene trattato come una directory. Quindi, gli utenti che tentano di accedere al percorso di origine devono accedervi come a una directory (con una barra). Ad esempio, se viene creato il CDN `www.example.com` utilizzando il percorso che include la directory `/images`, l'URL per raggiungerlo deve essere `www.example.com/images/`

L'omissione della barra, ad esempio, utilizzando `www.example.com/images` provocherà un errore **Pagina non trovata**.

## Per HTTPS, perché non mi posso collegare tramite un comando curl o un browser utilizzando il nome host?

Al momento HTTPS è supportato solo tramite un certificato wildcard. A causa di questa limitazione, la connessione deve essere effettuata utilizzando il CNAME; il tentare di collegarsi utilizzando il nome host provocherà un errore.

## Qual è la modalità di funzionamento prevista quando viene caricato il CNAME o il nome host sul tuo browser per i protocolli CDN supportati?

|URL del browser| CDN solo con il protocollo HTTP | CDN solo con il protocollo HTTPS | CDN con entrambi i protocolli HTTP e HTTPS |
|-------|-----|-----|-----|
|http://nomehost| Caricamento riuscito | 301 Spostato definitivamente | 301 Spostato definitivamente |
|https://nomehost | Accesso negato | Reindirizza alla pagina web di IBM | Reindirizza alla pagina web di IBM | 
|http://cname| 301 Spostato definitivamente | Accesso negato | Caricamento riuscito | 
|https://cname| Reindirizza alla pagina web di IBM | Caricamento riuscito | Caricamento riuscito |

## Come configuro la mia CDN (Content Delivery Network) per IBM COS (Cloud Object Storage)?
[Ecco un'esercitazione](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sulla creazione di una CDN (Content Delivery Network) per IBM Cloud Object Storage

## Perché il mio nome host non viene caricato sul browser quando scelgo IBM COS (Cloud Object Storage) come origine?

Quando la tua CDN IBM Cloud è configurata per utilizzare IBM COS come archivio oggetti, l'accesso diretto al sito web non funzionerà. Devi specificare il percorso della richiesta completo nella barra dell'indirizzo del browser (ad esempio, `www.example.com/index.html`). Questa modalità di funzionamento è dovuta alla limitazione del documento di indice in IBM COS.

## Quale è la dimensione file più grande che può essere recapitata tramite la CDN Akamai?

Dei tentativi di richiamare o distribuire file più grandi di 1,8GB riceveranno una risposta `403 Access Forbidden` (Accesso non consentito) per la configurazione delle prestazioni predefinita. Se l'ottimizzazione dei file di grandi dimensioni è abilitata, sono possibili dei download di file fino a 320GB.

## Ho ricevuto una notifica che il mio certificato di origine sta per scadere. Cosa faccio adesso?

Attieniti alla procedura indicata in [questo articolo](https://community.akamai.com/docs/DOC-7708) da Akamai.

## Cos'è una richiesta di intervallo di byte e come funziona con la CDN Akamai?

Una richiesta di intervallo di byte viene utilizzata per richiamare del contenuto parziale da un server di origine. L'intestazione della richiesta HTTP di intervallo indica quale parte del contenuto deve essere restituita dal server. È possibile richiedere diverse parti per volta con una singola intestazione di intervallo e il server può restituire tali intervalli in una risposta in più parti. Se il server restituisce degli intervalli, risponde con uno stato 206 (Contenuto parziale).

Quando una richiesta di intervallo di byte viene inviata utilizzando la CDN IBM Cloud con Akamai, l'utente può ricevere un codice risposta 200 (OK) per la prima richiesta e un codice risposta 206 per tutte le successive richieste. Ciò è dovuto al fatto che i server edge Akamai richiedono il contenuto dall'origine in formato compresso. Quindi, quando un server edge non ha un oggetto nella sua cache, né ha alcuna informazione relativa alla lunghezza del contenuto dell'oggetto, procederà all'origine e richiederà l'intero oggetto. In questo caso, l'origine fornisce l'oggetto senza l'intestazione di lunghezza del contenuto ad Akamai e all'utente finale sarà fornito l'intero oggetto anche se si trattava di una richiesta di intervallo di byte. Per tale motivo viene generato il codice di stato 200. Nelle successive richieste, il server edge ha l'oggetto nella sua cache e fornirà il codice di stato 206.

Un modo per assicurare una risposta 206, anche per la prima richiesta di intervallo di byte, consiste nel disabilitare `Transfer-Encoding: chunked` sul tuo server di origine.

## Quale sicurezza è inclusa con la soluzione IBM CDN con Akamai?

Utilizzando la piattaforma Akamai distribuita, ottieni una capacità di ridimensionamento e una resilienza senza pari, con più di 240.000 server in più di 130 paesi. La Akamai Platform si frappone tra la tua infrastruttura e i tuoi utenti finali e funge da primo livello di difesa per improvvisi picchi di traffico. Anche la Akamai Intelligent Platform è un proxy inverso che ascolta e risponde alle richieste solo sulle porte 80 e 443, il che significa che il traffico sulle altre porte viene rilasciato sull'edge senza essere inoltrato alla tua infrastruttura.
