---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-25"

keywords: hotlink, protection, class, behavior, API, valid string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 快速鏈結保護類別
{: #hotlink-protection-class}

`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 類別包含「快速鏈結保護 API」所使用的屬性。此物件用來透過呼叫 API 的方法，來設定 CDN 的「快速鏈結保護」行為。順利呼叫 API 之後，「快速鏈結保護 API」也會傳回該物件。

**類別** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`：

* **不**支援 URL 的第一個標籤中的 `://`。
   * **有效**：`http*www.example.com`
   * **無效**：`http://www.example.com`

* `protectionType`：指定以在 HTTP 要求有一個「Referer 標頭」值符合 `refererValues` 中的其中一個項目時，容許或拒絕內容的存取。沒有相符的項目時，便會發生相反的狀況。
  * protectionType 的可能值：
    * `ALLOW`
    * `DENY`
* `refererValues`：是以單一空格區格的參照端 URL 清單，可能也有萬用字元相符項，格式為字串。
  * `refererValues` 的一些**有效**字串範例：
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * `refererValues` 的一些**無效**字串範例：
   
      |**無效範例的說明**| 範例
      |-------|-----|
      |包含超過 2100 個字元（總計）|`www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |包含至少一個大於 255 個字元的 URL 相符項目| `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |包含清單中的重複項目|`domain1.example.com domain1.example.com`|
      |空的 refererValues| ` `|
      |使用 [RFC-3986 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://tools.ietf.org/html/rfc3986#section-2) 中未指定的字元|`domain1.exa}mple.com domain1.example.com`|
      |不支援 `&` 字元|`www.example.com/path&`|
      |不支援 `refererValues` 字串（該字串含有至少一個 URL 相符項目，其中，第一個 `.` 字元之前的字集包含 `://`）|`www.example.org http://www.example.com`|


