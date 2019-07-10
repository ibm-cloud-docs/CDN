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

# Configuration d'IBM Cloud Object Storage pour CDN
{: #configure-ibm-cloud-object-storage-for-cdn}

Pour pouvoir utiliser des objets stockés dans {{site.data.keyword.cloud}} Object Storage, définissez la propriété "acl" (à savoir, la liste de contrôle d'accès) pour chacun des objets contenus dans votre compartiment sur la valeur d'accès "public-read".

Pour installer tous les clients ou outils nécessaires, voir la [section IBM Cloud Object Storage Developer](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#for-developers). Ce guide suppose que vous avez installé l'interface de ligne de commande AWS officielle, compatible avec l'API {{site.data.keyword.cloud_notm}} Object Storage S3.

L'exemple de code ci-dessous indique comment configurer l'accès "public-read" pour tous les objets de votre compartiment à l'aide de l'interface de ligne de commande.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```

Pour plus d'informations sur l'utilisation d'{{site.data.keyword.cloud_notm}} Object Storage, voir [Accélération de la distribution des fichiers statiques à l'aide d'un CDN avec le stockage d'objets Cloud](https://cloud.ibm.com/docs/tutorials?topic=solution-tutorials-static-files-cdn#accelerate-delivery-of-static-files-using-a-cdn)
