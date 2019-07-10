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

# IBM Cloud Object Storage für CDN konfigurieren
{: #configure-ibm-cloud-object-storage-for-cdn}

Um in {{site.data.keyword.cloud}} Object Storage gespeicherte Objekte zu verwenden, müssen Sie als Wert für die Eigenschaft 'acl' (Access Control List, Zugriffssteuerungsliste) für jedes Objekt in Ihrem Bucket den Zugriff 'public-read' festlegen.

Informationen zum Installieren der erforderlichen Clients oder Tools finden Sie im Abschnitt für [IBM Cloud Object Storage-Entwickler](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers). Diese Anleitung geht davon aus, dass Sie die offizielle Befehlszeilenschnittstelle AWS installiert haben, die mit der {{site.data.keyword.cloud_notm}} Object Storage S3-API kompatibel ist.

Der folgende Beispielcode zeigt, wie der Zugriff 'public-read' für alle Objekt in Ihrem Bucket über die Befehlszeilenschnittstelle festgelegt wird.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="IHR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

Weitere Informationen zur Verwendung von {{site.data.keyword.cloud_notm}} Object Storage finden Sie im Abschnitt [CDN mit Cloud Object Storage verwenden, um Ihre statischen Dateien schneller zu liefern](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn).
