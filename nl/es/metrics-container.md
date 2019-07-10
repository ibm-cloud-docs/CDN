---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# Clase de contenedor para métricas
{: #container-class-for-metrics}

La recopilación `SoftLayer_Container_Network_CdnMarketplace_Metrics` contiene los atributos para nuestras API de métricas. Existen tres valores posibles para el atributo `type`: PIECHART, LINEGRAPH o TOTALS. La salida para los demás atributos de este contenedor dependerá del `type`, que se describe al detalle a continuación. Todos los atributos excepto `type` son matrices. No todos los atributos se utilizan para todos los tipos.

**clase** `SoftLayer_Container_Network_CdnMarketplace_Metrics`:
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

El tipo PIECHART es para datos que se enviarán a un gráfico circular. En este caso, los únicos atributos utilizados son: `names`, `xaxis` e `yaxis`. El atributo `names` es una matriz que contiene los nombres de región. El atributo `xaxis` contiene el uso en GB, e `yaxis1` es el porcentaje de prueba.


El tipo LINEGRAPH es para datos que se enviarán a un gráfico de líneas. En este caso, la matriz `names` contendrá los nombres de las métricas que se encuentran en los atributos `xaxis`, `yaxis`, `yaxis2`...`yaxis20`, y los datos reales estarán en las matrices correspondientes.


Si `type` es TOTALS, la matriz `names` contendrá los nombres de las métricas que se encuentran en la matriz `totals` correspondiente, y los datos reales estarán en la matriz `totals`.
