---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-11"

keywords:

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Updating CDN configuration details
{: #updating-cdn-configuration-details}

After your CDN is running, you can update CDN configuration details. Follow these steps.
{: shortdesc}

1. On the CDN page, select your CDN, which takes you to the **Overview** page.
2. Select the **Settings** tab. Your CDN configuration details are displayed.

   ![Settings tab](images/settings-tab.png){: caption="Setting tab" caption-side="top"}

   You only see **SSL Certificate** if your CDN was configured with HTTPS.
   {: note}

   For **Server**, the following fields can be changed:
      * Host header
      * Origin server address
      * HTTP/HTTPS Port
      * Serve Stale Content
      * Respect Headers
      * Optimization options
      * Cache-query

   For **Object Storage**, the following fields can be changed:
      * Host header
      * Endpoint
      * Bucket name
      * HTTPS Port
      * Allowed file extensions
      * Serve Stale Content
      * Respect Headers
      * Optimization options
      * Cache-query

3. Update the **Origin** or **Other Options** details if needed, then click the **Save** button in the lower right corner to update your CDN configuration details.
