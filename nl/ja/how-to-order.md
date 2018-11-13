---

copyright:
  years: 2017,2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CDN の注文

コンテンツ配信ネットワーク (CDN) を注文する方法について、以下に説明します。 CDN は、[IBM Cloud ポータル](https://www.ibm.com/cloud-computing/bluemix/)から注文できます。

## CDN ページへのナビゲート

**ステップ 1:**

[IBM Cloud ポータル](https://www.ibm.com/cloud-computing/bluemix/)からアカウントにログインします。

**ステップ 2:**

[「IBM Cloud カタログ」](https://console.bluemix.net/catalog/)をクリックします。 左側のナビゲーション・バーから**「ネットワーク」**を選択します。

   ![Bluemix CDN ナビゲーション](images/bluemix_navigation.png)

**ステップ 3:**

**CDN タイル**をクリックします。

   ![Bluemix CDN アイコン](images/bluemix_tile.png)


## 新規 CDN の注文

**ステップ 1:**

右下の**「作成」**をクリックします。これで、CDN アカウントがまだない場合は、CDN アカウントが作成され、CDN 構成画面にリダイレクトされます。

   ![CDN 概要](images/content-delivery.png)

**ステップ 2:**

以下のように、**「名前の構成 (Configure Name)」**フィールドに入力します。  

  * **「ホスト名」**(**必須**) を指定します。これは、CDN の 1 次 ID となります (例えば、`example.testingcdn.net`)。  
  * オプションで、カスタム **CNAME** を指定できます (`myfirstcdn.cdnedge.bluemix.net` など)。CNAME が指定されないと、自動で作成されます。 CNAME には、サフィックス `cdnedge.bluemix.net` が自動的に付加されます。不適切な CNAME を使用するとサービスの終了につながるおそれがあります。

       ![名前の構成](images/configure-hostname-cname.png)  

    **注**: 新しい CDN をプロビジョニングした後は、DNS プロバイダーで CNAME を構成**しなければなりません**。

**ステップ 3:**

以下のように、**「オリジンの構成 (Configure Your Origin)」**フィールドに入力します。このフィールドを構成するには、**「サーバー」**または**「オブジェクト・ストレージ」**のオプションを選択する必要があります。  

  * **ステップ 3、オプション 1: 「サーバー」オプション**

     ![オリジンの構成](images/configure-origin-server.png)

      * **「オリジン・サーバー・アドレス」** (オリジン・サーバーのホスト名または IPv4 アドレス) を指定する必要があります。**「HTTPS ポート」**も選択される場合、**「オリジン・サーバー・アドレス」**は、IP アドレスでなくホスト名にしなければなりません。

      * **「ホスト・ヘッダー (Host header)」**(オプション) を指定します。 指定しない場合、デフォルトは**「ホスト名」**です。ホスト・ヘッダーについて詳しくは、[ホスト・ヘッダーのサポート](feature-descriptions.html#host-header-support)の機能説明を参照してください。  

      * オリジンでコンテンツを取得できる**「パス」**を指定します (オプション)。この時点でパスを追加することによる影響については、[パスベースの CDN マッピング](feature-descriptions.html#path-based-cdn-mappings)の機能説明を参照してください。

      * **「HTTP ポート」**または**「HTTPS ポート」**、あるいはその両方を指定することもできます。 これらのフィールドは、オリジン・サーバーに接続するために使用できるプロトコルとポート番号を示します。 デフォルト以外のポート番号の場合、許可ポート番号のリストについては、[よくある質問](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。

      * **「SSL 証明書」**: このオプションは、「HTTPS ポート」が選択された場合に_のみ_ 表示されます。「サーバー」または「オブジェクト・ストレージ」のいずれかで**「HTTPS ポート」**を選択する場合、SSL 証明書のオプションとして**「ワイルドカード証明書」**または**「DV SAN 証明書」**を選択できます。両方とも、HTTPS による強化されたセキュリティーを提供します。
        * **「ワイルドカード証明書」**は、**CNAME** を使用する場合にのみ HTTPS トラフィックを許可し、お客様側ではこれ以上のアクションは必要ありません。
        * **「DV SAN 証明書」**は、ドメインでの HTTPS トラフィックを許可しますが、検証のための追加ステップが必要です。このオプションを選択した場合に必要なステップと時間制約については、[HTTPS のためのドメイン制御検証の実行](how-to-https.html#completing-domain-control-validation-for-https)のページを参照してください。

	     ![オリジン・サーバーの構成](images/ssl-cert-options.png)

  * **ステップ 3、オプション 2: 「オブジェクト・ストレージ」オプション**

    ![オブジェクト・ストレージの構成](images/configure-origin-object-storage.png)

      * オブジェクトをフェッチする元の**「エンドポイント」**を指定する**必要**があります。

      * **「ホスト・ヘッダー (Host header)」**(オプション) を指定します。 指定しない場合、デフォルトは**「ホスト名」**です。ホスト・ヘッダーについて詳しくは、[ホスト・ヘッダーのサポート](feature-descriptions.html#host-header-support)の機能説明を参照してください。  

      * オリジンでコンテンツを取得できる**「パス」**を指定します (オプション)。ここでパスを追加することによる影響については、[パスベースの CDN マッピング](feature-descriptions.html#path-based-cdn-mappings)の機能説明を参照してください。

      * コンテンツを保管する**バケット**の名前を指定する**必要**があります。

      * **「HTTP ポート」**または**「HTTPS ポート」**、あるいはその両方を指定することもできます。 これらのフィールドは、オリジン・サーバーに接続するために使用できるプロトコルとポート番号を示します。 デフォルト以外のポート番号の場合、許可ポート番号のリストについては、[よくある質問](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)を参照してください。

      * **「SSL 証明書」**: このオプションは、「HTTPS ポート」が選択された場合に_のみ_ 表示されます。「サーバー」または「オブジェクト・ストレージ」のいずれかで**「HTTPS ポート」**を選択する場合、SSL 証明書のオプションとして**「ワイルドカード証明書」**または**「DV SAN 証明書」**を選択できます。両方とも、HTTPS による強化されたセキュリティーを提供します。
        * **「ワイルドカード証明書」**は、**CNAME** を使用する場合にのみ HTTPS トラフィックを許可し、お客様側ではこれ以上のアクションは必要ありません。
        * **「DV SAN 証明書」**は、ドメインでの HTTPS トラフィックを許可しますが、検証のための追加ステップが必要です。このオプションを選択した場合に必要なステップと時間制約については、[HTTPS のためのドメイン制御検証の実行](how-to-https.html#completing-domain-control-validation-for-https)のページを参照してください。

        ![HTTPS の構成](images/ssl-cert-options.png)

      **注**: クラウド・オブジェクト・ストレージ・プロバイダーで、バケット内の各オブジェクトの**「アクセス制御リスト」** (ACL) を「public-read」に設定する必要があります。

右下隅の**「作成」**ボタンを選択して、CDN を作成します。
