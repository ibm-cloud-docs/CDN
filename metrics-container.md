---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# Metrics container
{: #metrics-container}

The `SoftLayer_Container_Network_CdnMarketplace_Metrics` collection contains the attributes that are used by our Metrics APIs. There are three possible values for the `type` attribute: PIECHART, LINEGRAPH, or TOTALS. The output for the rest of the attributes in this container depend on the `type`, which is described in further detail. All attributes except `type` are arrays. Not all attributes are used for all types.
{:shortdesc}

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

The PIECHART type is for data that is sent to a pie chart. In this case, the only attributes that are used are: `names`, `xaxis`, and `yaxis`. The `names` attribute is an array containing the region names. The `xaxis` attribute contains the usage in GB, and `yaxis1` is the test percentage.


The LINEGRAPH type is for data that will be sent to a line graph. In this case, the `names` array ontains the names of the metrics that are found in the `xaxis`, `yaxis`, `yaxis2`...`yaxis20` attributes, and the actual data is in the corresponding arrays.


If `type` is TOTALS, the `names` array contains the names of the metrics that are found in the corresponding `totals` array, and the actual data is contained in the `totals` array.
