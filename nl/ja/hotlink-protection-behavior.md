---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-19"

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

`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking class` には、ホット・リンク保護 API によって使用される属性が含まれています。 このオブジェクトは、この API を呼び出して CDN のホット・リンク保護の動作を設定するために使用されます。  また、正常な API 呼び出しの後にホット・リンク保護 API によって返されます。

**クラス** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`:

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
    * `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...
      * 2100 を超える文字が含まれている
    * `domain1.example.com domain1.example.com`
      * リスト内に重複が含まれている
    * ` `
      * 空の refererValues
    * `domain1.exa}mple.com domain1.example.com`
      * [RFC-3986 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://tools.ietf.org/html/rfc3986#section-2) に指定されていない文字を使用している
    * `www.example.com/path&`
      * `&` 文字はサポートされない
    * `www.example.org http://www.example.com`
      * 最初の `.` 文字の前に `://` を含む文字セットを持つ少なくとも 1 つの URL 一致語がある `refererValues` ストリングはサポートされない
