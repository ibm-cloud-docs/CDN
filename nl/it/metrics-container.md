---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# Classe contenitore per le metriche
{: #container-class-for-metrics}

La raccolta `SoftLayer_Container_Network_CdnMarketplace_Metrics` contiene gli attributi utilizzati dalle nostre API Metrics. Ci sono tre possibili valori per l'attributo `type`: PIECHART, LINEGRAPH o TOTALS. L'output per il resto degli attributi in questo contenitore dipende dal tipo (`type`), che è descritto in maggiore dettaglio di seguito. Tutti gli attributi tranne `type` sono degli array. Non tutti gli attributi sono utilizzati per tutti i tipi.

**class** `SoftLayer_Container_Network_CdnMarketplace_Metrics`:
* `type`
* `names`
* `totals`
* `percentage`
* `time`
* `xaxis`
* `yaxis`
* `yaxis2`
* `yaxis3`
* `yaxis4`
* `yaxis5`
* `yaxis6`
* `yaxis7`
* `yaxis8`
* `yaxis9`
* `yaxis10`
* `yaxis11`
* `yaxis12`
* `yaxis13`
* `yaxis14`
* `yaxis15`
* `yaxis16`
* `yaxis17`
* `yaxis18`
* `yaxis19`
* `yaxis20`

Il tipo PIECHART è per i dati che verranno inviati ad un grafico a torta. In questo caso, i soli attributi utilizzati sono: `names`, `xaxis` e `yaxis`. L'attributo `names` è un array che contiene i nomi regione. L'attributo `xaxis` contiene l'utilizzo in GB e `yaxis1` è la percentuale di test.


Il tipo LINEGRAPH è per i dati che verranno inviati ad un grafico a linee. In questo caso, l'array `names` conterrà i nomi delle metriche che si trovano negli attributi `xaxis`, `yaxis`, `yaxis2`...`yaxis20` e i dati effettivi saranno negli array corrispondenti.


Se `type` è TOTALS, l'array `names` conterrà i nomi delle metriche che si trovano nel corrispondente array `totals` e i dati effettivi sono contenuti nell'array `totals`.
