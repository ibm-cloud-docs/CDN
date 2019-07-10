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

# CDN에 대한 IBM Cloud Object Storage 구성
{: #configure-ibm-cloud-object-storage-for-cdn}

{{site.data.keyword.cloud}} Object Storage에 저장된 오브젝트를 사용하려면 버킷에서 각 오브젝트에 대한 "public-read" 액세스의 "acl" 특성(즉, 액세스 제어 목록) 값을 설정해야 합니다.

필요한 클라이언트 또는 도구를 설치하려면 [IBM Cloud Object Storage 개발자 섹션](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers)을 참조하십시오. 이 안내서에서는 {{site.data.keyword.cloud_notm}} Object Storage S3 API와 호환 가능한 공식 AWS 명령행 인터페이스를 설치했다고 가정합니다.

다음 예제 코드는 명령행 인터페이스를 사용하여 버킷에서 모든 오브젝트에 대한 "public-read" 액세스를 설정하는 방법을 표시합니다.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

{{site.data.keyword.cloud_notm}} Object Storage를 사용하는 방법에 대한 자세한 정보는 [Cloud Object Storage 및 CDN을 사용하여 정적 파일 전달 가속화](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)를 참조하십시오.
