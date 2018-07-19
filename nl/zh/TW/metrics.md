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

# 度量值

## 如何檢視度量值及使用情形？

您可以在**概觀**頁面上檢視度量值及使用情形，從 **Content Delivery Networks** 頁面中選取 CDN 即可到達此頁面。**附註**：在您建立 CDN 之後，最多可能需要 48 個小時，才會顯示度量值。

## 我建立了 CDN 而且有資料流量通過 CDN。為什麼我的度量值未出現？

建立 CDN 之後，需要 48 小時度量值才會出現。


## 我最少可以檢視度量值幾天？有上限嗎？

您可以使用 IBM Cloud Content Delivery Network 與 Akamai 檢視的度量值有天數的上限和下限。最少可以收集度量值 7 天。最多可以檢視度量值 90 天。如果是使用 API，則建議使用 89 天作為上限，以說明任何時區差異。

## 為什麼當總命中數為零時，命中率不是零？
命中率代表從 Edge Server 快取遞送內容，而不是從原點伺服器遞送內容的時間百分比。計算方式如下：

> ((Edge hits - Ingress hits)/Edge hits) * 100



其中：Edge hits 定義為「從一般使用者對 Edge Server 的所有命中」  
Ingress hits 定義為「原點或進入的命中是針對從您的原點到 Akamai Edge Server 的資料流量」

因為命中率的計算是在帳戶層次，而不是針對 CDN，所以在您帳戶中的所有 CDN 命中率都會相同。這一點也解釋了為何當特定 CDN 的邊緣命中數為零時，命中率可能不是零。

