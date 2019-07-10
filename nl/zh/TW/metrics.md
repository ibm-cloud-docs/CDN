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

# 度量值
{: #metrics}

當您第一次從清單選取 CDN 時，會開啟「概觀」頁面。在這裡您可以看到所選時段（預設值是 30 天）的「頻寬總計」、「總命中數」和「命中率」。

  ![度量值概觀](images/metrics-overview.png)

緊接在概觀下方，您將看到「頻寬總計」、「每個地區的頻寬」、「總命中數」和「依類型排列的命中數」的圖型表示法。

  ![度量值圖形](images/metrics-graphs.png)

**附註**：在您建立 CDN 之後，最多可能需要 48 個小時，才會顯示度量值。

## 我最少可以檢視度量值幾天？有上限嗎？

可以使用 {{site.data.keyword.cloud}} Content Delivery Network 與 Akamai 來檢視度量值的天數有上限和下限。最少可以收集度量值 7 天。最多可以檢視度量值 90 天。如果是使用 API，則建議使用 89 天作為上限，以說明任何時區差異。

## 為什麼當總命中數為零時，命中率不是零？
命中率代表從邊緣伺服器快取遞送內容，而不是從原點伺服器遞送內容的時間百分比。計算方式如下：

`((Edge hits - Ingress hits)/Edge hits) * 100

`

其中：

_Edge hits_ 定義為「從一般使用者對邊緣伺服器的所有命中」。  
_Ingress hits_ 定義為「原點或進入的命中是針對從您的原點到 Akamai 邊緣伺服器的資料流量」。

因為命中率的計算是在帳戶層次，而不是針對 CDN，所以在您帳戶中的所有 CDN 命中率都會相同。這一點也解釋了為何當特定 CDN 的邊緣命中數為零時，命中率可能不是零。

## 度量值會即時更新嗎？

不會。度量值每 24 小時更新一次。
