---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Metriche
{: #metrics}

Quando selezioni inizialmente la tua CDN dall'elenco, viene aperta la pagina di panoramica (Overview). Qui puoi vedere la larghezza di banda totale (Total Bandwidth), il totale riscontri (Total Hits) e la percentuale riscontri (Hit Ratio) per il periodo di tempo selezionato (il valore predefinito è 30 giorni).

  ![Panoramica delle metriche](images/metrics-overview.png)

Direttamente sotto la panoramica, vedrai una rappresentazione grafica per la larghezza di banda totale (Total Bandwidth), la larghezza di banda per ogni regione (Bandwidth per Region), il totale riscontri (Total Hits) e i riscontri per tipo (Hits By Type).

  ![Grafici delle metriche](images/metrics-graphs.png)

**NOTA**: dopo che hai creato la tua CDN, potrebbero essere necessarie fino a 48 ore perché le metriche vengano visualizzate.

## C'è un numero minimo di giorni per cui posso visualizzare le metriche? Esiste un massimo?

C'è un numero minimo e uno massimo di giorni per cui puoi visualizzare le metriche utilizzando la CDN (Content Delivery Network) {{site.data.keyword.cloud}} con Akamai. Le metriche possono essere raccolte per un minimo di 7 giorni e possono essere visualizzate per un massimo di 90 giorni. Per coloro che utilizzano l'API, si consiglia di utilizzare 89 giorni come massimo, per tenere conto delle differenze di fuso orario.

## Perché la percentuale di riscontri non è zero quando il totale di riscontri è zero?
La percentuale di riscontri rappresenta la percentuale di volte in cui il contenuto è stato distribuito dalla cache del server edge, piuttosto che dal server di origine. Viene calcolata nel seguente modo:

`((Edge hits - Ingress hits)/Edge hits) * 100`

dove:

_Edge hits_ è definito come "All hits to the edge servers from the end-users."  
_Ingress hits_ è definito come "Origin or Ingress hits are for traffic from your origin to Akamai edge servers."

Poiché la percentuale di riscontri viene calcolata a livello dell'account e non per CDN, essa sarà uguale per tutte le CDN nel tuo account. Questo spiega inoltre perché la percentuale di riscontri può essere diversa da zero quando il numero di riscontri edge per una CDN particolare è zero.

## Le metriche sono aggiornate in tempo reale?

No. Le metriche vengono aggiornate ogni 24 ore.
