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

# Configuración de IBM Cloud Object Storage for CDN
{: #configure-ibm-cloud-object-storage-for-cdn}

Para utilizar objetos almacenados en {{site.data.keyword.cloud}} Object Storage, debe establecer el valor de la propiedad "acl" (es decir, la lista de control de accesos) para que cada objeto del grupo disponga de acceso "public-read".

Para instalar los clientes y herramientas necesarios, consulte la [sección de IBM Cloud Object Storage Developer](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers). Esta guía presupone que ha instalado la interfaz de línea de mandatos AWS oficial, que es compatible con la API de {{site.data.keyword.cloud_notm}} Object Storage S3.

El código de ejemplo siguiente es una muestra sobre cómo definir el acceso "public-read" para todos los objetos del grupo mediante la interfaz de línea de mandatos.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

Para obtener más información sobre cómo utilizar {{site.data.keyword.cloud_notm}} Object Storage consulte [Utilización de CDN con Cloud Object Storage para entregar sus archivos estáticos con mayor rapidez](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)
