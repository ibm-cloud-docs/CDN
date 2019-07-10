---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# Domande frequenti
{: #faqs}

## Cos'è una CDN (Content Delivery Network)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Una CDN (Content Delivery Network) è una raccolta di server edge distribuiti in varie parti del paese o del mondo. Il contenuto web viene fornito da un server edge che si trova nella zona geografica più vicina al cliente che richiede il contenuto. Questa tecnica consente agli utenti di ricevere il contenuto con meno ritardi di quanto possiamo ottenere consegnando il contenuto da una posizione centralizzata. Fornisce un'esperienza complessiva migliore per i tuoi clienti.

## Come funziona una CDN (Content Delivery Network)?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

Una CDN raggiunge il suo scopo memorizzando in cache il contenuto web sui server edge in tutto il mondo. Quando un utente richiede dei contenuti web, la richiesta di contenuto viene indirizzata al server edge geograficamente più vicino a tale utente. Riducendo la distanza che il contenuto deve percorrere, il CDN offre un rendimento ottimizzato, una latenza ridotta e prestazioni più elevate.

## Come viene creato il mio account di servizio CDN (Content Delivery Network) IBM Cloud?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

Il tuo account viene creato durante il processo dell'ordine CDN. Se stai creando una CDN dal portale legacy, quando fai clic sul pulsante **Order CDN** nella **pagina Network -> CDN**, il tuo account viene creato. Se stai creando una CDN dal portale {{site.data.keyword.cloud}}, quando fai clic sul pulsante **Create** nella **pagina Network -> Content**, il tuo account viene creato.

## Cosa devo fare quando la mia CDN è nello stato CNAME Configuration?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

Per la CDN HTTPS basata sul certificato SAN e HTTP, aggiorna il tuo record DNS in modo che il tuo sito web punti al `CNAME` associato alla tua nuova associazione CDN. Per la CDN HTTPS basata sul certificato jolly, questo aggiornamento del DNS **NON** è necessario.

**Nota**: l'attivazione dell'aggiornamento potrebbe richiedere fino a 15-30 minuti. Contatta il tuo provider DNS per ottenere una stima di tempo accurata.

## Come aggiungo un record CNAME per il mio dominio CDN in DNS?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

Nella tua pagina di configurazione DNS per il tuo dominio CDN, puoi creare un record CNAME con il nome dominio CDN come Host e il CNAME IBM che hai utilizzato per configurare la CDN come CNAME. Il CNAME IBM termina con `cdnedge.bluemix.net`.

Ecco come si presenta un tipico record CNAME nella pagina di configurazione DNS:

| **Tipo di risorsa** | **Host** | **Punta a (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minuti |


## Cosa mi verrà addebitato in fattura per la mia CDN
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

Ti viene addebitata solo la larghezza di banda utilizzata per l'istanza CDN (Content Delivery Network) IBM Cloud. Se il tuo CDN non utilizza alcuna larghezza di banda, non ti verranno addebitate spese. I prezzi della larghezza di banda variano a seconda della posizione regionale del server edge. Puoi visualizzare i prezzi della larghezza di banda per regione geografica nel [documento dei prezzi](/docs/infrastructure/CDN?topic=CDN-pricing) per questo servizio.

## Quando riceverò l'addebito in fattura per la mia CDN?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

La fatturazione di IBM Cloud Content Delivery Network avviene in base al periodo di fatturazione stabilito nel tuo account {{site.data.keyword.BluSoftlayer_notm}}.

## Se seleziono `delete` dal menu Overflow CDN, viene eliminato il mio account?
{: faq}

No; verrà eliminato solo quel CDN. Il tuo account esiste ancora e puoi creare altri CDN.

## La memorizzazione in cache di contenuto utilizza il push o il pull?
{: #does-content-caching-use-push-or-pull}
{: faq}

La memorizzazione del contenuto nella cache viene effettuata utilizzando il modello _pull di origine_. Il pull di origine è un metodo con cui i dati vengono "estratti" dal server edge dall'interno del server di origine, a differenza del caricamento manuale del contenuto sul server edge.

## C'è un browser consigliato da utilizzare per la configurazione del servizio CDN?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Sì, Firefox e Chrome sono i browser consigliati. Ti consigliamo di utilizzare le loro versioni più recenti con la tua CDN (Content Delivery Network) IBM Cloud.

## Qual è lo scopo di fornire un percorso quando creo la mia CDN?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

Fornire un percorso mentre crei la tua CDN ti consente di isolare i file che possono essere forniti tramite la CDN da uno specifico server di origine.

## Il mio CDN è in uno stato di errore. Cosa faccio adesso?
{: faq}

Fai riferimento alle pagine [Risoluzione dei problemi](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) o [Come ottenere aiuto e supporto](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp) oppure apri un ticket nel [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/).

## Dove trovo il CNAME per la mia CDN se non ne ho fornito uno?
{: faq}

Fai clic sulla CDN per accedere alla pagina **Panoramica** nel portale. Nell'angolo destro puoi vedere una sezione **Dettagli** con le informazioni sul `CName`.

## La mia richiesta di eliminazione per un determinato percorso file è in corso. Posso inviare una nuova richiesta per lo stesso percorso file?

No. Ci può essere una sola richiesta di eliminazione attiva per un determinato percorso di file alla volta.

## IPv6 (Internet Protocol version 6) è supportato con il servizio IBM Cloud Content Delivery Network? Come funziona?
{: faq}

IPv6 (o supporto dual stack) è supportato dai server edge di Akamai. È progettato per aiutare i clienti solo con origine IPv4 ad accettare le connessioni dai client IPv6, eseguire la conversione da IPv6 e IPv4 ai margini e andare avanti all'origine con IPv4.

**NOTA:** la creazione di una IBM Cloud CDN utilizzando un indirizzo IPv6 come indirizzo del server di origine non è supportata.

## Esistono delle limitazioni sui numeri porta HTTP e HTTPS consentiti per Akamai?
{: faq}

Sì. Per il fornitore Akamai, sono consentiti solo i seguenti numeri porta:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 e 45002.

## Quale URL deve essere utilizzato per accedere ai dati nel CDN o nel percorso di origine?
{: faq}

Il percorso di un'associazione CDN o per l'origine viene trattato come una directory. Quindi, gli utenti che tentano di accedere al percorso di origine devono accedervi come a una directory (con una barra). Ad esempio, se viene creato il CDN `www.example.com` utilizzando il percorso che include la directory `/images`, l'URL per raggiungerlo deve essere `www.example.com/images/`

L'omissione della barra, ad esempio, utilizzando `www.example.com/images` provocherà un errore **Pagina non trovata**.

## Come configuro la mia CDN (Content Delivery Network) per IBM COS (Cloud Object Storage)?
{: faq}

[Ecco un'esercitazione](/docs/tutorials?topic=solution-tutorials-static-files-cdn) sulla creazione di una CDN (Content Delivery Network) per IBM Cloud Object Storage

## Ho ricevuto una notifica che il mio certificato di origine sta per scadere. Cosa faccio adesso?
{: faq}

Attieniti alla procedura indicata in [questo articolo![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://community.akamai.com/docs/DOC-7708) di Akamai.

## Quale sicurezza è inclusa con la soluzione IBM CDN con Akamai?
{: faq}

Utilizzando la piattaforma Akamai distribuita, ottieni una capacità di ridimensionamento e una resilienza senza pari con migliaia di server in più di 50 paesi. La Akamai Platform si frappone tra la tua infrastruttura e i tuoi utenti finali e funge da primo livello di difesa per improvvisi picchi di traffico. Anche la Akamai Intelligent Platform è un proxy inverso che ascolta e risponde alle richieste solo sulle porte 80 e 443, il che significa che il traffico sulle altre porte viene rilasciato sull'edge senza essere inoltrato alla tua infrastruttura.

## I cookie dal server di origine sono conservati dalla CDN Akamai?
{: faq}

Per il contenuto non memorizzabile in cache, o qualsiasi contenuto che non viene memorizzato nella cache, i cookie vengono conservati dall'origine. Per il contenuto memorizzato in cache dai server edge, i cookie non vengono conservati.

## Come utilizzo la console IBM Cloud per dare ad altri utenti l'autorizzazione a creare o gestire una CDN?
{: faq}

L'utente master dell'account può fornire ad altri utenti l'autorizzazione a creare e gestire una CDN.

Dalla pagina principale della console IBM Cloud, attieniti alla seguente procedura per modificare le autorizzazioni:
 * Seleziona la scheda **Gestisci**
 * Seleziona **Accesso (IAM)**
 * Fai clic sulla scheda **Utenti** dal riquadro di sinistra
 * Fai clic sull'**Utente** desiderato
 * Seleziona quindi la scheda **Infrastruttura classica**
 * Quindi, nella scheda **Autorizzazioni**, espandi la categoria **Servizi**
 * Seleziona **Gestisci account CDN**
 * Fai clic sul pulsante **Salva**

Dalla pagina principale della console legacy, attieniti alla seguente procedura per modificare le autorizzazioni:
 * Seleziona la scheda **Account**
 * Seleziona **Utenti -> Elenco utenti**
 * Fai clic sul **Nome utente** desiderato
 * Seleziona quindi la scheda **Autorizzazioni portale**
 * Seleziona la scheda **Servizi**
 * Seleziona **Gestisci account CDN**
 * Fai clic sul pulsante **Modifica autorizzazioni portale**

## Perché il pulsante Create non viene visualizzato o è disabilitato nella pagina https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai?
{: faq}

Se sei l'utente master dell'account, devi eseguire un upgrade dell'account perché il pulsante Create sia visualizzato o abilitato in questa pagina. Dalla pagina della console IBM Cloud, attieniti alla seguente procedura come utente master dell'account:
 * Apri il riquadro di navigazione di sinistra facendo clic sull'icona `tripla barra` nell'angolo superiore sinistro della pagina web
 * Seleziona **Infrastruttura classica**
 * Fai clic sul pulsante **Esegui upgrade dell'account** e attieniti alle istruzioni

Se sei uno degli utenti secondari dell'account, l'utente master dell'account ti deve concedere l'autorizzazione `Aggiungi/esegui upgrade dei servizi` perché il pulsante Create sia visualizzato o abilitato in questa pagina. Dalla pagina della console IBM Cloud, l'utente master dell'account deve attenersi alla seguente procedura per modificare le tue autorizzazioni:
 * Seleziona la scheda **Gestisci**
 * Seleziona **Accesso (IAM)**
 * Fai clic sulla scheda **Utenti** dal riquadro di sinistra
 * Fai clic sull'**Utente** desiderato
 * Seleziona quindi la scheda **Infrastruttura classica**
 * Quindi, nella scheda **Autorizzazioni**, espandi la categoria **Account**
 * Seleziona **Aggiungi/esegui upgrade dei servizi**
 * Fai clic sul pulsante **Salva**

## Perché non riesco a raggiungere la mia pagina web tramite la mia CDN dopo la configurazione di Hotlink Protection con `protectionType` `ALLOW`?
{: faq}

Consideriamo un esempio in cui il dominio del tuo sito web per gli utenti finali è configurato per essere il dominio/nome host della tua CDN: `cdn.example.com`. Quando qualcuno cerca di raggiungere una pagina web navigando direttamente dalla barra di navigazione del browser, il browser in genere non invia le intestazioni Referer nella sua richiesta HTTP. Ad esempio, quando navighi direttamente in questo modo a `https://cdn.example.com/`, la tua CDN considera che la richiesta contiene una non corrispondenza rispetto al `refererValues` specificato. Quando valuta l'effetto o la risposta appropriati tramite il tuo Hotlink Protection, la CDN determina che si è verificata una non corrispondenza. Pertanto, la tua CDN rifiuterà l'accesso, piuttosto che consentirlo ('ALLOW').

## Posso utilizzare l'endpoint privato dell'archiviazione oggetti nelle impostazioni della CDN?
{: faq}

No, la CDN può connettersi solo a Object Storage su endpoint pubblici.

## Posso utilizzare la funzione Brotli nel servizio CDN?
{: faq}

No, la funzione Brotli non è supportata con il nostro servizio CDN con Akamai.

## Come creo un endpoint CDN senza utilizzare il dominio?
{: faq}

Puoi creare un endpoint CDN senza utilizzare il dominio, ma SOLO per una CDN del tipo **Wildcard HTTPS**. Durante la creazione di una CDN del tipo **Wildcard HTTPS**, il tuo **CNAME** agisce come l'endpoint CDN e il **CNAME** viene utilizzato per fornire il traffico.
