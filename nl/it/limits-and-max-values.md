---

copyright:
  years: 2018
lastupdated: "2018-10-04"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Limiti e valori massimi

## Esiste un valore massimo per Time To Live? Un minimo?

Il valore massimo per Time To Live è 2.147.483.647 secondi, che equivale a circa 68 anni! Il valore minimo è 0 secondi.

## Esiste un limite sul numero di voci di origine e TTL?

Sì, il limite combinato è di 75 voci per ogni CDN.

## Quale è la dimensione file più grande che può essere recapitata tramite la CDN Akamai?

Dei tentativi di richiamare o distribuire file più grandi di 1,8GB riceveranno una risposta `403 Access Forbidden` (Accesso non consentito) per la configurazione delle prestazioni predefinita. Se l'ottimizzazione dei file di grandi dimensioni è abilitata, sono possibili dei download di file fino a 320GB.
