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

# 为 CDN 配置 IBM Cloud Object Storage
{: #configure-ibm-cloud-object-storage-for-cdn}

要使用存储在 {{site.data.keyword.cloud}} Object Storage 中的对象，您必须为存储区中的每个对象设置“acl”（即访问控制表）属性的值，以允许“public-read”访问权。

要安装任何必备客户机或工具，请参阅 [IBM Cloud Object Storage Developer 部分](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers)。本指南假设您已安装正式的 AWS 命令行界面，该界面与 {{site.data.keyword.cloud_notm}} Object Storage S3 API 兼容。

下面的示例代码显示如何使用命令行界面，为存储区中的所有对象设置“public-read”访问权。

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

有关如何使用 {{site.data.keyword.cloud_notm}} Object Storage 的更多信息，请参阅[将 CDN 与 Cloud Object Storage 配合使用以更快交付静态文件](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)
