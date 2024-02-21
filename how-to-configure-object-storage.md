---

copyright:
  years: 2017, 2020
lastupdated: "2020-12-23"

keywords: object storage, bucket

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Configuring CDN with IBM Cloud Object Storage
{: #configure-cdn-with-ibm-cloud-object-storage}

You can use {{site.data.keyword.cloud}} Content Delivery Network (CDN) to quickly and securely deliver the website assets (images, videos, documents) hosted and served on the IBM Cloud Object Storage to users around the world.

## Before you begin
{: #cos-before-you-begin}

Before you begin using CDN with IBM Cloud Object Storage (ICOS), you'll first need to store objects with [public access](/docs/cloud-object-storage?topic=cloud-object-storage-iam-public-access) in {{site.data.keyword.cloud_notm}} Object Storage.

## Create a CDN instance with ICOS
{: #create-a-cdn-instance-with-icos}

1. Follow the [guidelines to create a CDN instance](/docs/CDN?topic=CDN-order-a-cdn) with **Object Storage** type of origin to configure your S3 **Endpoint** and **Bucket name** with public access objects stored in the ICOS.
1. Follow the [guidelines to get your CDN instance into RUNNING status](/docs/CDN?topic=CDN-next-steps-after-ordering).
1. Access your ICOS objects with CDN URL: `http://<hostname>/<path>/index.html`.

For more information, see [Accelerate delivery of static files using a CDN](/docs/solution-tutorials?topic=solution-tutorials-static-files-cdn).

## FAQs for CDN with ICOS
{: #faqs-for-cdn-with-icos}
{: faq}

### Can I use ICOS private endpoints for CDN?
{: #use-objects-stored-in-icos}

No. The CDN edge servers can only access the ICOS public endpoints, so objects in the ICOS buckets should provide public access.

### Will traffic sent from ICOS to CDN be charged?
{: #traffic-from-icos-to-cdn-charge}

Yes. The CDN and ICOS don't have a way of measuring each other's traffic, so both traffic from ICOS and CDN are charged.

### How does the CDN edge server retrieve content from the ICOS objects?
{: #cdn-edge-server-retrieve-content}

The CDN uses the S3 endpoint to access the ICOS objects, and replaces the path in the url to the bucket name. For example, if your ICOS S3 endpoint `s3.us-south.cloud-object-storage.appdomain.cloud` with bucket name `xyz-bucket-name` is added in path `/example-cos/*`, when you open the CDN URL `www.example.com/example-cos/*`, the CDN edge server retrieves the content from `s3.us-south.cloud-object-storage.appdomain.cloud/xyz-bucket-name/*`.

### How do I use the ICOS default index page with CDN?
{: #using-icos-default-index-with-cdn}

The CDN does not support the default index page for the ICOS objects because the ICOS S3 endpoint does not have the default index. You must specify the complete request path in the browser's address bar (for example, `www.example.com/index.html`). If you want the CDN to automatically access the default index page of the ICOS, create a CDN with **Server** type of origin instead of an **Object Storage** type. For example, if you have a ICOS with static website hosting endpoint `xyz-bucket-name.s3-web.us-south.cloud-object-storage.appdomain.cloud`, you can create a CDN `www.example.com` with server type of origin `xyz-bucket-name.s3-web.us-south.cloud-object-storage.appdomain.cloud` in path `/*`, when a user opens the CDN URL `www.example.com`, the CDN edge server retrieves the content from `xyz-bucket-name.s3-web.us-south.cloud-object-storage.appdomain.cloud/`, and ICOS will return your default index page.
