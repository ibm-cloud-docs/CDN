---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 制限および最大値
{: #limits-and-maximum-values}

## 存続時間の最大値はありますか。 最小値もありますか。
{: #is-there-a-maximum-value-for-time-to-live}

存続時間の最大値は 2,147,483,647 秒で、これは約 68 年と同等です。 最小値は 0 秒です。

## オリジンと TTL の項目数に制限はありますか
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

はい。両方を合計した制限は、CDN につき 75 項目です。

## Akamai CDN から配信できる最大ファイル・サイズは
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

1.8 GB より大きいファイルを取得したり配信したりしようとすると、デフォルトのパフォーマンス構成では`「403 アクセス禁止」`応答が表示されます。 ラージ・ファイルの最適化が有効になっている場合、320 GB までのファイル・ダウンロードが可能です。

## オリジンの応答ヘッダーの最大サイズは
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

応答ヘッダーの最大サイズは 16KB です。特定のオリジンが設定しようとしている Cookie が大きすぎて、クライアントがそれ以降の要求で返すことができない場合、Akamai はこの制限をエッジ・サーバーに設定します。応答ヘッダーが 16KB より大きい場合、要求は `502 Bad Gateway` 応答を取得します。

## クライアントの要求ヘッダーの最大サイズは
{: #what-is-the-maximum-size-for-the-client-request-headers}

要求ヘッダーの最大サイズは 32KB です。要求ヘッダーが 32KB より大きい場合、Akamai エッジ・サーバーは `400 Bad Request` 応答を返します。
