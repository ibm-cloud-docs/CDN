---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# 度量值的容器類別
{: #container-class-for-metrics}

`SoftLayer_Container_Network_CdnMarketplace_Metrics` 集合包含了我們的度量值 API 所使用的屬性。`type` 屬性有三個可能值：PIECHART、LINEGRAPH 或 TOTALS。此容器中其餘的屬性輸出取決於 `type`，這在底下會更詳細地說明。除了 `type` 以外的所有屬性都是陣列。並非所有屬性都會用於所有類型。

**類別** `SoftLayer_Container_Network_CdnMarketplace_Metrics`：
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

PIECHART 類型適用於將傳送至圓餅圖的資料。在此情況下，只會用到下列屬性：`names`、`xaxis` 及 `yaxis`。`names` 屬性是包含地區名稱的陣列。`xaxis` 屬性包含以 GB 為單位的用量，而 `yaxis1` 則是測試百分比。


LINEGRAPH 類型適用於將傳送至折線圖的資料。在此情況下，`names` 陣列將包含 `xaxis`、`yaxis`、`yaxis2`...`yaxis20` 屬性中找到的度量值名稱，而實際資料將在對應的陣列中。


如果 `type` 是 TOTALS，則 `names` 陣列將包含對應 `totals` 陣列中找到的度量值名稱，而實際資料將包含在 `totals` 陣列中。
