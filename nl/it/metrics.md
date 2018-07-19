---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Metriche

## Como posso visualizzare le metriche e l'utilizzo?

Puoi visualizzare le metriche e l'utilizzo nella pagina **Panoramica**, che può essere raggiunta selezionando la tua CDN dalla pagina **Content Delivery Networks**. **NOTA**: dopo che hai creato la tua CDN, potrebbero essere necessarie fino a 48 ore perché le metriche vengano visualizzate.

## Ho creato una CDN e c'era traffico di dati attraverso essa. Perché le mie metriche non vengono visualizzate?

Dopo che hai creato una CDN, ci vogliono 48 prima che le metriche vengano visualizzate.


## C'è un numero minimo di giorni per cui posso visualizzare le metriche? Esiste un massimo?

C'è un numero minimo e uno massimo di giorni per cui puoi visualizzare le metriche utilizzando la CDN (Content Delivery Network) IBM Cloud con Akamai. Le metriche possono essere raccolte per un minimo di 7 giorni e possono essere visualizzate per un massimo di 90 giorni. Per coloro che utilizzano l'API, si consiglia di utilizzare 89 giorni come massimo, per tenere conto delle differenze di fuso orario.

## Perché la percentuale di riscontri non è zero quando il totale di riscontri è zero?
La percentuale di riscontri rappresenta la percentuale di volte in cui il contenuto è stato distribuito dalla cache del server edge, piuttosto che dal server di origine. Viene calcolata nel seguente modo:

> ((Edge hits - Ingress hits)/Edge hits) * 100

dove:
I riscontri edge sono definiti come "Tutti i riscontri ai server edge dagli utenti finali"  
I riscontri Ingress sono definiti come "I riscontri di origine o Ingress per il traffico dalla tua origine ai server edge Akamai"

Poiché la percentuale di riscontri viene calcolata a livello dell'account e non per CDN, essa sarà uguale per tutte le CDN nel tuo account. Questo spiega inoltre perché la percentuale di riscontri può essere diversa da zero quando il numero di riscontri edge per una CDN particolare è zero.

