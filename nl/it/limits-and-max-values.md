---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limiti e valori massimi
{: #limits-and-maximum-values}

## Esiste un valore massimo per Time To Live? Un minimo?
{: #is-there-a-maximum-value-for-time-to-live}

Il valore massimo per Time To Live è 2.147.483.647 secondi, che equivale a circa 68 anni! Il valore minimo è 0 secondi.

## Esiste un limite sul numero di voci di origine e TTL?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

Sì, il limite combinato è di 75 voci per ogni CDN.

## Quale è la dimensione file più grande che può essere recapitata tramite la CDN Akamai?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

Dei tentativi di richiamare o distribuire file più grandi di 1,8GB riceveranno una risposta `403 Access Forbidden` (Accesso non consentito) per la configurazione delle prestazioni predefinita. Se l'ottimizzazione dei file di grandi dimensioni è abilitata, sono possibili dei download di file fino a 320GB.

## Qual è la dimensione massima per le intestazioni della risposta di origine?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

La dimensione dell'intestazione della risposta massima è 16KB. Nel caso in cui alcune origini configurino la restituzione di cookie troppo grandi per il client nelle successive richieste, Akamai imposta questo limite nei server edge. Se le intestazioni della risposta sono più grandi di 16KB, la richiesta riceverà una risposta `502 Bad Gateway`.

## Qual è la dimensione massima per le intestazioni della richiesta del client?
{: #what-is-the-maximum-size-for-the-client-request-headers}

La dimensione delle intestazioni della richiesta massima è 32KB. Se le intestazioni della richiesta sono più grandi di 32KB, il server edge Akamai restituirà una risposta `400 Bad Request`.
