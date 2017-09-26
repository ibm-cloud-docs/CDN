---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Domande frequenti

## Cos'è il CDN (Content Delivery Network)?

Il CDN (Content Delivery Network) è una raccolta di server perimetrali distribuiti in varie parti del paese o del mondo. Il contenuto web viene fornito da un server perimetrale che si trova nella zona geografica più vicina al cliente che richiede il contenuto. Questa tecnica consente agli utenti di ricevere il contenuto con meno ritardi di quanto possiamo ottenere consegnando il contenuto da una posizione centralizzata. Fornisce un'esperienza complessiva migliore per i tuoi clienti.

## Come funzione CDN?

CDN raggiunge il suo scopo memorizzando in cache i contenuti web nei server perimetrali in tutto il mondo. Quando un utente richiede dei contenuti web, la richiesta di contenuto viene indirizzata al server perimetrale geograficamente più vicino a tale utente. Riducendo la distanza che il contenuto deve percorrere, il CDN offre un rendimento ottimizzato, una latenza ridotta e prestazioni più elevate.  

## Come viene creato il mio account CDN?

Il tuo account viene creato durante l'elaborazione dell'ordine CDN, quando fai clic su **Seleziona** nel menu "Selezione fornitore".

## Cosa devo fare quando il mio CDN è in stato CONFIGURAZIONE_CNAME?

Aggiorna il record DNS in modo che il tuo sito web punti al `CNAME` associato alla tua nuova associazione CDN.
**Nota**: l'attivazione dell'aggiornamento potrebbe richiedere fino a 15-30 minuti. Contatta il tuo provider DNS per ottenere una stima di tempo accurata.

## Che cosa mi verrà addebitato?

Ti viene addebitata la larghezza di banda utilizzata per CDN. Se il tuo CDN non utilizza alcuna larghezza di banda, non ti verranno addebitate spese. I prezzi della larghezza di banda variano a seconda della posizione regionale del server perimetrale.

## Quando riceverò l'addebito?

La fatturazione CDN avviene in base al periodo di fatturazione stabilito nel tuo account {{site.data.keyword.BluSoftlayer_notm}}.

## Como posso visualizzare le metriche e l'utilizzo?

Puoi visualizzare le metriche e l'utilizzo nella pagina **Panoramica**, che può essere raggiunta selezionando il tuo CDN dalla pagina **Content Delivery Networks**. **Nota**: dopo aver creato il tuo CDN, potrebbero essere necessarie fino a 48 ore per visualizzare le metriche.

## Esiste un numero minimo di giorni in cui posso visualizzare le metriche? Esiste un massimo?

Sì. Le metriche possono essere raccolte per un minimo di 7 giorni e possono essere visualizzate per un massimo di 90 giorni. Per coloro che utilizzano l'API, si consiglia di utilizzare 89 giorni come massimo, per tenere conto delle differenze di fuso orario.

## Come funziona il certificato con carattere jolly HTTPS?

Il certificato con carattere jolly è il modo più conveniente per consegnare in modo sicuro i contenuti web ai tuoi utenti finali. Per utilizzare il certificato con carattere jolly, il cliente deve utilizzare il nome host CNAME come punto di ingresso del servizio (ad esempio, _https://example.cdnedge.bluemix.net_). Una volta abilitata l'associazione CDN per HTTPS utilizzando il certificato con carattere jolly, il server perimetrale contatta il server di origine attraverso HTTPS. Se il server di origine è specificato come nome host, per impostazione predefinita il server perimetrale utilizza il dominio di origine come intestazione SNI (Server Name Indication) per la negoziazione TLS (Transport Layer Security) con il server di origine. Tuttavia, se il dominio di origine è specificato come indirizzo IP, è necessario fornire l'intestazione di host di origine. Un altro suggerimento è che il certificato di origine deve essere firmato da un'autorità di certificazione (CA) riconosciuta.

## Se seleziono `delete` dal menu di overflow CDN, viene eliminato il mio account?

No; verrà eliminato solo quel CDN. Il tuo account esiste ancora e puoi creare altri CDN.

## La memorizzazione nella cache utilizza il push o il pull?

La memorizzazione del contenuto nella cache viene effettuata utilizzando il modello _pull di origine_. Il pull di origine è un metodo con cui i dati vengono "estratti" dal server perimetrale dall'interno del server di origine, a differenza del caricamento manuale del contenuto sul server perimetrale. 

## Esiste un valore massimo per Time To Live? Un minimo?

Il valore massimo per Time To Live è 21.474.836.471 secondi, che equivale a circa 681 anni! Il valore minimo è 30 secondi.

## Cos'è l'opzione Utilizza contenuto obsoleto?

Se il tempo di memorizzazione nella cache scade per un contenuto, il server perimetrale cercherà di recuperare il contenuto dal server di origine. Se per qualche motivo il server di origine è inattivo o non può essere contattato e se questa opzione è impostata, il server perimetrale può servire il contenuto obsoleto, il che è la scelta preferita dalla maggior parte dei clienti. Attualmente, i nostri CDN possono essere configurati solo con questa opzione impostata su **Sì** come valore predefinito. La modifica dell'opzione su **No** non avrà alcun impatto: l'opzione **Utilizza contenuto obsoleto** continuerà a essere impostata su **Sì**.

## Esiste un limite sul numero di voci di origine e TTL?

Sì, il limite combinato è di 75 voci per ogni CDN.

## Esiste un limite sul numero di CDN che posso avere?

Sì, puoi avere un limite di 10 CDN per account.

## C'è un browser consigliato da utilizzare per la configurazione del servizio CDN?

Sì, i browser consigliati sono Firefox e Chrome alle versioni più recenti.

## Qual è lo scopo di fornire un percorso durante la creazione del CDN?

Se fornisci un percorso durante la creazione di un CDN, ciò ti consente di isolare i file che possono essere serviti tramite CDN da un particolare server di origine.

## Come faccio a sapere se il mio CDN funziona?
Immetti il seguente comando 'curl' sostituendo "origin.cdntesting.net/assets/ibm_3d.gif" con il relativo percorso file sulla tua origine: 
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

## Dove posso trovare il mio CName se non ne ho fornito uno? 
Fai clic sul CDN per accedere alla pagina **Panoramica**. Nell'angolo destro puoi vedere una sezione **Dettagli** con le informazioni sul `CName`.

## Esiste un limite sul numero di richieste di eliminazione che possono essere attive contemporaneamente?
Sì. Ci possono essere solo 5 richieste di eliminazione attive alla volta.

## La mia richiesta di eliminazione per un determinato percorso file è in corso. Posso inviare una nuova richiesta per lo stesso percorso file?
No. Ci può essere una sola richiesta di eliminazione attiva per un determinato percorso di file alla volta.
