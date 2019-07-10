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

# ホット・リンク保護クラス
{: #hotlink-protection-class}

`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection class` には、ホット・リンク保護 API によって使用される属性が含まれています。このオブジェクトは、この API を呼び出して CDN のホット・リンク保護の動作を設定するために使用されます。  また、正常な API 呼び出しの後にホット・リンク保護 API によって返されます。

**class** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`:

* URL の最初のラベルの `://` はサポート**されません**。
   * **有効**: `http*www.example.com`
   * **無効**: `http://www.example.com`

* `protectionType`: HTTP 要求に `refererValues` 内にあるいずれかの語に一致する Referer ヘッダー値が含まれている場合に、コンテンツへのアクセスを許可または拒否するかを指定します。 一致がないときには、逆のことが行われます。
  * protectionType に指定できる値は以下のとおりです。
    * `ALLOW`
    * `DENY`
* `refererValues`: ワイルドカード・マッチングも含む可能性がある、ストリングの形式のリファラー URL の単一スペース区切りリストです。
  * `refererValues` の**有効な**ストリングの例を以下に示します。
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * `refererValues` の**無効な**ストリングの例を以下に示します。
   
      |**無効な例の説明**| 例
      |-------|-----|
      |2100 を超える文字が含まれている (合計)| `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |255 文字を超える URL 一致語が少なくとも 1 つ含まれている | `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |リスト内に重複が含まれている| `domain1.example.com domain1.example.com`|
      |空の refererValues| ` `|
      |[RFC-3986 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://tools.ietf.org/html/rfc3986#section-2) に指定されていない文字を使用している|`domain1.exa}mple.com domain1.example.com`|
      |`&` 文字はサポートされない|`www.example.com/path&`|
      |最初の `.` 文字の前に `://` を含む文字セットを持つ少なくとも 1 つの URL 一致語がある `refererValues` ストリングはサポートされない|`www.example.org http://www.example.com`|


