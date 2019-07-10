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

# 配置 CDN 的 IBM Cloud Object Storage
{: #configure-ibm-cloud-object-storage-for-cdn}

若要充分運用 {{site.data.keyword.cloud}} Object Storage 中所儲存的物件，您必須設定儲存區中每個物件的 "acl" 內容值（亦即，存取控制清單）來進行 "public-read" 存取。

若要安裝所有必要的用戶端或工具，請參閱 [IBM Cloud Object Storage 開發人員小節](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers)。本手冊假設您已安裝正式 AWS 指令行介面，其與 {{site.data.keyword.cloud_notm}} Object Storage S3 API 相容。

下列程式碼範例示範如何使用指令行介面來設定儲存區中所有物件的 "public-read" 存取權。

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

如需如何使用 {{site.data.keyword.cloud_notm}} Object Storage 的相關資訊，請參閱[使用 CDN 與 Cloud Object Storage 搭配，以更快速地遞送靜態檔案](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)
