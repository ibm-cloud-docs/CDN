---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Utilizzo di Cache Control per controllare la durata della memorizzazione in cache di un client HTTP
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

Quando si utilizza una CDN, sono disponibili due livelli di memorizzazione nella cache:

  * La **Caching at the edge** (Memorizzazione in cache all'edge) si verifica quando un server edge CDN memorizza in cache del contenuto dall'origine.
  * La **Caching downstream** (Memorizzazione in cache di downstream) dalla rete edge di server si verifica quando un utente finale o un client HTTP, come ad esempio un browser richiedente, memorizza in cache del contenuto da un server edge.

Il metodo che scegli per controllare la durata della memorizzazione in cache presso il richiedente, come ad esempio un browser, dipende dai seguenti fattori:

  * Se l'[intestazione Respect Header](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details) è ON oppure OFF. Per impostazione predefinita è impostata su ON.
  * Se il server di origine fornisce un valore `max-age` nell'intestazione Cache-Control per del contenuto specifico. 

Indipendentemente da come variano questi fattori, la tua origine deve fornire un'intestazione Cache-Control per il contenuto previsto all'edge, se vuoi che i server edge inviino delle risposte HTTP con l'intestazione Cache-Control per tale contenuto.

Essenzialmente, le intestazioni Cache-Control inviate da un server edge downstream chiedono al richiedente di memorizzare in cache il contenuto associato in base alle direttive o ai valori di memorizzazione in cache specificati dal server edge.

## Respect Header: Off
{: #respect-header-off}

Se la tua origine non fornisce un'intestazione Cache-Control con una direttiva e un valore `max-age` per del contenuto specifico, la durata per la cache per tale contenuto specifico memorizzato in cache sull'edge è ancora derivato dalle impostazioni TTL della tua CDN. Inoltre, il server edge risponde al richiedente downstream con un valore `max-age` di Cache-Control uguale a quello minore tra questi due:
  * Il valore `max-age` di Cache-Control dell'origine.
  * Il tempo residuo rimanente fino a quando il contenuto non diventa obsoleto sull'edge.

Tuttavia, se la tua origine non fornisce un'intestazione Cache-Control al server edge, i server edge non forniranno un'intestazione Cache-Control al richiedente. La durata della cache edge per il tuo contenuto è ancora derivata dalle impostazioni TTL della tua CDN.

## Respect Header: On
{: #respect-header-on}

Se la tua origine non fornisce un'intestazione Cache-Control con `max-age` per del contenuto specifico, il valore `max-age` di Cache-Control dell'origine diventa la durata della cache per tale specifico contenuto memorizzato in cache sull'edge, sovrascrivendo qualsiasi impostazione TTL applicabile per tale contenuto. Inoltre, l'edge risponde al richiedente con un valore `max-age` di Cache-Control uguale al tempo residuo rimanente finché il contenuto non diventa obsoleto sul server edge.

Tuttavia, se la tua origine non fornisce un'intestazione Cache-Control al server edge, il server edge non fornirà un'intestazione Cache-Control al richiedente. La durata della cache edge per il tuo contenuto è ancora derivata dalle impostazioni TTL della tua CDN.

## Riepilogo
{: #summary}

|Respect Header|L'origine fornisce Cache-Control|Durata in cache del contenuto specifico sul server edge|Il server edge fornisce Cache-Control|
|---|---|---|---|
|On|Sì, l'origine specifica un `max-age`|Durata della cache edge sovrascritta con il valore `max-age` dell'origine|Sì, edge fornisce anche un `max-age` con un valore che è il tempo (sovrascritto) prima che l'edge debba eseguire l'aggiornamento del contenuto dall'origine|
|On|Sì, ma l'origine non specifica un `max-age`|Durata della cache edge basata sulla configurazione TTL della CDN|Sì, edge fornisce anche un `max-age` con un valore che è il tempo prima che l'edge debba eseguire l'aggiornamento del contenuto dall'origine|
|On|No|Durata della cache edge basata sulla configurazione TTL della CDN|No|
|Off|Sì, l'origine specifica un `max-age`|Durata della cache edge basata sulla configurazione TTL della CDN|Sì, edge fornisce anche un valore `max-age` che è inferiore al valore `max-age` dell'origine e il tempo prima che l'edge debba eseguire l'aggiornamento del contenuto dall'origine|
|Off|Sì, ma l'origine non specifica un `max-age`|Durata della cache edge basata sulla configurazione TTL della CDN|Sì, edge fornisce anche un `max-age` con un valore che è il tempo prima che l'edge debba eseguire l'aggiornamento del contenuto dall'origine|
|Off|No|Durata della cache edge basata sulla configurazione TTL della CDN|No|

## Ulteriori informazioni sul controllo della cache
{: #more-information-on-cache-control}

* Come [gestire la tua CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)
* Cache-Control come definito nella sezione 14.9 di [RFC 2616 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ietf.org/rfc/rfc2616.txt)
