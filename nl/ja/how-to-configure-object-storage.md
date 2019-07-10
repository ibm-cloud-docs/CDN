---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: object storage, bucket, configuration, details, updating

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# CDN に関する IBM Cloud Object Storage の構成
{: #configure-ibm-cloud-object-storage-for-cdn}

{{site.data.keyword.cloud}} Object Storage に保管されたオブジェクトを利用するには、バケット内の各オブジェクトについて「acl」プロパティー (つまり、アクセス制御リスト) の値を「public-read」のアクセス権限に設定する必要があります。

必要なクライアントまたはツールをインストールするには、[IBM Cloud Object Storage Developer セクション](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers)を参照してください。 このガイドでは、{{site.data.keyword.cloud_notm}} Object Storage S3 API と互換の公式 AWS コマンド・ライン・インターフェースがインストールされていることを前提としています。

以下のサンプル・コードは、コマンド・ライン・インターフェースを使用して、バケット内のすべてのオブジェクトに「public-read」のアクセス権限を設定する方法を示しています。

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

{{site.data.keyword.cloud_notm}} Object Storage の使用方法について詳しくは、[クラウド・オブジェクト・ストレージで CDN を使用して静的ファイルのデリバリーを高速化する](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)を参照してください
