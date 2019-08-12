---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-08-06"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# FAQs
{: #faqs}

## What is a Content Delivery Network (CDN)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

A Content Delivery Network (CDN) is a collection of edge servers that are distributed through various parts of the country or the world. Their web content is served from an edge server, which is located in the geographic area closest to the customer who requests the content. This technique lets the users receive the content with less delay than we could achieve by delivering the content from one centralized location. It delivers a better overall experience for your customers.

## How does a Content Delivery Network (CDN) work?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

A CDN achieves its purpose by caching web content on edge servers around the world. When a user requests web content, the content request is routed to the edge server that is geographically closest to that user. By reducing the distance the content must travel, the CDN offers optimized throughput, minimized latency, and increased performance.

## How is my IBM Cloud Content Delivery Network service account created?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

Your account is created during the CDN order process. If you are creating a CDN from the Legacy portal, when you click the **Order CDN** button, under the **Network -> CDN page**, your account is created. If you are creating a CDN from the {{site.data.keyword.cloud}} portal, when you click the **Create** button, under the **Catalog -> Network -> Content Delivery Network** page, your account is created.

## What do I do when my CDN is in CNAME Configuration Status?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

For HTTP and SAN Certificate based HTTPS CDN, update your DNS record so that your website points to the `CNAME` associated with your new CDN mapping. For Wildcard Certificate based HTTPS CDN, this DNS update is **NOT** needed.

**Note**: It may take up to 15-30 minutes for the update to become active. Check with your DNS provider to obtain an accurate time estimate.

## How do I add a CNAME record for my CDN domain in DNS?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

In your DNS configuration page for your CDN domain, you can create a CNAME record with CDN Domain name as the Host and the IBM CNAME you used to configure the CDN, as the CNAME. The IBM CNAME ends with `cdn.appdomain.cloud`.

A typical CNAME record would look like the following on the DNS configuration page:

| **Resource Type** | **Host** | **Points to (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdn.appdomain.cloud | 15 minutes |


## What will I be billed for in my CDN
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

You are only billed for bandwidth used per IBM Cloud Content Delivery Network instance. If no bandwidth is used by your CDN, no charges are incurred. Bandwidth prices vary, depending on the regional location of the edge server. You can see bandwidth pricing by geographic region in the [pricing document](/docs/infrastructure/CDN?topic=CDN-pricing) for this service.

## When will I be billed for my CDN?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

IBM Cloud Content Delivery Network billing occurs according to the billing period established in your {{site.data.keyword.BluSoftlayer_notm}} Account.

## If I select `delete` from the CDN's overflow menu, does that delete my account?
{: faq}

No; it will only delete that CDN. Your account still exists, and you can create additional CDNs.

## Does content caching use push or pull?
{: #does-content-caching-use-push-or-pull}
{: faq}

Content Caching is done using an _origin pull_ model. Origin Pull is a method by which data is "pulled" by the edge server from the Origin Server, as opposed to manually uploading the content onto the edge server.

## Is there a recommended browser to use for CDN service configuration?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Yes, Firefox and Chrome are the recommended browsers. It is recommended that you use their latest versions with your IBM Cloud Content Delivery Network.

## What is the purpose of providing a path when creating my CDN?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

If you provide a path while creating your CDN, it allows you to isolate the files that can be served through CDN from a particular Origin Server.

## My CDN is in an Error State. What do I do now?
{: faq}

Please refer to the [Troubleshooting](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) or [Getting Help and Support](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp) pages, or open a ticket in the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).

## Where do I find the CNAME for my CDN if I didn't provide one?
{: faq}

Click on your CDN to access the **Overview** Page in the Portal. On the right corner you can see a **Details** section with the `CName` information.

## My Purge Request for a given file path is in progress. Can I submit a new request for the same file path?

No. There can only be one active Purge request for a given file path at a time.

## Is Internet Protocol version 6 (IPv6) supported with the IBM Cloud Content Delivery Network service? How does it work?
{: faq}

IPv6 (or dual stack support) is supported by Akamai's Edge servers. It is designed to help customers with IPv4 only origin to accept connections from IPv6 clients, convert from IPv6 to IPv4 at the Edge and go forward to the origin with IPv4.

**NOTE:** Creating an IBM Cloud CDN using an IPv6 address as the Origin Server Address is not supported.

## Are there any restrictions on what HTTP and HTTPS port numbers are allowed for Akamai?
{: faq}

Yes. For the Akamai vendor, only the following port numbers are allowed:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410, and 45002.

## What URL should be used for access to data under the CDN or Origin Path?
{: faq}

The path for a CDN mapping or for the origin is treated as a directory. Therefore, users trying to access the origin path should access it as a directory (with a slash). For example, if CDN `www.example.com` is created using the path that includes the `/images` directory, the URL to reach it should be `www.example.com/images/`

Omitting the slash, for example, using `www.example.com/images` will result in a **Page Not Found** error.

## How do I set up my Content Delivery Network for IBM Cloud Object Storage (COS)?
{: faq}

[Here's a tutorial](/docs/tutorials?topic=solution-tutorials-static-files-cdn) on creating a Content Delivery Network for IBM Cloud Object Storage.

## I received notification that my Origin Certificate is expiring. What do I do now?
{: faq}

Follow the steps outlined in [this article ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://community.akamai.com/docs/DOC-7708) from Akamai.

## What security is included with the IBM CDN solution with Akamai?
{: faq}

Using the distributed Akamai platform, you get unparalleled scale and resiliency with thousands of servers in over 50 countries. The Akamai Platform stands between your infrastructure and your end users, and it acts as first level of defense for sudden surges in traffic. Akamai Intelligent Platform also is a reverse proxy that listens and responds to requests on ports 80 and 443 only, which means that traffic on other ports is dropped at the edge without being forwarded to your infrastructure.

## Are cookies from the origin server preserved by the Akamai CDN?
{: faq}

For non-cacheable content, or any content that is not cached, cookies are preserved from the origin. For content that is cached by Edge servers, cookies are not preserved.

## How do I use the IBM Cloud Console to give other users permission to create or manage a CDN?
{: faq}

The account's Master User can provide other users with permission to create and manage a CDN.

From the IBM Cloud console main page, follow these steps to edit permissions:
 * Select the **Manage** tab
 * Select **Access (IAM)**
 * Click on the **Users** tab from left pane
 * Click on the desired **User**
 * Then select the **Classic infrastructure** tab
 * Then under **Permissions** tab, expand the **Services** category
 * Select **Manage CDN Account**
 * Click **Save** button

From the legacy console main page, follow these steps to edit permissions:
 * Select the **Account** tab
 * Select **Users -> User List**
 * Click on the desired **Username**
 * Then select the **Portal Permissions** tab
 * Select the **Services** tab
 * Select **Manage CDN Account**
 * Click the **Edit Portal Permissions** button

## Why is the Create button not shown or disabled on https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai page?
{: faq}

If you are the account's Master User, then you must upgrade the account in order for the Create button to appear or be enabled on this page. From the IBM Cloud console page, follow these steps as the account's Master User:
 * Open the left navigation pane by clicking the `triple bar` icon in top left corner of web page
 * Select **Classic Infrastructure**
 * Click on **Upgrade Account** button and follow the instructions

If you are one of the account's secondary Users, then the account's Master User must give you the `Add/Upgrade Services` permission in order for the Create button to appear or be enabled on this page. From the IBM Cloud console page, the account's Master User should follow these steps to edit your permissions:
 * Select the **Manage** tab
 * Select **Access (IAM)**
 * Click on the **Users** tab from left pane
 * Click on the desired **User**
 * Then select the **Classic infrastructure** tab
 * Then under **Permissions** tab, expand the **Account** category
 * Select **Add/Upgrade Services**
 * Click **Save** button

## Why am I unable to reach my webpage through my CDN after configuring Hotlink Protection with `protectionType` `ALLOW`?
{: faq}

Let's consider an example in which your website's domain for end-users is configured to be your CDN's domain/host name: `cdn.example.com`. When someone attempts to reach a webpage by navigating directly from the browser's navigation bar, the browser typically does not send Referer headers in its HTTP request. For example, when you directly navigate in this way to `https://cdn.example.com/`, your CDN considers that the request contains a non-match against the specified `refererValues`. When the CDN evaluates the appropriate effect or response through your Hotlink Protection, it determines that a non-match occurred. Therefore, your CDN will deny access, rather than 'ALLOW'.

## Can I use private endpoint of object storage in CDN settings?
{: faq}

No, CDN can only connect to Object Storage on Public endpoints.

## Can I use Brotli feature in CDN service?
{: faq}

No, Brotli feature is not supported with our CDN service with Akamai.

## How do I create a CDN endpoint without using the domain?
{: faq}

You can create a CDN endpoint without using the Domain, but ONLY for a CDN of type **Wildcard HTTPS**. While creating a CDN of type **Wildcard HTTPS**, your **CNAME** acts as the CDN endpoint, and the **CNAME** is used to serve the traffic.
