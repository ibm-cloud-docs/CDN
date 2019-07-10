---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: content delivery network, web content, caching, edge servers, streaming content

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# コンテンツ配信ネットワーク (CDN) について
{: #about-content-delivery-networks-cdn-}

コンテンツ配信ネットワーク (CDN) は、全国または世界中に分散されたエッジ・サーバーの集合です。 その Web コンテンツは、要求元のユーザーに最も近い地理的領域にあるエッジ・サーバーから提供されます。 この技術により、エンド・ユーザーは少ない遅延時間でコンテンツを受け取ることができ、より優れた総合的な経験をカスタマーに提供することができます。

## CDN の動作
{: #how-does-a-cdn-work}

CDN は、世界中のエッジ・サーバー上に Web コンテンツをキャッシュすることで、その目的を果たします。 カスタマーが Web コンテンツを要求すると、そのカスタマーに地理的に最も近いエッジ・サーバーに、コンテンツ要求がルーティングされます。 CDN では、コンテンツの必要移動距離を削減することで、スループットの最適化、最小限の遅延、パフォーマンスの向上が実現されます。 コンテンツ・プロバイダーは、{{site.data.keyword.cloud}} Content Delivery Network with Akamai を使用して、世界中から要求されたコンテンツを最小限の構成で有効に配信することができます。

![CDN 概要図](images/high-level-cdn-diagram.png)

## 機能
{: #features}

{{site.data.keyword.cloud}} Content Delivery Network サービスの主要機能は、以下のとおりです。
  * ホスト・サーバー・オリジンのサポート
  * オブジェクト・ストレージ・オリジンのサポート
  * 明確なパスを使用した複数オリジンのサポート
  * パスベースの CDN マッピング
  * キャッシュ・コンテンツのパージ
  * 存続時間 (TTL)
  * グラフィカル・ビューを使用したメトリック
  * ワイルドカード証明書および DV SAN 証明書を使用した HTTPS プロトコルのサポート
  * ホスト・ヘッダーのサポート
  * 失効コンテンツの提供
  * キャッシュ・キー照会引数
  * コンテンツ圧縮
  * ラージ・ファイルの最適化
  * ビデオ・オンデマンド
  * 地理的アクセス制御
  * ホット・リンク保護

機能の完全な説明については、[機能の説明文書](/docs/infrastructure/CDN?topic=CDN-feature-descriptions)を参照してください。

## アーキテクチャー図
{: #architectural-diagram}

次の図は、IBM Cloud CDN の 3 層アーキテクチャーの概要を図式で示したものです。

![アーキテクチャー図](images/3-tier-architecture.png)
