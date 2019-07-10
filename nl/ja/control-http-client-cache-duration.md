---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# キャッシュ制御を使用した HTTP クライアントのキャッシュ期間の制御
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

CDN を使用する場合、以下の 2 つのレベルのキャッシングが使用可能です。

  * **エッジでのキャッシング**は、CDN エッジ・サーバーがオリジンからのコンテンツをキャッシュに入れるときに発生します。
  * サーバーのエッジ・ネットワークから**下流へのキャッシング**は、要求側ブラウザーなど、エンド・ユーザーや HTTP クライアントが、エッジ・サーバーからのコンテンツをキャッシュに入れるときに発生します。

ブラウザーなどの要求側にコンテンツをキャッシュしておく期間を制御するために選択するメソッドは、以下の要因で決まります。

  * [ヘッダーの尊重 (Respect Header) の設定](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details)が ON か OFF か。デフォルトでは、ON に設定されています。
  * オリジン・サーバーが、特定のコンテンツの Cache-Control ヘッダーで `max-age` 値を提供しているかどうか。 

エッジ・サーバーに、対象のコンテンツの Cache-Control ヘッダーを含む HTTP 応答を送信させる場合、これらの要因の変化に関係なく、オリジンは、対象コンテンツの Cache-Control ヘッダーをエッジに提供する必要があります。

基本的に、エッジ・サーバーから下流へと送信される Cache-Control ヘッダーは、エッジ・サーバーによって指定されたキャッシングのディレクティブまたは値に従って、関連するコンテンツをキャッシュに入れるように要求側に依頼します。

## ヘッダーを尊重 (Respect Header): Off
{: #respect-header-off}

オリジンが特定のコンテンツに対しての、`max-age` ディレクティブおよび値を指定した Cache-Control ヘッダーを提供する場合でも、エッジにキャッシュされている特定のコンテンツのキャッシュ期間は、CDN の TTL 設定から取得されます。 さらに、エッジ・サーバーは、次のうち少ない方の Cache-Control `max-age` 値を使用して、下流の要求側に応答します。
  * オリジンの Cache-Control `max-age` 値。
  * エッジでコンテンツが失効するまでの残された時間。

ただし、オリジンが Cache-Control ヘッダーをエッジ・サーバーに提供しない場合、エッジ・サーバーは Cache-Control ヘッダーを要求側に提供しません。 その場合、コンテンツのエッジ・キャッシュ期間は、CDN の TTL 設定から取得されます。

## ヘッダーを尊重 (Respect Header): On
{: #respect-header-on}

オリジンが特定のコンテンツに対しての、`max-age` を指定した Cache-Control ヘッダーを提供する場合、オリジンの Cache-Control `max-age` 値が、エッジにキャッシュされているその特定のコンテンツのキャッシュ期間になり、そのコンテンツのすべての適用可能 TTL 設定をオーバーライドします。 さらに、エッジは、エッジ・サーバーでコンテンツが失効するまでに残された時間に等しい Cache-Control `max-age` 値を使用して要求側に応答します。

ただし、オリジンが Cache-Control ヘッダーをエッジ・サーバーに提供しない場合、エッジ・サーバーは Cache-Control ヘッダーを要求側に提供しません。 その場合、コンテンツのエッジ・キャッシュ期間は、CDN の TTL 設定から取得されます。

## 要約
{: #summary}

|ヘッダーを尊重 (Respect Header)|オリジンが Cache-Control を提供|エッジ・サーバー上の特定のコンテンツのキャッシュ期間|エッジ・サーバーが Cache-Control を提供|
|---|---|---|---|
|On|はい。オリジンが `max-age` を指定します|オリジンの `max-age` 値によってオーバーライドされるエッジ・キャッシュ期間|はい。エッジはまた、エッジがオリジンからのコンテンツをリフレッシュする必要が生じるまでの (オーバーライドされた) 時間である値を持つ `max-age` も提供します|
|On|はい。ただし、オリジンは `max-age` を指定しません|CDN の TTL 構成に基づくエッジ・キャッシュ期間|はい。エッジは、エッジがオリジンからのコンテンツをリフレッシュする必要が生じるまでの時間である値を持つ `max-age` も提供します|
|On|いいえ|CDN の TTL 構成に基づくエッジ・キャッシュ期間|いいえ|
|Off|はい。オリジンが `max-age` を指定します|CDN の TTL 構成に基づくエッジ・キャッシュ期間|はい。エッジは、オリジンの `max-age` 値と、エッジがオリジンからのコンテンツをリフレッシュする必要が生じるまでの時間のうち、小さい方の `max-age` 値も提供します。|
|Off|はい。ただし、オリジンは `max-age` を指定しません|CDN の TTL 構成に基づくエッジ・キャッシュ期間|はい。エッジは、エッジがオリジンからのコンテンツをリフレッシュする必要が生じるまでの時間である値を持つ `max-age` も提供します|
|Off|いいえ|CDN の TTL 構成に基づくエッジ・キャッシュ期間|いいえ|

## キャッシュ・コントロールの詳細情報
{: #more-information-on-cache-control}

* [CDN の管理](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn)方法
* Cache-Control ([RFC 2616 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ietf.org/rfc/rfc2616.txt) のセクション 14.9 で定義)
