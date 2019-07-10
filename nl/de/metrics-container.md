---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# Containerklasse für Metriken
{: #container-class-for-metrics}

Die Sammlung `SoftLayer_Container_Network_CdnMarketplace_Metrics` enthält die Attribute, die von den Metrik-APIs verwendet werden. Es gibt drei mögliche Werte für das Attribut `type`: PIECHART (Kreisdiagramm), LINEGRAPH (Kurvendiagramm) oder TOTALS (Summen). Die Ausgabe für die übrigen Attribute in diesem Container hängt vom Typ (`type`) ab. Die Typen werden nachfolgend eingehender beschrieben. Alle Attribute mit Ausnahme von `type` sind Arrays. Nicht alle Attribute werden für alle Typen verwendet.

**Klasse** `SoftLayer_Container_Network_CdnMarketplace_Metrics`:
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

Der Typ PIECHART ist für Daten vorgesehen, die an ein Kreisdiagramm gesendet werden. In diesem Fall werden nur die folgenden Attribute verwendet: `names`, `xaxis` und `yaxis`. Das Attribut `names` ist ein Array, das die Regionsnamen enthält. Das Attribut `xaxis` enthält die Nutzung in GB und das Attribut `yaxis1` ist der Testprozentsatz.


Der Typ LINEGRAPH ist für Daten vorgesehen, die an ein Kurvendiagramm gesendet werden. In diesem Fall enthält das Array `names` die Namen der Metriken, die sich in den Attributen `xaxis`, `yaxis`, `yaxis2`...`yaxis20` befinden, und die tatsächlichen Daten sind in den entsprechenden Arrays enthalten.


Wenn `type` den Wert TOTALS hat, enthält das Array `names` die Namen der Metriken, die sich in dem entsprechenden Array `totals` befinden, und die tatsächlichen Daten sind im Array `totals` enthalten.
