---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# 度量值的容器类
{: #container-class-for-metrics}

`SoftLayer_Container_Network_CdnMarketplace_Metrics` 集合包含度量值 API 使用的属性。`type` 属性有三个可能的值：PIECHART、LINEGRAPH 或 TOTALS。此容器中其余属性的输出取决于 `type`，这将在下面进一步详细描述。除 `type` 以外的其他所有属性都是数组。并非所有属性都用于所有类型。

`SoftLayer_Container_Network_CdnMarketplace_Metrics` **类**：
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

PIECHART 类型用于将发送到饼图的数据。在这种情况下，仅使用属性：`names`、`xaxis` 和 `yaxis`。`names` 属性是包含区域名称的数组。`xaxis` 属性包含用量（以 GB 为单位），`yaxis1` 是测试百分比。


LINEGRAPH 类型用于将发送到折线图的数据。在这种情况下，`names` 数组将包含在 `xaxis`、`yaxis`、`yaxis2`...`yaxis20` 属性中找到的度量值名称，并且实际数据会位于对应的数组中。


如果 `type` 为 TOTALS，那么 `names` 数组将包含在对应的 `totals` 数组中找到的度量值名称，并且实际数据会包含在 `totals` 数组中。
