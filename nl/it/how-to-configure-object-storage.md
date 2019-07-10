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

# Configura IBM Cloud Object Storage per CDN
{: #configure-ibm-cloud-object-storage-for-cdn}

Per utilizzare gli oggetti memorizzati in {{site.data.keyword.cloud}} Object Storage, devi impostare il valore della proprietà "acl" (ossia, l'elenco di controllo degli accessi) per ogni oggetto nel tuo bucket per l'accesso "public-read".

Per installare gli eventuali client o strumenti necessari, consulta la [sezione IBM Cloud Object Storage Developer](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers). Questa guida presume che tu abbia installato l'interfaccia della riga di comando AWS ufficiale, che è compatibile con l'API {{site.data.keyword.cloud_notm}} Object Storage S3.

Il codice di esempio riportato di seguito mostra come impostare l'accesso "public-read" per tutti gli oggetti presenti nel tuo bucket, utilizzando l'interfaccia riga di comando.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

Per ulteriori informazioni su come utilizzare {{site.data.keyword.cloud_notm}} Object Storage fai riferimento a [Accelera la fornitura di file statici utilizzando una CDN](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)
