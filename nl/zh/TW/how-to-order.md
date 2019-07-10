---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# 訂購 CDN
{: #order-a-cdn}

在這裡您將瞭解如何訂購 Content Delivery Network (CDN)。您的 CDN 可以從 [{{site.data.keyword.cloud}} 主控台](https://cloud.ibm.com/login)訂購。

## 準備訂購
{: #preparation-for-ordering}

下列說明如何導覽至 CDN 頁面以下訂單。

**步驟 1**

* 從 [IBM Cloud 主控台](https://cloud.ibm.com/login)登入您的帳戶

**步驟 2**

按一下 [IBM Cloud 型錄](https://cloud.ibm.com/catalog/)。從左側的導覽列中，選取**網路**。

   ![IBM Cloud CDN 導覽](images/bluemix_navigation.png)

**步驟 3**

按一下 **CDN** 磚。

   ![IBM Cloud CDN 圖示](images/bluemix_tile.png)


## 訂購新的 CDN
{: #order-a-new-cdn}

進入訂購頁面之後，下列說明如何繼續建立及配置您的 CDN。

### 步驟 1：建立 CDN 帳戶
{: #create-your-cdn-account}

按一下右下角的**建立**，這會建立您的 CDN 帳戶（如果您尚未建立的話，並將您重新導向「CDN 配置」畫面。

   ![CDN 概觀](images/content-delivery.png)

### 步驟 2：命名 CDN
{: #step-2-name-your-cdn}

填寫**配置名稱**欄位：  

  * 指定**主機名稱**（**必要項目**），以提供作為 CDN 的主要 ID（例如，`example.testingcdn.net`）。  
  * 您可以選擇性地提供自訂 **CNAME**（例如 `myfirstcdn.cdnedge.bluemix.net`）。如果未提供任何 CNAME，則會為您建立一個 CNAME。字尾 `cdnedge.bluemix.net` 會自動附加至 CNAME。使用不適當的 CNAME 可能導致服務終止。

       ![配置名稱](images/configure-hostname-cname.png)  

佈建您的新 CDN 之後，您**必須**與 DNS 提供者一起配置 CNAME。
{: note}
### 步驟 3：配置原點
{: #step-3-configure-your-origin}

填寫**配置原點**欄位：若要配置此欄位，您必須選取**伺服器**或 **Object Storage** 選項  

  * **步驟 3，選項 1：伺服器選項**

     ![配置原點](images/configure-origin-server.png)

      * 您必須指定**原點伺服器位址**（原點伺服器的主機名稱或 IPv4 位址）。如果也選取了 **HTTPS 埠**，則**原點伺服器位址**必須是主機名稱，而非 IP 位址。

      * 指定**主機標頭**（選用）。如果未提供，將預設為**主機名稱**。如需主機標頭的相關資訊，請參閱[主機標頭支援](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support)的特性說明。  

      * 提供**路徑**，可以從原點上擷取該處的內容（選用）。請參閱[路徑型 CDN 對映](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings)的特性說明，以瞭解在此時新增路徑的含意。

      * 您可能也會提供 **HTTP 埠**及（或）**HTTPS 埠**這些欄位指出可以使用哪些通訊協定及埠號來與「原始伺服器」通訊。若為非預設的埠號，請參閱[常見問題 (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) 以取得接受的埠號清單。

      * **SSL 憑證**：_只_ 有在選取「HTTPS 埠」時，才會出現這個選項。如果您針對「伺服器」或 Object Storage 選取 **HTTPS 埠**，則可以選擇**萬用字元**或 **DV SAN 憑證**作為「SSL 憑證」選項。兩者都提供 HTTPS 所提供的加強型安全。
        * **萬用字元憑證**只有在使用 **CNAME** 時才容許 HTTPS 資料流量，您並不需要採取任何進一步動作。
        * **DV SAN 憑證**容許在您的網域上有 HTTPS 資料流量，但需要其他步驟來進行驗證。請參閱[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https)頁面，以瞭解選擇這個選項時牽涉到的必要步驟和時間限制。

	     ![配置原點伺服器](images/ssl-cert-options.png)

  * **步驟 3，選項 2：Object Storage 選項**

    ![配置 Object Storage](images/configure-origin-object-storage.png)

      * 您**必須**指定要從中提取物件的**端點**。

      * 指定**主機標頭**（選用）。如果未提供，將預設為**主機名稱**。如需主機標頭的相關資訊，請參閱[主機標頭支援](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support)的特性說明。  

      * 提供**路徑**，可以從原點上擷取該處的內容（選用）。請參閱[路徑型 CDN 對映](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings)的特性說明，以瞭解在這裡新增路徑的含意。

      * 您**必須**提供儲存內容的**儲存區**名稱。

      * 您可能也會提供 **HTTP 埠**及（或）**HTTPS 埠**這些欄位指出可以使用哪些通訊協定及埠號來與「原始伺服器」通訊。若為非預設的埠號，請參閱[常見問題 (FAQ)](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) 以取得接受的埠號清單。

      * **SSL 憑證**：_只_ 有在選取「HTTPS 埠」時，才會出現這個選項。如果您針對「伺服器」或 Object Storage 選取 **HTTPS 埠**，則可以選擇**萬用字元**或 **DV SAN 憑證**作為「SSL 憑證」選項。兩者都提供 HTTPS 所提供的加強型安全。
        * **萬用字元憑證**只有在使用 **CNAME** 時才容許 HTTPS 資料流量，您並不需要採取任何進一步動作。
        * **DV SAN 憑證**容許在您的網域上有 HTTPS 資料流量，但需要其他步驟來進行驗證。請參閱[完成 HTTPS 的網域控制驗證](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https)頁面，以瞭解選擇這個選項時牽涉到的必要步驟和時間限制。

        ![配置 HTTPS](images/ssl-cert-options.png)

您必須與雲端物件儲存空間提供者一起，為儲存區中的每個物件，將**存取控制清單** (ACL) 設為 "public-read"。
{: note}
      
### 步驟 4
{: #step-4}

* 在**建立**按鈕上，您必須選取右下方的**我已閱讀「主要服務合約」並同意上述條款**。

* 然後選取右下角的**建立**按鈕，以建立您的 CDN。
