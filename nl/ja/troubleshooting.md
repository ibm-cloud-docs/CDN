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

# トラブルシューティング
{: #troubleshooting}

本資料では、{{site.data.keyword.cloud}} CDN をトラブルシューティングするためのさまざまな方法について説明します。 サポートに問い合わせる必要がある場合は、お客様の CDN の参照番号を提供してください。

## CDN が動作していることは、どのようにすればわかりますか
{: #how-do-I-know-my-cdn-is-working}

以下の `curl` コマンドを実行します。その際、`http://your.cdn.domain/uri` を、ご使用の CDN 上の該当するファイル・パスに置き換えてください。

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

`curl` コマンドの出力が以下のサンプル・フォーマットと同様であれば、CDN は予期したとおりに動作しています。

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

## 503 エラーを受け取りました。 なぜですか?
{: #i-received-a-503-error-why}

503 エラーが表示される最も一般的な理由は、SSL 証明書チェーン内の証明書に関する問題です。

「`503 サービス使用不可`」といったエラーが表示されることがあります。  

503 エラーとともに、`「要求の処理中にエラーが発生しました。参照 #30.3598c0ba.1521745157.87201fff」` (実際の参照番号はそれぞれ異なります) と似たメッセージも表示される場合があります。 この場合、エラー・ストリング内の参照番号は、SSL ハンドシェーク障害に変換されます。

この問題を修正するには、オリジン・サーバーの SSL 証明書が以下の基準を満たしていることを確認します。
  * 証明書は、Akamai が信頼する認証局によって**発行されていなければ**なりません。 Akamai が信頼する証明書のリストは、[このリンク![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")] (https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates) で確認できます。
  * CDN で構成される*ホスト・ヘッダー* と**一致しなければ**なりません。
  * 自己署名で**あってはなりません**。
  * 有効期限が**切れていてはなりません**。

オリジンの証明書チェーンが前記の基準に従っていることを確認しても同じエラーが発生する場合は、[ヘルプとサポートの利用](/docs/infrastructure/CDN?topic=CDN-gettinghelp)のページを参照してください。 エラー・ストリングにある参照番号をメモし、IBM との通信にはそれを含めてください。

## IBM Cloud オブジェクト・ストレージ (COS) がオリジンの場合、ホスト名ではブラウザーにロードされません。
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

{{site.data.keyword.cloud_notm}} CDN が、COS をオブジェクト・ストレージとして使用するように構成されている場合、Web サイトへの直接アクセスは機能しません。ブラウザーのアドレス・バーに完全な要求パス (例えば、`www.example.com/index.html`) を指定する必要があります。 このような動作は、IBM COS の索引ドキュメントの制限が原因です。

## HTTPS で、ホスト名を使用して `curl` コマンドまたはブラウザーから接続できません。
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

ワイルドカード証明書を指定し、HTTPS を使用して CDN が作成された場合、接続には CNAME を使用する必要があります。例えば、`https://www.exampleCname.cdnedge.bluemix.net` などです。 これには、2018 年 6 月 18 日より前に、HTTPS を使用して作成された**すべて**の CDN が含まれます。 ホスト名を使用して接続を試行すると、エラーになります。

## サポートされるプロトコルについて、ブラウザーで CNAME またはホスト名をロードする際に予期される動作は何ですか
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

以下の表に、Web ブラウザーから**ホスト名**または **CNAME** をロードする際、サポートされるプロトコルで予期される動作を示します。

<table>
<caption caption-side=“top”>予期される動作の表</caption>
<thead>
<tr>
<th rowspan=2 scope="col">ブラウザー URL</th>
<th rowspan=2 scope="col">HTTP プロトコルのみを使用する CDN</th>
<th colspan=2 scope="col">HTTPS プロトコルのみを使用する CDN</th>
<th colspan=2 scope="col">HTTP と HTTPS の両方のプロトコルを使用する CDN</th>
</tr>
<tr>
<th scope="col"> ワイルドカード </th>
<th scope="col"> 共有 SAN </th>
<th scope="col"> ワイルドカード </th>
<th scope="col"> 共有 SAN </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> 正常なロード </td>
<td> 301 永続的に移動 </td>
<td> 正常なロード </td>
<td> 301 永続的に移動 </td>
<td> 正常なロード </td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> アクセス拒否 </td>
<td> IBM Cloud Web ページにリダイレクト </td>
<td> 正常なロード </td>
<td> IBM Cloud Web ページにリダイレクト </td>
<td> 正常なロード </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 永続的に移動 </td>
		<td> 正常なロード </td>
		<td> 301 永続的に移動 </td>
		<td> 正常なロード </td>
		<td> 301 永続的に移動 </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> IBM Cloud Web ページにリダイレクト </td>
		<td> 正常なロード </td>
		<td> 301 永続的に移動 </td>
		<td> 正常なロード </td>
		<td> IBM Cloud Web ページにリダイレクト </td>
</tr>
</tbody>
</table>

**共通エラー・メッセージ:**

`「301 永続的に移動」`メッセージは、ほとんどの場合、ホスト名を使用して `HTTPS` プロトコルまたは `HTTP_AND_HTTPS` プロトコルで CDN に到達しようとしていることを示しています。 HTTPS ワイルドカード証明書の制限のために、CDN へのアクセスには CNAME を使用**しなければ**なりません。

HTTP **のみ**のプロトコルでは、CNAME を使用して CDN に到達しようとした場合に`「301 永続的に移動」`メッセージを受け取ります。 この場合は、ホスト名を使用した場合_のみ_ CDN にアクセスできます。

`「アクセス拒否」`メッセージは、誤ったプロトコルを使用して CDN に到達しようとしているときに表示されます。 HTTP プロトコルで作成された CDN には `http` を使用し、HTTPS プロトコルで作成された CDN には `https` を使用してください。

**考えられるリダイレクト・エラー:**

URL が {{site.data.keyword.cloud_notm}} CDN Web ページにリダイレクトされる最も一般的な原因は、指定された URL が目的の CDN プロトコルに対して正しくないためです。CDN が、HTTPS または HTTP_AND_HTTPS のプロトコルで作成された場合、CDN へのアクセスには CNAME を使用しなければなりません。 例えば、HTTPS マッピングの場合は `https://examplecname.cdnedge.bluemix.net`、HTTP_AND_HTTPS マッピングの場合は `http://examplecname.cdnedge.bluemix.net` または `https://examplecname.cdnedge.bluemix.net` にします。

このような場合、プロトコルとドメインの両方が CDN のプロトコルに対して正しくないため、URL は {{site.data.keyword.cloud_notm}} CDN Web ページにリダイレクトされます。HTTP を_唯一_ のプロトコルとして作成された CDN の場合、到達できる方法はホスト名_のみ_ です。 例えば、`http://example.com` です。
