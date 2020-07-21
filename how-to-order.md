---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-26"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Ordering a CDN
{: #order-a-cdn}
{: help}
{: support}

To order an {{site.data.keyword.cloud}} Content Delivery Network (CDN), you must first create your CDN through the {{site.data.keyword.cloud_notm}} catalog. Depending on the configuration you select, then complete instructions in [Getting to Running status](/docs/CDN?topic=CDN-getting-to-running-status).
{: shortdesc}

1. From your browser, open the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} and log in to your account.
1. Select **Networking** in the navigation pane, then click the **Content Delivery Network** tile.
1. Click **Create** in the lower right corner of the page to create your CDN account.
1. On the CDN Configure page, complete the **Configure name** section.

   * Type the required **Hostname**, which serves as the primary identifier for your CDN (for example, `example.testingcdn.net`).  
   * Optionally, provide a custom **CNAME**, such as `myfirstcdn.cdn.appdomain.cloud.`. If no CNAME is provided, one is created for you. The suffix `cdn.appdomain.cloud.` is automatically appended to the CNAME. Use of an inappropriate CNAME can lead to end of services.

1. Complete the **Configure origin** section. You must select either the **Server** or the **Object Storage** tab.

    * Type the optional **Host header**. If one is not provided, it defaults to the **hostname**. See [Host header support](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#host-header-support) for more information.  
    * Provide an optional **Path** where content can be retrieved from on the Origin. See [Path-based CDN mappings](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#path-based-cdn-mappings) to understand the implications of adding a path at this time.

      For **Server**:  
       * Type the **Origin server address** (hostname or IPv4 address of the Origin server). If **HTTPS port** is also selected, the **Origin server address** must be the hostname, not the IP address.

      For **Object Storage**:
       * Type the required **Endpoint** from which to fetch the object.
       * Type the name of the **Bucket** in which your content is stored.  
         You must set the **Access Control List** (ACL) for each Object in your Bucket to "public-read" with your Cloud Object Storage provider.
         {: note}

    * Provide an **HTTP port**, an **HTTPS port**, or both. These fields indicate which protocol and port number can be used to contact the Origin server. For non-default port numbers, refer to [FAQs](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed) for a list of allowed port numbers.

      An **SSL certificate** option appears if you select HTTPS port. You can choose **Wildcard** or **DV SAN certificate** as your SSL certificate option. Both offer the enhanced security provided by HTTPS.
    * **DV SAN certificate** (default) allows HTTPS traffic over your domain, but requires additional steps to verify. See [Completing Domain Control Validation for HTTPS](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) to understand the steps that are required and time constraints that are involved with choosing this option.
    * **Wildcard certificate** allows HTTPS traffic only when using the **CNAME** and requires no further action on your part.

1. Review pricing estimates in the right pane.
1. Agree to the terms of the Master Service Agreement.
1. Click the **Create** button to create your CDN.
1. Follow steps for your specific configuration in [Getting to Running status](/docs/CDN?topic=CDN-getting-to-running-status).
