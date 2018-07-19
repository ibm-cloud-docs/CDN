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

Ti viene addebitata solo la larghezza di banda utilizzata per l'istanza CDN (Content Delivery Network) IBM Cloud. Se il tuo CDN non utilizza alcuna larghezza di banda, non ti verranno addebitate spese. I prezzi della larghezza di banda variano a seconda della posizione regionale del server edge. Puoi vedere i prezzi per regione geografica nella sezione [Introduzione](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-) per questo servizio.

## Quando riceverò l'addebito?

La fatturazione di IBM Cloud Content Delivery Network avviene in base al periodo di fatturazione stabilito nel tuo account {{site.data.keyword.BluSoftlayer_notm}}.

## Se seleziono `delete` dal menu di overflow CDN, viene eliminato il mio account?

No; verrà eliminato solo quel CDN. Il tuo account esiste ancora e puoi creare altri CDN.

## La memorizzazione nella cache utilizza il push o il pull?

La memorizzazione del contenuto nella cache viene effettuata utilizzando il modello _pull di origine_. Il pull di origine è un metodo con cui i dati vengono "estratti" dal server edge dall'interno del server di origine, a differenza del caricamento manuale del contenuto sul server edge.

## C'è un browser consigliato da utilizzare per la configurazione del servizio CDN?

Sì, Firefox e Chrome sono i browser consigliati. Ti consigliamo di utilizzare le loro versioni più recenti con la tua CDN (Content Delivery Network) IBM Cloud.

## Qual è lo scopo di fornire un percorso quando creo la mia CDN?

Fornire un percorso mentre crei la tua CDN ti consente di isolare i file che possono essere forniti tramite la CDN da uno specifico server di origine.

## La mia CDN è in uno stato di errore. Cosa faccio adesso?

Fai riferimento alle pagine [Risoluzione dei problemi](troubleshooting.html#troubleshooting) o [Come ottenere aiuto e supporto](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#getting-help) oppure apri un ticket nel [Portale del cliente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/).

## Dove trovo il mio CNAME se non ne ho fornito uno?

Fai clic sulla CDN per accedere alla pagina **Panoramica** nel portale. Nell'angolo destro puoi vedere una sezione **Dettagli** con le informazioni sul `CName`.

## La mia richiesta di eliminazione per un determinato percorso file è in corso. Posso inviare una nuova richiesta per lo stesso percorso file?

No. Ci può essere una sola richiesta di eliminazione attiva per un determinato percorso di file alla volta.

## IPv6 (Internet Protocol version 6) è supportato con il servizio IBM Cloud Content Delivery Network? Come funziona?

IPv6 (o supporto dual stack) è supportato dai server edge di Akamai. È progettato per aiutare i clienti solo con origine IPv4 ad accettare le connessioni dai client IPv6, eseguire la conversione da IPv6 e IPv4 ai margini e andare avanti all'origine con IPv4.

**NOTA:** la creazione di una CDN IBM Cloud utilizzando un indirizzo IPv6 come indirizzo del server di origine non è supportata.

## Esistono delle limitazioni sui numeri porta HTTP e HTTPS consentiti per Akamai?

Sì. Per il fornitore Akamai, sono consentiti solo i seguenti numeri porta:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 e 45002.

## Quale URL deve essere utilizzato per accedere ai dati nel CDN o nel percorso di origine?
Il percorso di un'associazione CDN o per l'origine viene trattato come una directory. Quindi, gli utenti che tentano di accedere al percorso di origine devono accedervi come a una directory (con una barra). Ad esempio, se viene creato il CDN `www.example.com` utilizzando il percorso che include la directory `/images`, l'URL per raggiungerlo deve essere `www.example.com/images/`

L'omissione della barra, ad esempio, utilizzando `www.example.com/images` provocherà un errore **Pagina non trovata**.

## Come configuro la mia CDN (Content Delivery Network) per IBM COS (Cloud Object Storage)?

[Ecco un'esercitazione](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sulla creazione di una CDN (Content Delivery Network) per IBM Cloud Object Storage

## Ho ricevuto una notifica che il mio certificato di origine sta per scadere. Cosa faccio adesso?

Attieniti alla procedura indicata in [questo articolo](https://community.akamai.com/docs/DOC-7708) da Akamai.

## Quale sicurezza è inclusa con la soluzione IBM CDN con Akamai?

Utilizzando la piattaforma Akamai distribuita, ottieni una capacità di ridimensionamento e una resilienza senza pari, con più di 240.000 server in più di 130 paesi. La Akamai Platform si frappone tra la tua infrastruttura e i tuoi utenti finali e funge da primo livello di difesa per improvvisi picchi di traffico. Anche la Akamai Intelligent Platform è un proxy inverso che ascolta e risponde alle richieste solo sulle porte 80 e 443, il che significa che il traffico sulle altre porte viene rilasciato sull'edge senza essere inoltrato alla tua infrastruttura.
