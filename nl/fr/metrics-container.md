---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# Classe de conteneur pour Metrics
{: #container-class-for-metrics}

La collection `SoftLayer_Container_Network_CdnMarketplace_Metrics` contient les attributs utilisés par nos API Metrics. Il existe trois valeurs possibles pour l'attribut `type` : PIECHART, LINEGRAPH ou TOTALS. La sortie du reste des attributs de ce conteneur dépend du `type`, qui est décrit plus en détail ci-dessous. Tous les attributs sauf `type` sont des tableaux. Tous les attributs ne sont pas utilisés pour tous les types.

**Classe** `SoftLayer_Container_Network_CdnMarketplace_Metrics` :
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

Le type PIECHART est réservé aux données qui seront envoyées à un graphique circulaire. Dans ce cas, les seuls attributs utilisés sont : `names`, `xaxis` et `yaxis`. L'attribut `names` est un tableau contenant les noms de régions. L'attribut `xaxis` contient l'utilisation en Go et l'attribut `yaxis1` représente le pourcentage de test.


Le type LINEGRAPH est réservé aux données qui seront envoyées à un diagramme linéaire. Dans ce cas, le tableau `names` contiendra les noms des mesures qui se trouvent dans les attributs `xaxis`, `yaxis`, `yaxis2`...`yaxis20` et les données en cours seront dans les tableaux correspondants.


Si le `type` est TOTALS, le tableau `names` contiendra les noms des mesures se trouvant dans le tableau `totals` correspondant et les données en cours seront contenues dans le tableau `totals`.
