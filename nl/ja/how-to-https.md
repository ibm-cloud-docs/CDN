---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# DV SAN を使用する HTTPS のドメイン制御検証の実行
{: #completing-domain-control-validation-for-https-with-dv-san}

以下の図は、CDN が作成されてから実行状況になるまでの、CDN が通過するさまざまな状態の概要を示しています。

  ![SAN 状態遷移図](images/state-diagram-san.png)

## ドメイン制御検証の初期手順
{: #initial-steps-to-domain-control-validation}

**ステップ 1:**

DV SAN 証明書を使用する CDN を注文した後、証明書要求プロセスが開始されます。 このプロセス中に、{{site.data.keyword.cloud}} CDN は Akamai からの証明書を要求します。 証明書が使用可能になったら、Akamai は認証局 (CA) に要求を発行します。

  * この期間、CDN の状況は**「証明書要求中 (Requesting Certificate)」**と示されます。

    ![「証明書要求中 (Requesting Certificate)」状況](images/requesting-cert.png)

**ステップ 2:**

CA は、要求を受け取ると、ドメイン検証チャレンジを発行します。

  * これが起こると、CDN の状況は**「ドメイン検証が必要 (Domain validation needed)」**に変わります。

    ![ドメイン検証が必要 (Domain Validation Needed)](images/domain-validation-needed.png)

**ステップ 3:**

検証が必要な CDN の名前をクリックします。 「概要 (Overview)」ページが開き、ここで CDN の全体的な状況を確認できます。 ページの上部にアラートが表示され、ドメイン検証が必要であることが示されます。 **「ドメイン検証の表示 (View domain validation)」**ボタンを選択すると、検証プロセスを実行するために必要なチャレンジ情報を表示するウィンドウが開きます。

   ![ドメイン検証が必要 (Domain Validation Needed)](images/view-domain-validation.png)

**ステップ 4:** ドメイン検証チャレンジに対処する方法についてのセクションで説明されている検証ステップのいずれかを完了すると、CDN は**「証明書デプロイ中 (Deploying certificate)」**状況に移ります。 この間に、Akamai は、検証された証明書をエッジ・サーバーに配布します。 証明書のデプロイには、2 時間から 4 時間かかる可能性があります。

  * このプロセスが完了すると、使用する検証方式に関係なく、すべてのドメインが **「CNAME 構成 (CNAME Configuration)」**状況に移ります。

[実行中にする](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running)のページに、CNAME 構成を完了し、CDN を監視する方法についての追加情報があります。


## ドメイン制御検証
{: #domain-control-validation}

CDN ドメイン・ネームを SAN 証明書に追加するには、ドメインを管理制御する権限があることを証明する必要があります。 この証明プロセスを、ドメイン制御検証 (DCV) の対処と呼びます。 DCV には、48 時間以内に対処する必要があります。 それができなかった場合、要求の有効期限が切れ、注文処理を再度開始する必要があります。 DCV に対処する 3 つの異なる方法について、以降のセクションで説明します。


### CNAME
{: #cname}

この方式は、CDN がライブ・トラフィックを提供**していない**場合に**のみ**推奨されます。 ライブ・トラフィックを提供しているドメインの場合は、標準方式またはリダイレクト方式を使用してドメインの検証を行うことをお勧めします。

この方式を使用するには、CDN ドメインの CNAME レコードを DNS 構成に追加します。 使用する CNAME 値は、CDN の作成時に使用した CNAME です。 値の最後は `cdnedge.bluemix.net` ドメインで終わる必要があります。 他のアクションは不要です。 この時点から、DCV は自動的に進行します。 検証には、2 時間から 4 時間かかる可能性があります。 証明書がデプロイされると、CDN は直接「実行中」状況に移ります。

ほとんどの DNS プロバイダーは、CNAME の設定または変更の手順を提供しています。 以下に、標準的な CNAME レコードの例を示します。

| **リソース・タイプ** | **ホスト** | **ポイント先 (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 分 |


---
### 標準
{: #standard}

ドメイン検証に「標準 (Standard)」方式を選択すると、「ドメイン検証 (Domain Validation)」ウィンドウに**チャレンジ URL** と**チャレンジ応答**が表示されます。 ドメイン検証プロセスを実行するために、提供された**チャレンジ応答**をオリジン・サーバーに追加します。 それが追加されると、CA は、**チャレンジ URL** に指定された URL を使用してオリジン・サーバーから**チャレンジ応答**を取得できます。 オリジン・サーバーが正しく構成された後、ドメイン検証には 2 時間から 4 時間かかる可能性があります。

   ![ドメイン検証チャレンジ - 標準](images/domain-validation-standard.png)

標準方式を使用してドメイン検証を正常に完了するためには、オリジン・サーバーを特定の方法で構成する必要があります。 _Apache(TM)_ サーバーおよび _Nginx(TM)_ サーバーを例に、手順の概要を以下に示します。

**シチュエーション例**
* オリジン・サーバー: `www.example.com`
* CDN ドメイン: `cdn.example.com`
* チャレンジ URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* チャレンジ応答: `examplechallenge`

#### Apache 構成
{: #apache-configuration}

  * **ステップ 1:** Apache2 サーバーが実行されているマシンにログインします。

  * **ステップ 2:** チャレンジ応答用のチャレンジ応答ファイルを、Web サイト・コンテンツ用のディレクトリー内の `.well-known/acme-challenge/` に作成します。  Apache2 Web サイト・コンテンツのデフォルトの場所は `/var/www/html/` です。 この例では、チャレンジ応答は `/var/www/html/.well-known/acme-challenge/` ディレクトリーに置かれます。

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **ステップ 3:** 必要であれば、Apache2 サーバー構成ファイルを開きます。 構成ファイルのデフォルトの場所は、`/etc/apache2/apache2.conf` および `/etc/apache2/sites-enabled/` です。

  * **ステップ 4:** 必要であれば、追加の **ServerAlias** として、CDN ドメインをオリジンの仮想ホストに追加します。

  * **ステップ 5:** Apache2 サーバー構成を変更する必要があった場合、以下のコマンドを使用して、最小のダウン時間で Apache2 サーバーを再始動します。

      ```
      apachectl -k graceful
      ```

  * **ステップ 6:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### Nginx 構成
{: #nginx-configuration}

  * **ステップ 1:** Nginx サーバーが実行されているマシンにログインします。

  * **ステップ 2:** チャレンジ応答用のチャレンジ応答ファイルを、Web サイト・コンテンツ用のディレクトリー内の `.well-known/acme-challenge/` に作成します。  Nginx Web サイト・コンテンツのデフォルトの場所は `/usr/share/nginx/html/` です。  この例では、チャレンジ応答は `/usr/share/nginx/html/.well-known/acme-challenge/` ディレクトリーに置かれます。
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **ステップ 3:** 必要であれば、Nginx サーバー構成ファイルを開きます。 構成ファイルのデフォルトの場所は、`/etc/nginx/nginx.conf` および `/etc/nginx/conf.d/` です。

  * **ステップ 4:** 必要であれば、追加の **server_name** として、CDN ドメインをオリジンの server ブロックに追加します。

  * **ステップ 5:** Nginx サーバー構成を変更する必要があった場合、以下のコマンドを使用して、最小のダウン時間で Nginx サーバーを再始動します。

      ```
      nginx -s reload
      ```

  * **ステップ 6:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### ドメイン検証に対処するこの標準方式が CA に対して準備ができていることを検証する
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* この方式が機能することを `curl` を介して検証するには、チャレンジ URL に対して次のコマンドを実行します。
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* この方式が機能することをブラウザーを介して検証するには、ブラウザーからチャレンジ URL にアクセスしてみます。

どちらの場合も、オリジン・サーバーに保管されたドメイン検証チャレンジ・ファイル・オブジェクトのコピーを取得できなければなりません。

#### 標準方式の場合のクリーンアップ
{: #clean-up-for-the-standard-method}

CDN が**「証明書デプロイ中 (Certificate deploying)」**状況に達した後、以下を行います。
1. (オプション) `examplechallenge-fileobject` ファイルを削除します。
1. (オプション) 必要であれば、追加した ServerAlias (Apache2) または server_name (Nginx) をサーバー構成から削除します。
1. CDN ドメインとオリジン・サーバー IP との間の A レコードを削除します。

---
### リダイレクト
{: #redirect}

**「リダイレクト (Redirect)」**タブをクリックすると、リダイレクトによってドメイン検証に対処するために必要なすべての情報が表示されます。 この情報により、CA はオリジン・サーバーを通して Akamai から**チャレンジ応答**のコピーを取得できます。 サーバーが正しく構成された後、ドメイン検証には 2 時間から 4 時間かかる可能性があります。

   ![ドメイン検証チャレンジ - リダイレクト](images/domain-validation-redirect.png)

リダイレクト方式を使用してドメイン検証を正常に実行するためには、Web サーバーを特定の方法で構成することが必要な場合があります。 Apache サーバーおよび Nginx サーバーを例に、手順の概要を以降のセクションで示します。

**シチュエーション例**
* オリジン・サーバー: `www.example.com`
* CDN ドメイン: `cdn.example.com`
* チャレンジ URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* URL リダイレクト: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Apache リダイレクト構成
{: #apache-redirect-configuration}

  * **ステップ 1:** Apache2 サーバーが実行されているマシンにログインします。

  * **ステップ 2:** Apache2 サーバー構成ファイルを開きます。 構成ファイルのデフォルトの場所は、`/etc/apache2/apache2.conf` および `/etc/apache2/sites-enabled/` です。

  * **ステップ 3:** 構成ファイル内の適切な場所に redirect ステートメントを追加します。 必要であれば、追加の **ServerAlias** として、CDN ドメインをオリジンの仮想ホストに追加します。

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **ステップ 4:** 以下のコマンドを使用して、最小のダウン時間で Apache2 サーバーを再始動します。

    ```
    apachectl -k graceful
    ```

  * **ステップ 5:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### Nginx リダイレクト構成
{: #nginx-redirect-confguration}

  * **ステップ 1:** Nginx サーバーが実行されているマシンにログインします。

  * **ステップ 2:** Nginx サーバー構成ファイルを開きます。 構成ファイルのデフォルトの場所は、`/etc/nginx/nginx.conf` および `/etc/nginx/conf.d/` です。

  * **ステップ 3:** このステップには、2 つの同等に有効なメソッドがあります。

    * オプション 1: (推奨) 適切な `server` ブロック内に、リダイレクトを実行するための `return` ディレクティブが含まれた `location` ブロックを追加します。 必要であれば、追加の **server_name** として、CDN ドメインをオリジンの server ブロックに追加します。

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives
      # ...
    }
    ```

   * オプション 2: `server` ブロック内に `rewrite` ディレクティブを追加します。 必要であれば、追加の **server_name** として、CDN ドメインをオリジンの server ブロックに追加します。

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives
      # ...
    }
    ```

  * **ステップ 4:** 以下のコマンドを使用して、最小のダウン時間で Nginx サーバーを再始動します。

    ```
    nginx -s reload
    ```

  * **ステップ 5:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### リダイレクトが起こっていることの検証
{: #verify-that-the-redirect-is-occurring}

上記のステップを完了すると、特定のチャレンジ URL のトラフィック_のみ_ が URL リダイレクトに転送されます。 `curl` またはブラウザーのいずれかを使用して、リダイレクトが適切に機能したことを検証できます。

* リダイレクトが機能することを `curl` を介して検証するには、チャレンジ URL に対して次のコマンドを実行します。

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* リダイレクトが機能することをブラウザーを介して検証するには、ブラウザーからチャレンジ URL に到達を試みます。

どちらの場合も、元の要求がリダイレクトされた先の Akamai (`dcv.akamai.com` ドメイン) から、ドメイン検証チャレンジ・ファイル・オブジェクトのコピーを取得できなければなりません。

#### リダイレクト方式の場合のクリーンアップ
{: #clean-up-for-the-redirect-method}

CDN が**「証明書デプロイ中 (Certificate deploying)」**状況に達した後、以下を行います。
1. (オプション) 構成ファイルから redirect ステートメントまたは redirect ブロックを削除します。
1. (オプション) 必要であれば、追加した ServerAlias (Apache2) または server_name (Nginx) をサーバー構成から削除します。
1. CDN ドメインとオリジン・サーバー IP との間の A レコードを削除します。
