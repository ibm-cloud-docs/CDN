---

copyright:
  years: 2017,2018
lastupdated: "2018-06-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 訂購 CDN

在這裡您將瞭解如何訂購 Content Delivery Network (CDN)。您可以從[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 或 [Bluemix 入口網站](https://www.ibm.com/cloud-computing/bluemix/)訂購 CDN。

## 從控制入口網站，請執行下列動作：

**步驟 1：**

若要開始，請使用唯一認證來登入[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/)。

**步驟 2：**

從顯示畫面頂端的導覽列中，選取**網路 -> CDN**。

   ![「網路」功能表選項](images/network-cdn.png)

**步驟 3：**

在 **Content Delivery Networks** 頁面上，選取右上角的**訂購 CDN** 按鈕。

   ![選取訂購 CDN](images/order-cdn-button.png)

## 從「IBM Cloud 入口網站」，請執行下列動作：

**步驟 1：**

登入 [IBM Cloud 入口網站](https://www.ibm.com/cloud-computing/bluemix/)

**步驟 2：**

按一下 [IBM Cloud 型錄](https://console.bluemix.net/catalog/)。從左側的導覽列中，選取**網路**。

   ![Bluemix CDN 導覽](images/bluemix_navigation.png)

**步驟 3：**

按一下 **CDN 磚**，以帶領您前往「供應商選項」畫面。

   ![Bluemix CDN 圖示](images/bluemix_tile.png)


**步驟 4：**

從**選取 CDN 提供者**畫面中，選擇 CDN 提供者選項。按一下**選取**按鈕，以確認您選取的選項，然後按一下畫面右下方的**下一步**，以開始佈建處理程序。  
       ![選取 CDN 提供者](images/Vendor_Select_And_Provision.png)

**步驟 5：**

填寫**配置名稱**欄位：  

  * 指定**主機名稱**（**必要項目**），以提供作為 CDN 的主要 ID（例如，_example.testingcdn.net_）。  
  * 您可以選擇性地提供自訂 **CNAME**（例如 _myfirstcdn.cdnedge.bluemix.net_）。如果未提供任何 CNAME，則會為您建立一個 CNAME。<要在這裡併入驗證資訊>  

       ![配置名稱](images/configure-hostname-cname.png)  

    **附註**：使用不適當的 CNAME 可能導致服務終止。

**步驟 6：**

填寫**配置原點**欄位：若要配置此欄位，您必須選取**伺服器**或 **Object Storage** 選項  

   * 指定**主機標頭**（選用）。如果未提供，將預設為**主機名稱**。如需主機標頭的相關資訊，請參閱[主機標頭支援](about.html#host-header-support-)的特性說明。  

   * 提供**路徑**（選用）。路徑應該相對於**主機名稱**。

      ![配置原點](images/configure-origin.png)  

  * **伺服器選項**：如果您選取**伺服器**選項，請輸入應該從中快取資料的「原點伺服器」的主機名稱或 IP 位址。
      * 如果您選取此選項，則必須指定**原點伺服器位址**（原點伺服器的主機名稱或 IPv4 位址）。如果選取 **HTTPS 埠**，**原點伺服器位址**必須是主機名稱，而非 IP 位址。
      * 您可能也會提供 **HTTP 埠**及（或）**HTTPS 埠**這些欄位指出可以使用哪些通訊協定及埠號來與「原始伺服器」通訊。若為非預設的埠號，請參閱[常見問題 (FAQ)](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai) 以取得接受的埠號清單。
      * **SSL 憑證**：_只_ 有在選取「HTTPS 埠」時，才會出現這個選項。\*「Object Storage 選項」說明後面會有 HTTPS 及 SSL 憑證配置的其他資訊。

	     ![配置原點伺服器](images/configure-origin-server.png)

  * **Object Storage 選項**：如果您選取 **Object Storage** 選項，則必須提供下列資訊：
      * 從中提取「物件」的**端點**、
      * 在其中儲存內容的**儲存區**名稱，以及
      * **HTTPS 埠**。
      * **SSL 憑證**：_只_ 有在選取「HTTPS 埠」時，才會出現這個選項。\*「Object Storage 選項」說明後面會有 HTTPS 及 SSL 憑證配置的其他資訊。
      * 您也可以指定可用於 CDN 服務的副檔名（以逗點區隔）（如果未指定任何副檔名，則會接受所有副檔名）。
      * 您必須將**儲存區**中每一個**物件**的**存取控制清單** (ACL) 設為 "public-read"。

      	  ![配置物件儲存空間](images/configure-origin-cos.png)

  * **SSL 憑證**：如果您針對「伺服器」或 Object Storage 選取 **HTTPS 埠**，則可以選擇**萬用字元**或 **DV SAN 憑證**作為 **SSL 憑證**選項。兩者都提供 HTTPS 所提供的加強型安全。
    * **萬用字元憑證**只有在使用 **CNAME** 時才容許 HTTPS 資料流量，您並不需要採取任何進一步動作。
    * **DV SAN 憑證**容許在您的網域上有 HTTPS 資料流量，但需要其他步驟來進行驗證。

        ![配置 HTTPS](images/configure-https.png)


**步驟 7：**

配置**其他選項**欄位：本節包含**遵循標頭**欄位的配置選項。

   * **遵循標頭**選項是**開啟**時，「原點」在標頭中所定義的 TTL 設定將置換預設 CDN TTL。依預設，**遵循標頭**會設為**開啟**，但您必須配置此欄位。  

        ![其他選項](images/other-options.png)

**步驟 8：**

選取右下角的**建立**按鈕，以建立您的 CDN。
