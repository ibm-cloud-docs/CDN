---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 如何訂購 CDN

在這裡您將瞭解如何訂購 Content Delivery Network (CDN)。您可以從[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/) 或 [Bluemix 型錄入口網站](https://www.ibm.com/cloud-computing/bluemix/) 訂購 CDN。

## 從控制入口網站，請執行下列動作：

1. 若要開始，請使用唯一認證來登入[客戶入口網站 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/)。

2. 從顯示畫面頂端的導覽列中，選取**網路 -> CDN**。

   ![「網路」功能表選項](images/network-cdn.png)

3. 在 **Content Delivery Networks** 頁面上，選取右上角的**訂購 CDN** 按鈕。

	![選取訂購 CDN](images/order-cdn-button.png)

## 從「Bluemix 入口網站」，請執行下列動作：

1. 登入 [Bluemix 入口網站](https://www.ibm.com/cloud-computing/bluemix/)

2. 按一下 **IBM Bluemix 型錄**。從左側的導覽列中，選取**網路**。

   ![Bluemix CDN 導覽](images/bluemix_navigation.png)

3. 按一下 **CDN 磚**，以帶領您前往「供應商選項」畫面。

   ![Bluemix CDN 圖示](images/bluemix_tile.png)

4. 從**選取 CDN 提供者**畫面中，選擇 CDN 提供者選項。按一下畫面底端的**已選取**按鈕，以確認您選取的選項，然後按一下**開始佈建**，以開始佈建處理程序。

	![選取 CDN 提供者](images/newReducedSizeVendorSelectAndProvision.png)
	
5. 填寫**配置名稱**欄位： 
      * 指定 _hostname_（**必要項目**），以提供作為 CDN 的主要 ID（例如，_example.testingcdn.net_）。
      * 您可以選擇性地提供自訂 _CNAME_（例如 _myfirstcdn.cdnedge.bluemix.net_）。如果未提供任何 CNAME，則會為您建立一個 CNAME。<要在這裡併入驗證資訊>
      
      ![配置名稱](images/configure-hostname-cname.png)
		
6. 填寫**配置原始**欄位：若要配置此欄位，您必須選取**伺服器**或 **Object Storage** 選項（指定**路徑**是選用的。<驗證資訊>）。
		
  * **伺服器選項**：如果您選取**伺服器**選項，請輸入應該從中快取資料的「原始伺服器」的主機名稱或 IP 位址。 
      * 如果您選取此選項，則必須指定「原始伺服器」的 **IP 位址**。
      * 您可能也會提供 **HTTP 埠**及（或）**HTTPS 埠**（這些欄位指出可以使用哪些通訊協定及埠號來與「原始伺服器」通訊）。

	   ![配置原始伺服器](images/configure-origin-server.png)
		
  * **Object Storage 選項**：如果您選取 **Object Storage** 選項，則必須提供下列資訊：
      * 從中提取「物件」的**端點**、
      * 在其中儲存內容的**儲存區**名稱，以及
      * **HTTPS 埠**。
      * 您也可以指定可用於 CDN 服務的副檔名（以逗點區隔）（如果未指定任何副檔名，則會接受所有副檔名）。
      * 您必須將**儲存區**中每一個**物件**的**存取控制清單** (ACL) 設為 "public-read"。
		
	   ![配置物件儲存空間](images/configure-origin-object-storage.png)

7. 配置**其他選項**欄位：本節包含**提供過時內容**欄位及**遵循標頭**欄位的配置選項。
    
     * **提供過時內容**欄位：**提供過時內容**是一個布林值，指出無法呼叫原始伺服器時是否提供過期的已快取內容。基於現行限制，依預設會啟用**提供過時內容**功能，不論使用者是將它設為**開啟**還是**關閉**。
     * **遵循標頭**欄位：**遵循標頭**選項是**開啟**時，「原始」在標頭中所定義的 TTL 設定將置換預設 CDN TTL。依預設，**遵循標頭**會設為**開啟**，但您必須配置此欄位。

		![其他選項](images/other-options.png)
		
8. 選取右下角的**建立**按鈕，以建立您的 CDN。
