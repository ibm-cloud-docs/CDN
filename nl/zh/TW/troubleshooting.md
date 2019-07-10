---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 疑難排解
{: #troubleshooting}

在本文件中，您將找到進行 {{site.data.keyword.cloud}} CDN 疑難排解的各種方式。如果您需要與支援中心聯絡，請務必提供 CDN 的參照號碼。

## 如何知道 CDN 正常運作？
{: #how-do-I-know-my-cdn-is-working}

請將 `http://your.cdn.domain/uri` 取代為 CDN 上的個別檔案路徑，以執行下列 `curl` 指令：

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

如果 `curl` 指令的輸出類似於下列範例，則 CDN 會如預期運作：

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```
{: screen}

## 我收到 503 錯誤。為什麼？
{: #i-received-a-503-error-why}

我們最常看見的 503 錯誤原因，是因為 SSL 憑證鏈中的憑證問題。

這是您可能看到的錯誤：`503 Service Unavailable`。  

與 503 錯誤一起，您也可能看到類似如下的訊息：`An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff`（實際參照號碼可能改變）。在此情況下，錯誤字串中的參照號碼會轉換成 SSL 信號交換失敗。

若要更正此問題，請確定您的原點伺服器 SSL 憑證符合下列準則：
  * 憑證**必須**由 Akamai 信任的憑證管理中心發出。您可以檢視 Akamai 信任的憑證管理中心清單，其位於 [這個鏈結 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")]](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)
  * 它**必須**符合 CDN 上配置的*主機標頭*
  * 它**不得**為自簽
  * 它**不得**過期

如果您已使用先前的準則驗證原點的憑證鏈，而仍然遇到相同的錯誤，請參閱[取得協助及支援](/docs/infrastructure/CDN?topic=CDN-gettinghelp)頁面。請記下參照錯誤字串，並將它包含在與我們的任何通訊裡。

## IBM Cloud Object Storage (COS) 為原點時，我的主機名稱無法在瀏覽器上載入。
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

當您的 {{site.data.keyword.cloud_notm}} CDN 配置為使用 COS 作為物件儲存空間時，將無法直接存取網站。您必須在瀏覽器的位址列指定完整要求路徑（例如 `www.example.com/index.html`）。此行為是由 IBM COS 中的索引文件限制所導致。

## 我無法透過 `curl` 指令或瀏覽器，使用主機名稱搭配 HTTPS 來連接。
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

如果您的 CDN 建立時是使用 HTTPS 搭配萬用字元憑證，則必須使用 CNAME 來建立連線，例如 `https://www.exampleCname.cdnedge.bluemix.net`。這包括**所有**在 2018 年 6 月 18 日之前使用 HTTPS 建立的 CDN。嘗試使用主機名稱連接會導致錯誤。

## 在您的瀏覽器上針對支援的通訊協定載入 CNAME 或主機名稱時，預期的行為為何？
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

此表格顯示在從 Web 瀏覽器載入**主機名稱**或 **CNAME** 時，針對支援通訊協定的預期行為。

<table>
<caption caption-side=“top”>預期行為表格</caption>
<thead>
<tr>
<th rowspan=2 scope="col">瀏覽器 URL</th>
<th rowspan=2 scope="col">CDN 僅搭配 HTTP 通訊協定</th>
<th colspan=2 scope="col">CDN 僅搭配 HTTPS 通訊協定</th>
<th colspan=2 scope="col">CDN 同時搭配 HTTP 和 HTTPS 通訊協定</th>
</tr>
<tr>
<th scope="col"> 萬用字元</th>
<th scope="col"> 共用 SAN </th>
<th scope="col"> 萬用字元</th>
<th scope="col"> 共用 SAN </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> 成功載入</td>
<td> 301 永久地移動</td>
<td> 成功載入</td>
<td> 301 永久地移動</td>
<td> 成功載入</td>
</tr>
<tr>
<td> `https://hostname `</td>
<td> 拒絕存取</td>
<td> 重新導向至 IBM Cloud 網頁</td>
<td> 成功載入</td>
<td> 重新導向至 IBM Cloud 網頁</td>
<td> 成功載入</td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 永久地移動</td>
		<td> 成功載入</td>
		<td> 301 永久地移動</td>
		<td> 成功載入</td>
		<td> 301 永久地移動</td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> 重新導向至 IBM Cloud 網頁</td>
		<td> 成功載入</td>
		<td> 301 永久地移動</td>
		<td> 成功載入</td>
		<td> 重新導向至 IBM Cloud 網頁</td>
</tr>
</tbody>
</table>

**一般錯誤訊息：**

`301 Moved permanently` 訊息最可能表示您正在嘗試使用 `HTTPS` 或 `HTTP_AND_HTTPS` 通訊協定搭配主機名稱呼叫到 CDN。由於 HTTPS 萬用字元憑證的限制，您**必須**使用 CNAME 來存取 CDN。

使用**僅限** HTTP 的通訊協定時，如果您嘗試使用 CNAME 呼叫到 CDN，則會收到 `301 Moved permanently` 訊息。在此情況下，您_只能_ 使用主機名稱取得 CDN 的存取。

當您嘗試使用不正確的通訊協定呼叫到 CDN 時，會看到 `Access denied` 訊息。請確定您針對以 HTTP 通訊協定建立的 CDN 使用 `http`，或是針對以 HTTPS 通訊協定建立的 CDN 使用 `https`。

**可能的重新導向錯誤：**

將 URL 重新導向至 {{site.data.keyword.cloud_notm}} CDN 網頁的行為，最常在 URL 對通訊協定而言不正確的時候看到。如果您的 CDN 是使用 HTTPS 或 HTTPS_AND_HTTPS 通訊協定建立，您必須使用 CNAME 來存取 CDN。例如：`https://examplecname.cdnedge.bluemix.net` 適用於 HTTPS 對映，`http://examplecname.cdnedge.bluemix.net` 或 `https://examplecname.cdnedge.bluemix.net` 則適用於 HTTP_AND_HTTPS 對映。

在此情況下，URL 會重新導向至 {{site.data.keyword.cloud_notm}} CDN 網頁，因為通訊協定和網域兩者對於 CDN 的通訊協定而言都不正確。對以使用 HTTP 作為_唯一_ 通訊協定建立的 CDN，_只能_ 藉由主機名稱呼叫到它。例如 `http://example.com`。
