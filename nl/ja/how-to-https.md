---

copyright:
  years: 2018
lastupdated: "2018-06-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HTTPS のためのドメイン制御検証の実行

## ドメイン制御検証の初期手順

**ステップ 1:**

DV SAN 証明書を使用する CDN を注文した後、証明書要求プロセスが開始されます。このプロセス中に、IBM Cloud CDN は Akamai からの証明書を要求します。 証明書が使用可能になったら、Akamai は認証局 (CA) に要求を発行します。

  * この間、CDN 状況は**「証明書要求中 (Requesting Certificate)」**と示されます。

    ![「証明書要求中 (Requesting Certificate)」状況](images/requesting-cert.png)

**ステップ 2:**

CA は、要求を受け取ると、ドメイン検証チャレンジを発行します。

  * これが起こると、CDN の状況は**「ドメイン検証が必要 (Domain validation needed)」**に変わります。

    ![ドメイン検証が必要 (Domain Validation Needed)](images/domain-validation-needed.png)

**ステップ 3:**

検証が必要な CDN の名前をクリックします。「概要 (Overview)」ページが開き、ここで CDN の全体的な状況を確認できます。ページの上部には、ドメイン検証が必要であることを示すアラートが表示されます。**「ドメイン検証の表示 (View domain validation)」**ボタンを選択すると、検証プロセスを実行するために必要なチャレンジ情報を表示するウィンドウが開きます。

   ![ドメイン検証が必要 (Domain Validation Needed)](images/view-domain-validation.png)

**ステップ 4:** ドメイン検証チャレンジに対処する方法についてのセクションで説明されている検証ステップのいずれかを完了すると、CDN は**「証明書デプロイ中 (Deploying certificate)」**状況に移ります。この間に、Akamai は、検証された証明書をエッジ・サーバーに配布します。証明書のデプロイには、2 時間から 4 時間かかる可能性があります。

  * このプロセスが完了すると、使用する検証方式に関係なく、すべてのドメインが **「CNAME 構成 (CNAME Configuration)」**状況に移ります。

CNAME 構成の実行および CDN の監視についての追加情報が『[実行中にする](basic-functions.html#get-to-running)』ページにあります。


## ドメイン検証チャレンジ

ドメイン検証チャレンジは、お客様がドメインの所有者であることを証明するものです。ドメイン検証チャレンジに対処する方法には、次の 3 つがあります。

**注:** 48 時間以内にドメイン検証要求に応答しない場合、要求は期限切れになり、注文プロセスを始めからやり直すことが必要になります。

### CNAME

CDN を注文する前にお客様の CNAME レコードが DNS プロバイダーに追加済みの場合、他の作業を行う必要はありません。ドメイン検証は、IBM Cloud、Akamai、および CA によって自動的に処理されます。検証には、2 時間から 4 時間かかる可能性があります。

  * DNS プロバイダーで CNAME をまだ構成していない場合は、この時点で行う必要があります。ほとんどの DNS プロバイダーは、CNAME の設定または変更の手順を提供しています。

   ![ドメイン検証 - CNAME](images/domain-validation-cname.png)

**注:** この方式は、CDN がライブ・トラフィックを提供**していない**場合**のみ**推奨されます。ライブ・トラフィックを提供しているドメインの場合は、「標準」方式を使用してドメインの検証を行うことをお勧めします。

---
### 標準

ドメイン検証に「標準 (Standard)」方式を選択すると、「ドメイン検証 (Domain Validation)」ウィンドウに**チャレンジ URL** と**チャレンジ応答**が表示されます。ドメイン検証プロセスを実行するには、提供された**チャレンジ応答**をオリジン・サーバーに追加する必要があります。これにより、CA は**チャレンジ URL** に指定された URL を使用してオリジン・サーバーから**チャレンジ応答**を取得できます。オリジン・サーバーが正しく構成された後、ドメイン検証には 2 時間から 4 時間かかる可能性があります。

   ![ドメイン検証チャレンジ - 標準](images/domain-validation-standard.png)

標準方式を使用してドメイン検証を正常に実行するためには、オリジン・サーバーを特定の方法で構成する必要があります。Apache サーバーおよび Nginx サーバーの手順例の概要を以下に示します。

**シチュエーション例**
* オリジン・サーバー: `www.example.com`
* CDN ドメイン: `cdn.example.com`
* チャレンジ URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* チャレンジ応答: `examplechallenge`

#### Apache 構成

  * **ステップ 1:** Apache2 サーバーが実行されているマシンにログインします。

  * **ステップ 2:** チャレンジ応答用のチャレンジ応答ファイルを、Web サイト・コンテンツ用のディレクトリー内の `.well-known/acme-challenge/` に作成します。Apache2 Web サイト・コンテンツのデフォルトの場所は `/var/www/html/` です。この例では、チャレンジ応答は `/var/www/html/.well-known/acme-challenge/` ディレクトリーに置かれます。

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **ステップ 3:** 必要であれば、Apache2 サーバー構成ファイルを開きます。構成ファイルのデフォルトの場所は、`/etc/apache2/apache2.conf` および `/etc/apache2/sites-enabled/` です。

  * **ステップ 4:** 必要であれば、追加の **ServerAlias** として、CDN ドメインをオリジンの仮想ホストに追加します。

  * **ステップ 5:** Apache2 サーバー構成を変更する必要があった場合、以下のコマンドを使用して、最小のダウン時間で Apache2 サーバーを再始動します。

      ```
      apachectl -k graceful
      ```

  * **ステップ 6:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### Nginx 構成

  * **ステップ 1:** Nginx サーバーが実行されているマシンにログインします。

  * **ステップ 2:** チャレンジ応答用のチャレンジ応答ファイルを、Web サイト・コンテンツ用のディレクトリー内の `.well-known/acme-challenge/` に作成します。Nginx Web サイト・コンテンツのデフォルトの場所は `/usr/share/nginx/html/` です。 この例では、チャレンジ応答は `/usr/share/nginx/html/.well-known/acme-challenge/` ディレクトリーに置かれます。
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **ステップ 3:** 必要であれば、Nginx サーバー構成ファイルを開きます。構成ファイルのデフォルトの場所は、`/etc/nginx/nginx.conf` および `/etc/nginx/conf.d/` です。

  * **ステップ 4:** 必要であれば、追加の **server_name** として、CDN ドメインをオリジンの server ブロックに追加します。

  * **ステップ 5:** Nginx サーバー構成を変更する必要があった場合、以下のコマンドを使用して、最小のダウン時間で Nginx サーバーを再始動します。

      ```
      nginx -s reload
      ```

  * **ステップ 6:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### ドメイン検証に対処するこの標準方式が CA に対して準備ができていることを検証する

* この方式が機能することを `curl` を介して検証するには、チャレンジ URL に対して次のコマンドを実行します。
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* この方式が機能することをブラウザーを介して検証するには、ブラウザーからチャレンジ URL にアクセスしてみます。

どちらの場合も、オリジン・サーバーに保管されたドメイン検証チャレンジ・ファイル・オブジェクトのコピーを取得できなければなりません。

#### 標準方式の場合のクリーンアップ

CDN が**「証明書デプロイ中 (Certificate deploying)」**状況に達した後、以下を行います。
1. (オプション) `examplechallenge-fileobject` ファイルを削除します。
1. (オプション) 必要であれば、追加した ServerAlias (Apache2) または server_name (Nginx) をサーバー構成から削除します。
1. CDN ドメインとオリジン・サーバー IP との間の A レコードを削除します。

---
### リダイレクト

**「リダイレクト (Redirect)」**タブをクリックすると、リダイレクトによってドメイン検証に対処するために必要なすべての情報が表示されます。この情報により、CA はオリジン・サーバーを通して Akamai から**チャレンジ応答**のコピーを取得できます。サーバーが正しく構成された後、ドメイン検証には 2 時間から 4 時間かかる可能性があります。

   ![ドメイン検証チャレンジ - リダイレクト](images/domain-validation-redirect.png)

リダイレクト方式を使用してドメイン検証を正常に実行するためには、Web サーバーを特定の方法で構成することが必要な場合があります。Apache サーバーおよび Nginx サーバーの手順例の概要を以下に示します。

**シチュエーション例**
* オリジン・サーバー: `www.example.com`
* CDN ドメイン: `cdn.example.com`
* チャレンジ URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* URL リダイレクト: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Apache リダイレクト構成

  * **ステップ 1:** Apache2 サーバーが実行されているマシンにログインします。

  * **ステップ 2:** Apache2 サーバー構成ファイルを開きます。構成ファイルのデフォルトの場所は、`/etc/apache2/apache2.conf` および `/etc/apache2/sites-enabled/` です。

  * **ステップ 3:** 構成ファイル内の適切な場所に redirect ステートメントを追加します。必要であれば、追加の **ServerAlias** として、CDN ドメインをオリジンの仮想ホストに追加します。
    ```
    Redirect http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **ステップ 4:** 以下のコマンドを使用して、最小のダウン時間で Apache2 サーバーを再始動します。

    ```
    apachectl -k graceful
    ```

  * **ステップ 5:** DNS で CDN ドメインとオリジン・サーバーの IP アドレスとの間に、A レコードを作成します。

#### Nginx リダイレクト構成

  * **ステップ 1:** Nginx サーバーが実行されているマシンにログインします。

  * **ステップ 2:** Nginx サーバー構成ファイルを開きます。構成ファイルのデフォルトの場所は、`/etc/nginx/nginx.conf` および `/etc/nginx/conf.d/` です。

  * **ステップ 3:** このステップには、2 つの同等に有効なメソッドがあります。

    * オプション 1: (推奨) 適切な `server` ブロック内に、リダイレクトを実行するための `return` ディレクティブが含まれた `location` ブロックを追加します。必要であれば、追加の **server_name** として、CDN ドメインをオリジンの server ブロックに追加します。

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

   * オプション 2: `server` ブロック内に `rewrite` ディレクティブを追加します。必要であれば、追加の **server_name** として、CDN ドメインをオリジンの server ブロックに追加します。

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

  * **ステップ 5:** DNS で CDN ドメインとオリジン・サーバーの IP との間に、A レコードを作成します。

#### リダイレクトが起こっていることの検証

上記のステップを完了すると、特定のチャレンジ URL のトラフィックのみが URL リダイレクトに転送されます。`curl` またはブラウザーのいずれかを介して、リダイレクトが適切に機能したことを検証できます。

* リダイレクトが機能することを `curl` を介して検証するには、チャレンジ URL に対して次のコマンドを実行します。

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* リダイレクトが機能することをブラウザーを介して検証するには、ブラウザーからチャレンジ URL にアクセスしてみます。

どちらの場合も、元の要求がリダイレクトされた先の dcv.akamai.com ドメインの Akamai から、ドメイン検証チャレンジ・ファイル・オブジェクトのコピーを取得できなければなりません。

#### リダイレクト方式の場合のクリーンアップ

CDN が**「証明書デプロイ中 (Certificate deploying)」**状況に達した後、以下を行います。
1. (オプション) 構成ファイルから redirect ステートメントまたは redirect ブロックを削除します。
1. (オプション) 必要であれば、追加した ServerAlias (Apache2) または server_name (Nginx) をサーバー構成から削除します。
1. CDN ドメインとオリジン・サーバー IP との間の A レコードを削除します。
