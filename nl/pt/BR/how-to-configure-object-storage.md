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

# Configure o IBM Cloud Object Storage para CDN
{: #configure-ibm-cloud-object-storage-for-cdn}

Para fazer uso de objetos armazenados no {{site.data.keyword.cloud}} Object Storage, deve-se configurar o valor da propriedade "acl" (ou seja, a lista de controle de acesso) para cada objeto em seu depósito para acesso "public-read".

Para instalar quaisquer clientes ou ferramentas necessários, consulte a [seção do IBM Cloud Object Storage Developer](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers). Este guia supõe que você instalou a interface de linha de comandos do AWS oficial, que é compatível com a API S3 do {{site.data.keyword.cloud_notm}} Object Storage.

O código de exemplo abaixo mostra como configurar o acesso de "leitura pública" para todos os objetos em seu depósito, usando a interface da linha de comandos.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

Para obter mais informações sobre como usar o {{site.data.keyword.cloud_notm}} Object Storage, consulte [Usando o CDN com o Cloud Object Storage para entregar seus arquivos estáticos mais rapidamente](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)
