---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# バイト範囲要求の処理
{: #working-with-byte-range-requests}

バイト範囲要求を使用して、オリジン・サーバーから部分的コンテンツを取得できます。 この文書は、示される可能性のある応答状況コードを理解するのに役立ちます。

{{site.data.keyword.cloud}} CDN with Akamai を使用して**バイト範囲要求**が送信された場合、最初の要求ではユーザーは `200 (OK)` 応答コードを受け取り、後続のすべての要求では `206` 応答コードを受け取ることがあります。これは、Akamai エッジ・サーバーがオリジンからのコンテンツを圧縮形式で要求することが原因です。 そのため、エッジ・サーバーは、キャッシュ内にオブジェクトを保持せず、オブジェクトのコンテンツ長についての情報も持っていない場合、オリジンに転送してオブジェクト全体を要求します。 それに対して、オリジンは、コンテンツ長ヘッダーなしでオブジェクトを Akamai に提供し、エンド・ユーザーには、これがバイト範囲要求であってもオブジェクト全体が提供されます。 その結果が `200` 状況コードです。 後続の要求では、エッジ・サーバーのキャッシュ内にオブジェクトがあるため、`206` 状況コードが提供されます。

**HTTP 要求の範囲**ヘッダーは、サーバーがコンテンツのどの部分を返すのかを示します。 複数の部分を一度に 1 つの範囲ヘッダーで要求でき、サーバーは、これらの範囲を複数パーツ応答で返送することがあります。 サーバーは範囲を返送する場合、206 (部分的コンテンツ) 状況で応答します。

最初のバイト範囲要求であっても確実に 206 応答が返されるようにする 1 つの方法は、オリジン・サーバーで `Transfer-Encoding: chunked` を無効にすることです。
