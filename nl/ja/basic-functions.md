---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: running status, additional steps, stop cdn, learn, configure cname, delete cdn, start cdn

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:warning: .warning}
{:download: .download}

# CDN を実行中状況にする
{: #getting-your-cdn-to-running-status}

CDN 実行中状態にするためのガイドラインについて説明します。 また、この文書では、CDN の開始および停止の方法についても説明します。

## 実行中にする
{: #get-to-running}

CDN を作成すると、CDN ダッシュボードにそれが表示されます。 ここには、CDN の名前、オリジン、プロバイダー、状況が示されます。  

 ![マッピング・リストのスクリーン・ショット](images/mapping-list.png)


HTTP、または、ワイルドカード証明書での HTTPS を指定して CDN を注文した場合は、ステップ 1 に進むことができます。

HTTPS DV SAN 証明書での CDN を作成した場合、ドメインを検証するための追加ステップが必要になる場合があります。それについては、『[HTTPS のためのドメイン制御検証の実行](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https)』ページに説明があります。

**ステップ 1:**

CDN を注文したら、DNS プロバイダーで **CNAME** を構成する必要があります。 ほとんどの DNS プロバイダーは、CNAME の設定または変更の手順を提供しています。

   * この期間中、CDN の状況は**「CNAME 構成 (CNAME Configuration)」**と表示されます。 変更がいつアクティブになるかを、DNS プロバイダーに確認してください。

   ![CNAME 構成](images/cname-config.png)  

**ステップ 2:**

DNS プロバイダーで CNAME を構成したらいつでも、CDN の状況の右側にあるオーバーフロー・メニューから**「状況の取得 (Get Status)」**を選択して、状況をチェックできます。

  ![CNAME getStatus](images/cname-getstatus.png)  

**ステップ 3:**

CNAME のチェーニングが完了したら、**「状況の取得 (Get Status)」**を選択すると状況が*「実行中 (RUNNING)」*に変わり、CDN は使用する準備ができた状態になります。

これで、CDN は実行中になりました。 ここからは、[「CDN の管理」](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#manage-your-cdn)ページに、構成オプションについての追加情報が示されるようになります。例えば、[存続時間](docs/infrastructure/CDN?topic=CDN-manage-your-cdn#setting-content-caching-time-using-time-to-live-)、[キャッシュ・コンテンツのパージ](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#purging-cached-content)、[オリジン・パスの詳細の追加](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#adding-origin-path-details)などです。

## CDN の開始
{: #starting-cdn}

CDN を開始すると、トラフィックをオリジンから Akamai エッジ・サーバーへ送信するよう DNS に通知されます。 マッピングが開始された後、DNS キャッシュは引き続きトラフィックをオリジンに送信する可能性があるため、マッピングの開始直後は、ドメインによって機能が認識されない場合があります。 更新にかかる時間は、DNS キャッシュがリフレッシュされる頻度に依存し、DNS プロバイダーによっても異なります。

CDN を開始できるのは、`「停止」`状況の場合のみです。
{: note}

**ステップ 1:**

オーバーフロー・メニュー (CDN 行の右側に 3 つのドットとして表示される) から**「CDN の開始 (Start CDN)」**をクリックします。

  ![オーバーフロー・メニュー](images/start_cdn.png)

**ステップ 2:**

サービス開始の確認を求める大きめのダイアログ・ウィンドウが表示されます。 **「確認」**を選択して続行します。

**ステップ 3:**

アクションが成功すると、画面の右上隅にダイアログ・ボックスが表示され、成功したこととその時間を知らせます。

**ステップ 4:**

このステップにより、状況が`「CNAME 構成 (CNAME Configuration)」`に変わります。

**ステップ 5:**

オーバーフロー・メニューから**「状況の取得 (Get Status)」**をクリックします。 このステップにより、状況が`「実行中」`に変わります。 CDN は作動可能になります。

## CDN の停止
{: #stopping-a-cdn}

STOP CDN 機能は、7 日以内の保守ウィンドウを対象としています。7 日後に CDN を起動する必要があります。そうしないと、この機能は無効になり、CDN CNAME へのトラフィックはオリジン・サーバーにリダイレクトされません。
{: important}

マッピングが停止すると、DNS 参照はオリジンに切り替わります。 トラフィックは CDN エッジ・サーバーをスキップし、コンテンツはオリジンから直接フェッチされます。 マッピングが停止した後は、短時間、コンテンツにアクセスできないことがあります。 これは、DNS キャッシュが引き続きトラフィックを Akamai エッジ・サーバーに送信している可能性があるためです。 ただし、この間、Akamai エッジ・サーバーはドメインのトラフィックを拒否します。 この期間がどれくらい続くかは、DNS キャッシュがリフレッシュされる頻度に依存し、DNS プロバイダーによっても異なります。

CDN の停止は、状況が`「実行中」`のときにのみ行えます。
{: note}

HTTPS SAN 証明書を使用して構成された CDN では、CDN を`「実行中」`状況に戻したときに HTTPS トラフィックが機能しなくなる可能性があるため、CDN を停止することは推奨**されません**。
{: important}

ワイルドカード・ドメインの CDN の停止は、現時点では許可**されていません**。
{: important}

**ステップ 1:**

オーバーフロー・メニュー (CDN 状況の右側にある縦の 3 つのドット) から「CDN の停止 (Stop CDN)」をクリックします。
 ![オーバーフロー・メニュー](images/stop_cdn.png)

**ステップ 2:**

サービス停止の確認を求める大きめのダイアログ・ウィンドウが表示されます。 **「確認」**を選択して続行します。

**ステップ 3:**

およそ 5 秒から 15 秒後に、状況が「停止 (Stopped)」に変わります。

## CDN の削除
{: #deleting-a-cdn}

CDN を削除するには、以下のステップを実行します。

オーバーフロー・メニューから`「削除」`を選択すると、CDN のみが削除されます。ご使用のアカウントは削除されません。
{: note}

**ステップ 1:**

オーバーフロー・メニューから「削除」をクリックします。

 ![CDN の削除のオーバーフロー・メニュー](images/delete_cdn.png)

**ステップ 2:**

削除の確認を求める大きめのダイアログ・ウィンドウが表示されます。 **「削除」**をクリックして続行します。

CDN が DV SAN 証明書で HTTPS を使用して構成されている場合、削除処理が完了するまでに最大 5 時間かかることがあります。
{: note}

  ![警告を伴う削除](images/delete-with-warning.png)

**ステップ 3:**

ステップ 1 と 2 を完了すると、CDN の状況は`「削除中 (Deleting)」`になります。 削除プロセスが完了し、オーバーフロー・メニューから再度「状況の取得 (Get Status)」をクリックすると、CDN リストからその行が削除されます。 削除プロセスが完了していない場合、この操作を行っても効果はありません。
