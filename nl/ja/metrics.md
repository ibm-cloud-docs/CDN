---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# メトリック
{: #metrics}

初めてリストから CDN を選択すると、「概要」ページが開きます。 ここには、選択した期間 (デフォルトは 30 日) の「合計処理能力 (Total Bandwidth)」、「合計ヒット数 (Total Hits)」、および「ヒット率」が表示されます。

  ![メトリック概要](images/metrics-overview.png)

概要のすぐ下には、「合計処理能力 (Total Bandwidth)」、「地域ごとの処理能力 (Bandwidth per Region)」、「合計ヒット数 (Total Hits)」、および「タイプ別ヒット数 (Hits By Type)」のグラフィカル表現が表示されます。

  ![メトリックのグラフ](images/metrics-graphs.png)

**注**: CDN の作成後、メトリックが表示されるまでに最大 48 時間かかることがあります。

## メトリックを表示できる最低日数がありますか。 最大日数もありますか。

{{site.data.keyword.cloud}} Content Delivery Network with Akamai を使用してメトリックを表示する場合、表示できる最小日数と最大日数があります。 メトリックは、最低 7 日間収集できます。 メトリックは、最大 90 日間表示できます。 API をご使用のお客様は、タイム・ゾーン間の時差を考慮して、最大として 89 日を使用することをお勧めします。

## 合計ヒット数がゼロのときに、ヒット率がゼロでないのはなぜですか。
ヒット率は、コンテンツがオリジン・サーバーから配信されるのではなく、エッジ・サーバー・キャッシュから配信された回数の比率を表します。 次のように計算されます。

`((エッジ・ヒット数 - 入口ヒット数)/エッジ・ヒット数) * 100`

それぞれの意味は以下のとおりです。

_エッジ・ヒット数_ は、「エンド・ユーザーからエッジ・サーバーへのすべてのヒット」と定義されます。  
_入口ヒット数_ は、「オリジン・ヒットまたは入口ヒットは、オリジンから Akamai エッジ・サーバーへのトラフィックに関するもの」と定義されます。

ヒット率は、CDN 単位ではなくアカウント・レベルで計算されるため、ヒット率はアカウント内のすべての CDN で同じになります。 この事実からも、特定の CDN のエッジ・ヒット数がゼロのときにヒット率がゼロでない場合がある理由を説明できます。

## メトリックはリアルタイムで更新されますか?

いいえ。メトリックは 24 時間ごとに更新されます。
