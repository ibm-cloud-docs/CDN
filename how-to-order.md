---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-23"

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

# Ordering a CDN
{: #order-a-cdn}

Learn how to order a Content Delivery Network (CDN). Your CDN can be ordered from the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/login).

## Preparing to order
{: #preparation-for-ordering}

Here's how to navigate to the CDN page to place your order.
1. Log in to your account from the [IBM Cloud console](https://cloud.ibm.com/login).
1. Click [Catalog](https://cloud.ibm.com/catalog/).
1. In the navigation pane, click **Networking**.
1. Click the **Content Delivery Network** tile.

## Ordering a new CDN
{: #order-a-new-cdn}

After you're on the ordering page, here's how to proceed to create and configure your CDN.

### Step 1: Creating your CDN account
{: #create-your-cdn-account}

Click **Create** at the lower right, which creates your CDN account if you don't have one already and redirects you to the CDN Configure screen.

   ![CDN overview](images/content-delivery.png)

### Step 2: Naming your CDN
{: #step-2-name-your-cdn}

Complete the **Configure Name** field:  

  * Specify the **Hostname** (**required**), which serves as the primary identifier for your CDN (for example, `example.testingcdn.net`).  
  * Optionally, you can provide a custom **CNAME** (such as `myfirstcdn.cdn.appdomain.cloud`). If no CNAME is provided, one is created for you. The suffix `cdn.appdomain.cloud` is automatically appended to the CNAME. Use of an inappropriate CNAME can lead to termination of services.

       ![Configure Name](images/configure-hostname-cname.png)  

After provisioning your new CDN, you **must** configure the CNAME with your DNS provider.
{: note}
### Step 3: Configuring your origin
{: #step-3-configure-your-origin}

Complete the **Configure Your Origin** field: To configure this field, you must select either the **Server** or the **Object Storage** option.  

  * **Step 3, Option 1: The Server Option**

     ![Configure Origin](images/configure-origin-server.png)

      * You must specify the **Origin Server Address** (hostname or IPv4 address of the Origin Server). If **HTTPS port** is also selected, the **Origin Server Address** must be the hostname and not IP address.

      * Specify the **Host header** (optional). If one is not provided, it defaults to the **Hostname**. See the feature description for [Host header support](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#host-header-support) for more information about the Host header.  

      * Provide a **Path** where content can be retrieved from on the Origin (optional). See the feature description for [Path-based CDN mappings](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#path-based-cdn-mappings) to understand the implications of adding a path at this time.

      * You can also provide an **HTTP port**, an **HTTPS port**, or both. These fields indicate which protocol and port number can be used to contact the Origin server. For non-default port numbers, refer to [the FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for a list of allowed port numbers.

      * **SSL Certificate** This option appears _only_ when HTTPS Port is selected. If you select **HTTPS Port** for either Server or Object Storage, you can choose **Wildcard** or **DV SAN Certificate** as your SSL Certificate option. Both offer the enhanced security provided by HTTPS.
         * **Wildcard Certificate** allows HTTPS traffic only when using the **CNAME** and requires no further action on your part
         * **DV SAN Certificate** allows HTTPS traffic over your domain, but requires additional steps to verify. See the [Completing Domain Control Validation for HTTPS](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https) page to understand the steps that are required and time constraints that are involved with choosing this option.

	     ![Configure origin server](images/ssl-cert-options.png)

  * **Step 3, Option 2: The Object Storage Option**

    ![Configure Object storage](images/configure-origin-object-storage.png)

      * You **must** specify the **Endpoint** from which to fetch the object.

      * Specify the **Host header** (optional). If one is not provided, it defaults to the **Hostname**. See the feature description for [Host header support](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#host-header-support) for more information about the Host header.  

      * Provide a **Path** where content can be retrieved from on the Origin (optional). See the feature description for [Path-based CDN mappings](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#path-based-cdn-mappings) to understand the implications of adding a path here.

      * You **must** provide the name of the **Bucket** in which your content is stored.

      * You can also provide an **HTTP port**, an **HTTPS port**, or both. These fields indicate which protocol and port number can be used to contact the Origin server. For non-default port numbers, refer to [the FAQ](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for a list of allowed port numbers.

      * **SSL Certificate** This option appears _only_ when HTTPS Port is selected. If you select **HTTPS Port** for either Server or Object Storage, you can choose **Wildcard** or **DV SAN Certificate** as your SSL Certificate option. Both offer the enhanced security provided by HTTPS.
         * **Wildcard Certificate** allows HTTPS traffic only when using the **CNAME** and requires no further action on your part
         * **DV SAN Certificate** allows HTTPS traffic over your domain, but it requires additional steps to verify. See the [Completing Domain Control Validation for HTTPS](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https) page to understand the steps that are required and time constraints that are involved with choosing this option.

        ![Configure HTTPS](images/ssl-cert-options.png)

You must set the **Access Control List** (ACL) for each Object in your Bucket to "public-read" with your Cloud Object Storage provider.
{: note}

### Step 4 Accepting the agreement
{: #step-4}

* Select **I have read the Master Service Agreement and agree to the terms therein**.

* Then, click the **Create** button to create your CDN.
