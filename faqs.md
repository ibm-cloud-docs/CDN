---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# FAQs

## What is a Content Delivery Network (CDN)?

A Content Delivery Network (CDN) is a collection of edge servers that are distributed through various parts of the country or the world. Their web content is served from an edge server, which is located in the geographic area closest to the customer who requests the content. This technique lets the users receive the content with less delay than we could achieve by delivering the content from one centralized location. It delivers a better overall experience for your customers.

## How does a Content Delivery Network (CDN) work?

A CDN achieves its purpose by caching web content on edge servers around the world. When a user requests web content, the content request is routed to the edge server that is geographically closest to that user. By reducing the distance the content must travel, the CDN offers optimized throughput, minimized latency, and increased performance.

## How is my IBM Cloud Content Delivery Network service account created?

Your account is created during the CDN order process. If you are creating a CDN from the Legacy portal, when you click the **Order CDN** button, under the **Network -> CDN page**, your account is created. If you are creating a CDN from the IBM cloud portal, when you click the **Create** button, under the **Catalog -> Network -> Content Delivery Network** page, your account is created.

## What do I do when my CDN is in CNAME Configuration Status?

For HTTP and SAN Certificate based HTTPS CDN, update your DNS record so that your website points to the `CNAME` associated with your new CDN mapping. For Wildcard Certificate based HTTPS CDN, this DNS update is **NOT** needed.

**Note**: It may take up to 15-30 minutes for the update to become active. Check with your DNS provider to obtain an accurate time estimate.

A typical CNAME record would look like the following on the DNS configuration page:

| **Resource Type** | **Host** | **Points to (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutes |


## What will I be billed for?

You are only billed for bandwidth used per IBM Cloud Content Delivery Network instance. If no bandwidth is used by your CDN, no charges are incurred. Bandwidth prices vary, depending on the regional location of the edge server. You can see bandwidth pricing by geographic region in the [Getting Started](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-) section for this service.

## When will I be billed?

IBM Cloud Content Delivery Network billing occurs according to the billing period established in your {{site.data.keyword.BluSoftlayer_notm}} Account.

## If I select `delete` from the CDN's overflow menu, does that delete my account?

No; it will only delete that CDN. Your account still exists, and you can create additional CDNs.

## Does caching use push or pull?

Content Caching is done using an _origin pull_ model. Origin Pull is a method by which data is "pulled" by the edge server from the Origin Server, as opposed to manually uploading the content onto the edge server.

## Is there a recommended browser to use for CDN service configuration?

Yes, Firefox and Chrome are the recommended browsers. It is recommended that you use their latest versions with your IBM Cloud Content Delivery Network.

## What is the purpose of providing a path when creating my CDN?

If you provide a path while creating your CDN, it allows you to isolate the files that can be served through CDN from a particular Origin Server.

## My CDN is in an Error State. What do I do now?

Please refer to the [Troubleshooting](troubleshooting.html#troubleshooting) or [Getting Help and Support](getting-help.html#gettinghelp) pages, or open a ticket in the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).

## Where do I find my CNAME if I didn't provide one?

Click on your CDN to access the **Overview** Page in the Portal. On the right corner you can see a **Details** section with the `CName` information.

## My Purge Request for a given file path is in Progress. Can I submit a new request for the same file path?

No. There can only be one active Purge request for a given file path at a time.

## Is Internet Protocol version 6 (IPv6) supported with the IBM Cloud Content Delivery Network service? How does it work?

IPv6 (or dual stack support) is supported by Akamai's Edge servers. It is designed to help customers with IPv4 only origin to accept connections from IPv6 clients, convert from IPv6 to IPv4 at the Edge and go forward to the origin with IPv4.

**NOTE:** Creating an IBM Cloud CDN using an IPv6 address as the Origin Server Address is not supported.

## Are there any restrictions on what HTTP and HTTPS port numbers are allowed for Akamai?

Yes. For the Akamai vendor, only the following port numbers are allowed:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410, and 45002.

## What URL should be used for access to data under the CDN or Origin Path?
The path for a CDN mapping or for the origin is treated as a directory. Therefore, users trying to access the origin path should access it as a directory (with a slash). For example, if CDN `www.example.com` is created using the path that includes the `/images` directory, the URL to reach it should be `www.example.com/images/`

Omitting the slash, for example, using `www.example.com/images` will result in a **Page Not Found** error.

## How do I set up my Content Delivery Network for IBM Cloud Object Storage (COS)?

[Here's a tutorial](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) on creating a Content Delivery Network for IBM Cloud Object Storage.

## I received notification that my Origin Certificate is expiring. What do I do now?

Follow the steps outlined in [this article](https://community.akamai.com/docs/DOC-7708) from Akamai.

## What security is included with the IBM CDN solution with Akamai?

Using the distributed Akamai platform, you get unparalleled scale and resiliency with more than 240,000 servers in over 130 countries. The Akamai Platform stands between your infrastructure and your end users, and it acts as first level of defense for sudden surges in traffic. Akamai Intelligent Platform also is a reverse proxy that listens and responds to requests on ports 80 and 443 only, which means that traffic on other ports is dropped at the edge without being forwarded to your infrastructure.

## Are cookies from the origin server preserved by the Akamai CDN? 

For non-cacheable content, or any content that is not cached, cookies are preserved from the origin. For content that is cached by Edge servers, cookies are not preserved.

## How do I use the IBM Cloud Console to give other users permission to create or manage a CDN?

In the IBM Cloud Console, the account's Master User can provide other users with permission to create and manage a CDN. From the IBM Cloud Console main page, follow these steps to edit permissions:
 * Select the **Account** tab
 * Select **Users -> User List**
 * Click on the desired **Username**
 * Then select the **Portal Permissions** tab
 * Select the **Services** tab
 * Select **Manage CDN Account**
 * Click the **Edit Portal Permissions** button
 * Set the permissions that are needed.

