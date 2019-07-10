---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# Utilizzo delle richieste di intervallo di byte
{: #working-with-byte-range-requests}

Utilizzando una richiesta di intervallo di byte, puoi richiamare il contenuto parziale da un server di origine. Questo documento può aiutarti a comprendere i codici di stato della risposta che potresti visualizzare.

Quando una **Richiesta di intervallo di byte** viene inviata utilizzando la {{site.data.keyword.cloud}} CDN con Akamai, l'utente può ricevere un codice risposta `200 (OK)` per la prima richiesta e un codice risposta `206` per tutte le richieste successive, in quanto i server edge Akamai richiedono il contenuto dall'origine in formato compresso. Pertanto, quando un server edge non ha un oggetto nella sua cache, né ha alcuna informazione relativa alla lunghezza del contenuto dell'oggetto, effettua un inoltro all'origine e richiede l'intero oggetto. In cambio, l'origine fornisce l'oggetto senza l'intestazione della lunghezza del contenuto ad Akamai e all'utente finale viene fornito l'intero oggetto anche se si trattava di una richiesta di intervallo di byte. Per tale motivo viene generato il codice di stato `200`. Nelle successive richieste, il server edge ha l'oggetto nella sua cache e fornisce il codice di stato `206`.

L'intestazione **Richiesta HTTP di intervallo** indica quale parte del contenuto deve essere restituita dal server. È possibile richiedere diverse parti per volta con una singola intestazione di intervallo e il server può restituire tali intervalli in una risposta in più parti. Se il server restituisce degli intervalli, risponde con uno stato 206 (Contenuto parziale).

Un modo per assicurare una risposta 206, anche per la prima richiesta di intervallo di byte, consiste nel disabilitare `Transfer-Encoding: chunked` sul tuo server di origine.
