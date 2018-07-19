---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Informazioni sulla CDN (Content Delivery Network)

Una CDN (Content Delivery Network) è una raccolta di server edge distribuiti in varie parti del paese o del mondo. Il contenuto web viene fornito da un server edge che si trova nella zona geografica più vicina al cliente che richiede il contenuto. Questa tecnica consente ai tuoi utenti finali di ricevere il contenuto con meno ritardo e offre un'esperienza complessiva migliore ai tuoi clienti.

## Come funziona una CDN?

Una CDN raggiunge il suo scopo memorizzando in cache il contenuto web sui server edge in tutto il mondo. Quando un cliente richiede del contenuto web, la richiesta di contenuto viene indirizzata al server edge geograficamente più vicino a tale cliente. Riducendo la distanza che il contenuto deve percorrere, il CDN offre un rendimento ottimizzato, una latenza ridotta e prestazioni più elevate. Utilizzando IBM Cloud Content Delivery Network con Akamai, i provider di contenuto possono ottenere una fornitura efficiente del contenuto richiesto da tutto il mondo con una configurazione minima.

Le funzioni chiave del servizio CDN (Content Delivery Network) IBM Cloud includono:
  * Supporto dell'origine server host
  * Supporto dell'origine Object Storage
  * Supporto per più origini con percorsi distinti
  * Associazioni di CDN basate sui percorsi
  * Eliminazione del contenuto presente nella cache
  * TTL (Time to Live)
  * Metriche con viste grafiche
  * Supporto del protocollo HTTPS con certificato wildcard e SAN DV
  * Supporto dell'intestazione host (Host Header)
  * Utilizzo di contenuto obsoleto
  * Argomenti di query della chiave della cache
  * Compressione del contenuto
  * Ottimizzazione dei file di grandi dimensioni
  * Video on-demand

Consulta [questo documento](feature-description.html#feature-descriptions) per le descrizioni complete delle funzioni.

## Digramma dell'architettura

Il seguente diagramma offre una panoramica schematica dell'architettura a tre livelli di CDN IBM Cloud.

![Digramma dell'architettura](images/3-tier-architecture.png)
