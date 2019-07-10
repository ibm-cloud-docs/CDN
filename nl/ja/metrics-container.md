---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# メトリック用のコンテナー・クラス
{: #container-class-for-metrics}

`SoftLayer_Container_Network_CdnMarketplace_Metrics` コレクションには、メトリック API によって使用される属性が含まれています。 `type` 属性に指定できる値には、PIECHART、LINEGRAPH、TOTALS の 3 つがあります。 このコンテナーの残りの属性の出力は、以下で詳細に説明されている `type` によって異なります。 `type` を除くすべての属性が配列です。 すべての属性がすべてのタイプに使用されるわけではありません。

**クラス** `SoftLayer_Container_Network_CdnMarketplace_Metrics`:
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

PIECHART タイプは、円グラフに送信されるデータ用です。 この場合、使用される属性は `names`、`xaxis`、および `yaxis` のみです。 `names` 属性は、地域名を含む配列です。 `xaxis` 属性には使用量 (GB) が含まれており、`yaxis1` はテストのパーセンテージです。


LINEGRAPH タイプは、折れ線グラフに送信されるデータ用です。 この場合、`names` 配列には、`xaxis`、`yaxis`、`yaxis2`...`yaxis20` の各属性にあるメトリックの名前が含められ、実際のデータは対応する配列内に含められます。


`type` が TOTALS の場合、`names` 配列には、対応する `totals` 配列内にあるメトリックの名前が含められ、実際のデータは `totals` 配列内に含められます。
