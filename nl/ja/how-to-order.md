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

# CDN の注文方法

コンテンツ配信ネットワーク (CDN) を注文する方法について、以下に説明します。CDN の注文は、[カスタマー・ポータル ![「外部リンク」アイコン](../../icons/launch-glyph.svg "「外部リンク」アイコン")](https://control.softlayer.com/) または [Bluemix カタログ・ポータル](https://www.ibm.com/cloud-computing/bluemix/)から行えます。

## コントロール・ポータルから:

1. まず、固有の資格情報を使用して[カスタマー・ポータル ![「外部リンク」アイコン](../../icons/launch-glyph.svg "「外部リンク」アイコン")](https://control.softlayer.com/) にログオンします。

2. ディスプレイ上部のナビゲーション・バーから、**「ネットワーク」->「CDN」**と選択します。

   ![「ネットワーク」メニュー・オプション](images/network-cdn.png)

3. **「コンテンツ配信ネットワーク」**ページで、右上隅の**「CDN の注文 (Order CDN)」**ボタンを選択します。

	![CDN の注文の選択](images/order-cdn-button.png)

## Bluemix ポータルから:

1. [Bluemix ポータル](https://www.ibm.com/cloud-computing/bluemix/)にログオンします。

2. **「IBM Bluemix カタログ」**をクリックします。左側のナビゲーション・バーから**「ネットワーク」**を選択します。

   ![Bluemix CDN ナビゲーション](images/bluemix_navigation.png)

3. **CDN タイル**をクリックします。これにより、「ベンダーの選択 (Vendor Selection)」画面に移動します。

   ![Bluemix CDN アイコン](images/bluemix_tile.png)

4. **「CDN プロバイダーの選択 (Select a CDN Provider)」**画面で、CDN プロバイダー・オプションから選択します。画面下部の**「選択 (Selected)」**ボタンをクリックして選択済みオプションを確定し、**「プロビジョンの開始 (Start Provision)」**をクリックしてプロビジョニング・プロセスを開始します。

	![CDN プロバイダーの選択](images/newReducedSizeVendorSelectAndProvision.png)
	
5. 以下のように、**「名前の構成 (Configure Name)」**フィールドに入力します。 
      * _ホスト名_ (**必須**) を指定します。これは、CDN の 1 次識別子となります (例えば、_example.testingcdn.net_ など)。
      * オプションで、カスタム _CNAME_ を指定できます (_myfirstcdn.cdnedge.bluemix.net_ など)。CNAME が指定されないと、自動で作成されます。
      
      ![名前の構成](images/configure-hostname-cname.png)
		
6. 以下のように、**「オリジンの構成 (Configure Your Origin)」**フィールドに入力します。このフィールドを構成するには、**「サーバー」**または**「オブジェクト・ストレージ」**のオプションを選択する必要があります。(**「パス」**の指定はオプションです。)
		
  * **「サーバー」オプション**: **「サーバー」**オプションを選択した場合は、データ・キャッシュ元のオリジン・サーバーのホスト名または IP アドレスを入力します。 
      * このオプションを選択した場合、オリジン・サーバーの**「IP アドレス」**を指定する必要があります。
      * **「HTTP ポート」**または**「HTTPS ポート」**、あるいはその両方を指定することもできます。(これらのフィールドは、オリジン・サーバーに接続するために使用できるプロトコルとポート番号を示します。)

	   ![オリジン・サーバーの構成](images/configure-origin-server.png)
		
  * **「オブジェクト・ストレージ」オプション**: **「オブジェクト・ストレージ」**オプションを選択した場合は、以下の情報を指定する必要があります。
      * オブジェクトをフェッチする元の**「エンドポイント」**。
      * コンテンツが保管された**「バケット」**の名前。
      * **「HTTPS ポート」**。
      * CDN サービスで使用できるファイル拡張子をコンマ区切りで指定することもできます。(ファイル名の拡張子を指定しないと、すべてのファイル拡張子が許可されます。)
      * **「バケット」**内の各**「オブジェクト」**の**「アクセス制御リスト」** (ACL) を "public-read" に設定する必要があります。
		
	   ![オブジェクト・ストレージの構成](images/configure-origin-object-storage.png)

7. **「その他のオプション」**フィールドの構成: このセクションには、**「失効コンテンツの提供 (Serve Stale Content)」**フィールドと**「ヘッダーを尊重 (Respect Headers)」**フィールドの構成オプションが含まれます。
    
     * **「失効コンテンツの提供 (Serve Stale Content)」**フィールド: **「失効コンテンツの提供 (Serve Stale Content)」**は、オリジン・サーバーに到達できない場合に、期限切れのキャッシュ・コンテンツを提供するかどうかを示すブール値です。現行の制限のため、**「失効コンテンツの提供 (Serve Stale Content)」**機能は、 ユーザーがこれを**「オン」**と**「オフ」**のどちらに設定したかに関係なく、デフォルトで有効になっています。
     * **「ヘッダーを尊重 (Respect Headers)」**フィールド: **「ヘッダーを尊重 (Respect Headers)」**オプションが**「オン」**の場合、オリジンによってヘッダーで定義された TTL 設定が、デフォルトの CDN TTL をオーバーライドします。**「ヘッダーを尊重 (Respect Headers)」**はデフォルトで**「オン」**に設定されていますが、このフィールドは構成する必要があります。

		![その他のオプション](images/other-options.png)
		
8. 右下隅の**「作成」**ボタンを選択して、CDN を作成します。
