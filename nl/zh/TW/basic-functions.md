---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: running status, additional steps, stop cdn, learn, configure cname, delete cdn, start cdn

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:warning: .warning}
{:download: .download}

# 讓 CDN 成為執行中狀態
{: #getting-your-cdn-to-running-status}

瞭解如何遵循這些準則，讓 CDN 進入 RUNNING 狀態。此文件也會告訴您如何啟動和停止 CDN。

## 進入執行中
{: #get-to-running}

在建立 CDN 之後，它就會出現在 CDN 儀表板上。您可以在這裡看到 CDN 名稱、「原點」、「提供者」及狀態。  

 ![對映清單擷取畫面](images/mapping-list.png)


如果您訂購的 CDN 具有使用「萬用字元」憑證的 HTTP 或 HTTPS，則可以繼續「步驟 1」。

如果您已建立具有 HTTPS DV SAN 憑證的 CDN，則可能需要執行其他步驟來驗證網域，而這些步驟可以在[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https)頁面上找到。

**步驟 1：**

在您訂購 CDN 之後，需要與 DNS 提供者一起配置 **CNAME**。大部分 DNS 提供者都會指示您如何設定或變更 CNAME。

   * 在此期間，您的 CDN 狀態會顯示為 **CNAME 配置**。請與 DNS 提供者聯繫，以找出變更何時生效。

   ![CNAME 配置](images/cname-config.png)  

**步驟 2：**

在您與 DNS 提供者一起配置 CNAME 之後，隨時都可以透過從 CDN 狀態右側的「溢位」功能表中選取**取得狀態**來檢查狀態。

  ![CNAME 取得狀態](images/cname-getstatus.png)  

**步驟 3：**

CNAME 鏈結完成後，選取**取得狀態**會將狀態變更為 *RUNNING*，並且可以開始使用 CDN。

恭喜！您的 CDN 現在正在執行。在這裡，[管理 CDN](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#manage-your-cdn) 頁面包含關於配置選項（例如[存活時間](docs/infrastructure/CDN?topic=CDN-manage-your-cdn#setting-content-caching-time-using-time-to-live-)、[清除快取的內容](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#purging-cached-content)及[新增原點路徑詳細資料](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#adding-origin-path-details)）的其他資訊。

## 啟動 CDN
{: #starting-cdn}

啟動 CDN 會通知 DNS 將來自您原點的資料流量導向 Akamai 邊緣伺服器。啟動對映之後，DNS 快取可能仍會將資料流量指引到原點，因此網域可能不會在啟動對映之後立即看到此功能。更新所需的時間取決於 DNS 快取重新整理的頻率，且會視您的 DNS 提供者而變。

只有在 CDN 處於 `Stopped` 狀態時，才能啟動 CDN。
{: note}

**步驟 1：**

從「溢位」功能表（此功能表顯示為 CDN 列右側的三個點）中，按一下**啟動 CDN**。

  ![「溢位」功能表](images/start_cdn.png)

**步驟 2：**

即會出現較大的對話框視窗，要求確認您要啟動此服務。選取**確認**以繼續。

**步驟 3：**

如果動作成功，則對話框會出現在畫面右上角，讓您知道它已成功，並且附有時間。

**步驟 4：**

這個步驟會將「狀態」變更為 `CNAME Configuration`

**步驟 5：**

從「溢位」功能表中，按一下**取得狀態**。這個步驟會將狀態變更為 `Running`。您的 CDN 將變成可操作。

## 停止 CDN
{: #stopping-a-cdn}

STOP CDN 功能是要讓維護時間不超出 7 天。在 7 天之後，必須啟動 CDN，否則將予以停用，而且不會將流向 CDN CNAME 的資料流量重新導向至原點伺服器。
{: important}

對映停止之後，DNS 查閱會切換至原點。資料流量會跳過 CDN 邊緣伺服器，而內容會直接從原點提取。對映停止之後，可能會有短暫的期間無法存取您的內容。這是因為 DNS 快取仍然可能將資料流量導向 Akamai 邊緣伺服器。不過，在這段時間內，Akamai 邊緣伺服器會拒絕網域的資料流量。這段期間持續的長度取決於 DNS 快取重新整理的頻率，且會視您的 DNS 提供者而變。

只有在處於 `Running` 狀態時，才能停止 CDN。
{: note}

針對配置「HTTPS SAN 憑證」的 CDN **不建議**停止 CDN，因為當您將 CDN 移回 `Running` 狀態時，HTTPS 資料流量可能無法運作。
{: important}

目前**不容許**停止萬用字元網域的 CDN。
{: important}

**步驟 1：**

從「溢位」功能表（CDN 狀態右側的 3 個垂直點）中，按一下「停止 CDN」。
 ![「溢位」功能表](images/stop_cdn.png)

**步驟 2：**

即會出現較大的對話框視窗，要求您確認要停止此服務。選取**確認**以繼續。

**步驟 3：**

大約 5 到 15 秒之後，狀態應該會變更為「已停止」

## 刪除 CDN
{: #deleting-a-cdn}

若要刪除 CDN，請遵循下列步驟：

從「溢位」功能表中選取`刪除`，只會刪除 CDN，並不會刪除您的帳戶。
{: note}

**步驟 1：**

從「溢位」功能表中，按一下「刪除」。

 ![刪除 CDN「溢位」功能表](images/delete_cdn.png)

**步驟 2：**

即會出現較大的對話框視窗，要求確認您要刪除。按一下**刪除**以繼續。

如果 CDN 是使用具有「DV SAN 憑證」的 HTTPS 所配置，則最多可能需要 5 小時才能完成此刪除處理程序。
{: note}

  ![刪除，但發出警告](images/delete-with-warning.png)

**步驟 3：**

完成步驟 1 和 2 之後，您的 CDN 狀態將是`刪除中`。刪除處理程序完成後，再按一次溢位功能表中的「取得狀態」，以便從 CDN 清單中移除列。如果刪除處理程序尚未完成，則此動作沒有作用。
