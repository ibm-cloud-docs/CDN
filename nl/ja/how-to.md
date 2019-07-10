---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# CDN の管理
{: #manage-your-cdn}

本資料では、CDN を管理するための一般的なタスクについて説明します。

## 「存続時間」を使用したコンテンツ・キャッシング時間の設定
{: #setting-content-caching-time-using-time-to-live}

CDN が実行中になった後、存続時間 (TTL) を使用してコンテンツ・キャッシング時間を設定できます。 特定のファイルまたはディレクトリー・パスの存続時間は、そのコンテンツをキャッシュに入れる時間の長さを示します。 CDN マッピングを作成したとき、デフォルトのグローバル TTL として 3600 秒 (1 時間) が作成されています。

**ステップ 1:**  

CDN ページで、CDN を選択します。これにより、**「概要」**ページに移動します。

**ステップ 2:**  

矢印を使用して、または新しい時間を入力することで、時間を調整できます。 時間値は秒単位で指定します。 例えば、3600 秒は 1 時間に相当します。 `timeToLive` に選択できる最小値は 0 秒、最大値は 2147483647 秒 (約 24855 日) です。 **「保存」**ボタンを選択すると、コンテンツ・キャッシング時間が設定されます。

  ![TTL の追加](images/adding-path.png)

**ステップ 3:**

保存したら、オーバーフロー・メニュー・オプションを使用して、TTL 設定の**「編集」**または**「削除」**を行えます。 (**注**: TTL のパスは変更できません。 マッピング・パスが変更されると、TTL パスが自動的に更新されます。)

  ![TTL の編集または削除](images/edit-delete-ttl-setting.png)  

  * コンテンツが複数のルールに一致する場合は、最も新しく追加された構成が優先されます。

  * TTL 値は、特定のファイル名またはディレクトリー対してのみ設定可能です。 正規表現は、予測不能の振る舞いを生成する可能性があるため、サポートされません。

## オリジン・パスの詳細の追加
{: #adding-origin-path-details}

CDN の状況が*「CNAME 構成」* または*「実行中」* の場合、オリジン・パスの詳細を追加できます。 複数のオリジン・サーバーからのコンテンツの提供を選択できます。 例えば、ビデオ用とは異なるサーバーから、写真を配信することができます。 オリジンは、ホスト・サーバーまたはオブジェクト・ストレージをベースとすることができます。

CDN は、オリジン・サーバーの URL 変換を行います。例えば、オリジン `xyz.example.com` がパス `/example/*` を指定して追加された場合、ユーザーが URL `www.example.com/example/*` を開いたときに、CDN エッジ・サーバーは、`xyz.example.com/*` からコンテンツを取得します。
{: note}

**ステップ 1:**

CDN ページで、CDN を選択します。これにより、**「概要」**ページに移動します。  

**ステップ 2:**

**「オリジン (Origins)」**タブを選択し、**「オリジンの追加 (Add Origin)」**ボタンを選択します。 このステップにより、オリジンを構成できる新規ダイアログ・ウィンドウが開きます。  

   ![オリジン、オリジンの追加](images/add-origins.png)

**ステップ 3:**

*必ず*、パスを指定してください。 オプションでホスト・ヘッダーを指定することができます。  

   ![オリジン、オリジンの追加](images/add-origin-path.png)

**ステップ 4:**

**「サーバー」**または**「オブジェクト・ストレージ」**のいずれかを選択します。

  * **「サーバー」**を選択した場合は、オリジン・サーバー・アドレスを IPv4 アドレスとして入力するか、または _hostname_ を入力します。 ホスト名と完全修飾ドメイン・ネーム (FQDN) を指定することをお勧めします。 CDN の作成時に選択したプロトコルに応じて、HTTP ポートまたは HTTPS ポート、あるいはその両方も指定します。 HTTPS ポートを使用する場合は、オリジン・サーバー・アドレスは、IP アドレスではなく_ホスト名_ **でなければなりません**。

       ![オリジン・サーバーの追加](images/add-origin-server-default.png)

  * **「オブジェクト・ストレージ」**を選択した場合は、エンドポイント、バケット名、および HTTPS ポートを指定します。 オプションで、CDN サービスで使用できるファイル拡張子を指定します。 何も指定しないと、すべてのファイル拡張子が許可されます。

       ![オリジンのオブジェクト・ストレージの追加](images/add-origin-object-storage.png)

  * **「最適化 (Optimization)」**オプションと**「キャッシュ・キー (Cache Key)」**オプションは、サーバー構成とオブジェクト・ストレージ構成で同じです。

    * ドロップダウン・メニューから**「最適化 (Optimization)」**オプションを選択します。 **「一般 Web 配信 (General web delivery)」**がデフォルト・オプションです。**「ラージ・ファイル (Large file)」**または**「ビデオ・オンデマンド (Video on demand)」**の最適化を選択することもできます。 **「一般 Web 配信 (General web delivery)」**を使用すると、CDN は 1.8 GB までのコンテンツを提供でき、**「ラージ・ファイル (Large file)」**最適化を使用すると、1.8 GB から 320 GB までのファイルをダウンロードできます。 **「ビデオ・オンデマンド (Video on demand)」**は、セグメント化されたストリーミング形式の配信用に CDN を最適化します。 [ラージ・ファイルの最適化](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization)と[ビデオ・オンデマンド](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand)の機能の説明に、詳細情報が記載されています。

        ![パフォーマンス構成オプション](images/performance-config-options.png)

    * ドロップダウン・メニューから**「キャッシュ・キー (Cache Key)」**オプションを選択します。 デフォルト・オプションは**「すべてを含める (Include-all)」**です。 **「指定を含める (Include specified)」**または**「指定を無視 (Ignore specified)」**を選択した場合、含めるか無視する照会ストリングをスペースで区切って入力する **必要があります**。 例えば、単一の照会ストリングの場合は `uuid=123456` と入力し、2 つの照会ストリングの場合は `uuid=123456 issue=important` と入力します。  機能の説明に、[キャッシュ・キーの照会引数](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args)に関する詳細が記載されています。

        ![キャッシュ・キー・オプション](images/cache-key-options.png)

UI で表示されるプロトコルおよびポートのオプションは、CDN の注文時に選択した内容と一致します。例えば、CDN の注文の一部として**「HTTP ポート」**を選択した場合は、**「HTTP ポート」**オプションのみが「オリジンの追加 (Add Origin)」の一部として表示されます。
{: note}

**ステップ 5:**

**「追加」**ボタンを選択して、オリジン・パスを追加します。

オブジェクト・ストレージのオリジン・パスにファイル拡張子を指定すると、オリジン・パスと同じ URL を持つ TTL 設定の有効範囲が、これらの指定されたファイル拡張子を持つすべてのファイルになります。例えば、オリジン・パス `/example` を作成し、ファイル拡張子「jpg png gif」を指定すると、TTL パス `/example` の TTL 値の有効範囲には、`/example` ディレクトリーとそのサブディレクトリーにあるすべての JPG、PNG、GIF のファイルが含まれます。
{: note}

**ステップ 6:**

追加したら、オーバーフロー・メニュー・オプションを使用して、オリジンの**「編集」**または**「削除」**を行えます。

  ![オリジンの編集または削除](images/edit-delete-origin.png)

## キャッシュ・コンテンツのパージ
{: #purging-cached-content}

CDN が実行中になった後、ベンダーのサーバーからキャッシュ・コンテンツをパージできます。

**ステップ 1:**

CDN ページで、CDN を選択します。これにより、**「概要」**ページに移動します。

**ステップ 2:**

**「パージ (Purge)」**タブを選択します。

   ![「パージ (Purge)」ページ](images/purge_tab.png)

**ステップ 3:**

パージするファイルを示した標準の UNIX パス構文を入力し、**「パージ (Purge)」**ボタンを選択します。一度に 1 つのファイルのみをパージできます。パージ・パスに使用できる構文について詳しくは、『[ルールおよび命名規則](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-)』ページを参照してください。

**ステップ 4:**

パージすると、このアクティビティーが**「パージ・アクティビティー (Purge Activity)」**の下にリストされます。 オーバーフロー・メニュー・オプションを使用して、**「パージの再実行 (Redo purge)」**、またはパスの**「お気に入り (Favorite)」**の登録を行えます。

   ![パージ・アクティビティー](images/purge-activity.png)

15 を超えるパージがある場合、パージ・アクティビティーは 15 日ごとに自動的に削減されます。
{: note}

## CDN 構成の詳細の更新
{: #updating-cdn-configuration-details}

CDN が実行中になった後、CDN 構成の詳細を更新できます。

**ステップ 1:**

CDN ページで、CDN を選択します。これにより、**「概要」**ページに移動します。

**ステップ 2:**

**「設定」**タブを選択します。 CDN 構成の詳細が表示されます。

   ![「設定」タブ](images/settings-tab.png)  

CDN が HTTPS で構成されている場合のみ SSL 証明書が表示されます。
{: note}

**「サーバー」**の場合、以下のフィールドを変更できます。
  * ホスト・ヘッダー (Host header)
  * オリジン・サーバー・アドレス
  * HTTP/HTTPS ポート
  * 失効コンテンツの提供
  * ヘッダーを尊重 (Respect Headers)
  * 最適化オプション (Optimization options)
  * キャッシュ照会 (Cache-query)    

**「オブジェクト・ストレージ」**では、以下のフィールドを変更できます。
  * ホスト・ヘッダー (Host header)
  * エンドポイント
  * バケット名
  * HTTPS ポート
  * 許可されるファイル拡張子
  * 失効コンテンツの提供
  * ヘッダーを尊重 (Respect Headers)
  * 最適化オプション (Optimization options)
  * キャッシュ照会 (Cache-query)

**ステップ 3:**

必要であれば、**「オリジン (Origin)」**または**「その他のオプション (Other Options)」**の詳細を更新し、右下隅の**「保存」**ボタンをクリックして、CDN 構成の詳細を更新します。

   ![「保存」ボタン](images/save-button.png)
