---

copyright:
  years: 2017,2018
lastupdated: "2018-02-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CDN の注文

コンテンツ配信ネットワーク (CDN) を注文する方法について、以下に説明します。 CDN の注文は、[カスタマー・ポータル ![「外部リンク」アイコン](../../icons/launch-glyph.svg "「外部リンク」アイコン")](https://control.softlayer.com/) または [Bluemix ポータル](https://www.ibm.com/cloud-computing/bluemix/)から行えます。

## カスタマー・ポータルから:

**ステップ 1:**

まず、固有の資格情報を使用して[カスタマー・ポータル ![「外部リンク」アイコン](../../icons/launch-glyph.svg "「外部リンク」アイコン")](https://control.softlayer.com/) にログオンします。

**ステップ 2:**

ディスプレイ上部のナビゲーション・バーから、**「ネットワーク」->「CDN」**と選択します。

   ![「ネットワーク」メニュー・オプション](images/network-cdn.png)

**ステップ 3:**

**「コンテンツ配信ネットワーク」**ページで、右上隅の**「CDN の注文 (Order CDN)」**ボタンを選択します。

   ![CDN の注文の選択](images/order-cdn-button.png)

## Bluemix ポータルから:

**ステップ 1:**

[Bluemix ポータル](https://www.ibm.com/cloud-computing/bluemix/)にログオンします。

**ステップ 2:**

[「IBM Bluemix カタログ」](https://console.bluemix.net/catalog/)をクリックします。 左側のナビゲーション・バーから**「ネットワーク」**を選択します。

   ![Bluemix CDN ナビゲーション](images/bluemix_navigation.png)

**ステップ 3:**

**CDN タイル**をクリックします。これにより、「ベンダーの選択 (Vendor Selection)」画面に移動します。

   ![Bluemix CDN アイコン](images/bluemix_tile.png)


**ステップ 4:**

**「CDN プロバイダーの選択 (Select a CDN Provider)」**画面で、CDN プロバイダー・オプションから選択します。 **「選択」**ボタンをクリックして選択済みオプションを確認し、画面の右下にある**「プロビジョンの開始 (Start Provision)」**をクリックしてプロビジョニング・プロセスを開始します。  
       ![CDN プロバイダーの選択](images/Vendor_Select_And_Provision.png)

**ステップ 5:**

以下のように、**「名前の構成 (Configure Name)」**フィールドに入力します。  

  * **「ホスト名」**(**必須**) を指定します。これは、CDN の 1 次 ID となります (例えば、_example.testingcdn.net_)。  
  * オプションで、カスタム **CNAME** を指定できます (_myfirstcdn.cdnedge.bluemix.net_ など)。 CNAME が指定されないと、自動で作成されます。   

       ![名前の構成](images/configure-hostname-cname.png)  

    **注**: 不適切な CNAME を使用するとサービスの終了につながるおそれがあります。

**ステップ 6:**

以下のように、**「オリジンの構成 (Configure Your Origin)」**フィールドに入力します。このフィールドを構成するには、**「サーバー」**または**「オブジェクト・ストレージ」**のオプションを選択する必要があります。  

   * **「ホスト・ヘッダー (Host header)」**(オプション) を指定します。指定しない場合、デフォルトは**「ホスト名」**です。ホスト・ヘッダーについて詳しくは、[ホスト・ヘッダーのサポート](about.html#host-header-support-)の機能説明を参照してください。  
   
   * **「パス」**(オプション) を指定します。「パス」は**「ホスト名」**に対する相対パスでなければなりません。 
   
      ![オリジンの構成](images/configure-origin.png)  

  * **「サーバー」オプション**: **「サーバー」**オプションを選択した場合は、データ・キャッシュ元のオリジン・サーバーのホスト名または IP アドレスを入力します。
      * このオプションを選択した場合、**「オリジン・サーバー・アドレス」**(オリジン・サーバーのホスト名または IPv4 アドレス) を指定する必要があります。**「HTTPS ポート」**を選択する場合、**「オリジン・サーバー・アドレス」**は、IP アドレスではなくホスト名でなければなりません。
      * **「HTTP ポート」**または**「HTTPS ポート」**、あるいはその両方を指定することもできます。 これらのフィールドは、オリジン・サーバーに接続するために使用できるプロトコルとポート番号を示します。 デフォルト以外のポート番号の場合、許可ポート番号のリストについては、[よくある質問](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。

	     ![オリジン・サーバーの構成](images/configure-origin-server.png)

  * **「オブジェクト・ストレージ」オプション**: **「オブジェクト・ストレージ」**オプションを選択した場合は、以下の情報を指定する必要があります。
      * オブジェクトのフェッチ元の**「エンドポイント」**。
      * コンテンツが保管された**「バケット」**の名前。
      * **「HTTPS ポート」**。
      * CDN サービスで使用できるファイル拡張子をコンマ区切りで指定することもできます。(ファイル名の拡張子を指定しないと、すべてのファイル拡張子が許可されます。)
      * **「バケット」**内の各**「オブジェクト」**の**「アクセス制御リスト」** (ACL) を「public-read」に設定する必要があります。

	     ![オブジェクト・ストレージの構成](images/configure-origin-object-storage.png)

**ステップ 7:**

**「その他のオプション」**フィールドを構成します。このセクションには、**「ヘッダーを尊重 (Respect Headers)」**フィールドの構成オプションが含まれています。

   * **「ヘッダーを尊重 (Respect Headers)」**オプションが**「オン」**の場合、オリジンによってヘッダーで定義された TTL 設定が、デフォルトの CDN TTL をオーバーライドします。**「ヘッダーを尊重 (Respect Headers)」**はデフォルトで**「オン」**に設定されていますが、このフィールドは構成する必要があります。  

        ![その他のオプション](images/other-options.png)

**ステップ 8:**

右下隅の**「作成」**ボタンを選択して、CDN を作成します。
