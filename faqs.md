---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-28"

keywords: faqs, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for CDN
{: #faqs}

Have a question about CDN? Review frequently asked questions, which provide answers to provisioning concerns, application access, and other common inquiries.
{: shortdesc}

## What is a Content Delivery Network (CDN)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}
{: support}

A Content Delivery Network (CDN) is a collection of Edge servers that are distributed through various parts of the country or the world. Their web content is served from an Edge server, which is located in the geographic area closest to the customer who requests the content. This technique lets the users receive the content with less delay than we might achieve by delivering the content from one centralized location. It delivers a better overall experience for your customers.

## How does a Content Delivery Network (CDN) work?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}
{: support}

A CDN achieves its purpose by caching web content on Edge servers around the world. When a user requests web content, the content request is routed to the Edge server that is geographically closest to that user. By reducing the distance that the content must travel, the CDN offers optimized throughput, minimized latency, and increased performance.

## How is my {{site.data.keyword.cloud}} Content Delivery Network service account created?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}
{: support}

Your account is created during the CDN ordering process. If you are creating a CDN from the legacy portal, when you click the **Order CDN** button, under the **Network > CDN page**, your account is created. If you are creating a CDN from the {{site.data.keyword.cloud_notm}} portal, when you click the **Create** button, under the **Catalog > Network > Content Delivery Network** page, your account is created.

## What do I do when my CDN is in CNAME configuration status?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}
{: support}

For HTTP and SAN certificate-based HTTPS CDN, update your DNS record so that your website points to the `CNAME` associated with your new CDN mapping.

For wildcard, certificate-based HTTPS CDN, this DNS update is not needed because you access the website through `https://<CNAME>`. You can refresh your CDN status by clicking **Get status** from the menu of your CDN instance.  

It can take up to 15 - 30 minutes for the update to take effect. Check with your DNS provider to obtain an accurate time estimate.
{: note}

## How do I add a CNAME record for my CDN domain in DNS?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}
{: support}

In your DNS configuration page for your CDN domain, you can create a CNAME record with the CDN domain name as the Host, and the IBM CNAME you used to configure the CDN as the CNAME. The IBM CNAME ends with `cdn.appdomain.cloud.`.

A typical CNAME record looks similar to the following on the DNS configuration page:

| **Resource Type** | **Host** | **Points to (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdn.appdomain.cloud | 15 minutes |
{: caption="CNAME record example" caption-side="bottom"}

## Where do I find cost estimates for CDN?
{: #where-do-i-find-cost-estimates-for-cdn}
{: faq}
{: support}

You can estimate the cost of a service using the cost estimator on the provisioning pages for CDN offerings. For example, log in to the [IBM Cloud Content Delivery Network](/catalog/infrastructure/cdn-powered-by-akamai) console and click **Create**. As you complete the ordering form, cost estimates appear in the Summary side panel.

## When am I billed for my CDN?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}
{: support}

{{site.data.keyword.cloud_notm}} Content Delivery Network billing occurs according to the billing period established in your {{site.data.keyword.cloud_notm}} account.

## If I select `Delete` from the Overflow ![Overflow menu](images/overflow.png) menu, does that delete my account?
{: #if-i-select-delete-does-that-delete-my-account}
{: faq}
{: support}

No, if you select 'Delete' from the Overflow ![Overflow menu](images/overflow.png) menu, only that CDN is deleted. Your account still exists, and you can create additional CDNs.

## Does content caching use push or pull?
{: #does-content-caching-use-push-or-pull}
{: faq}

Content caching is done using an _origin pull_ model. Origin Pull is a method by which data is "pulled" by the Edge server from the origin server, as opposed to manually uploading the content onto the Edge server.

## Is there a recommended browser to use for CDN service configuration?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Yes, Firefox and Chrome are the recommended browsers. It is recommended that you use the latest versions with your {{site.data.keyword.cloud_notm}} Content Delivery Network.

## What is the purpose of providing a path when creating my CDN?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

If you provide a path while creating your CDN, it allows you to isolate the files that can be served through CDN from a particular origin server.

## My CDN is in an Error State. What do I do now?
{: #my-cdn-is-in-an-error-state}
{: faq}

Refer to the [Troubleshooting](/docs/CDN?topic=CDN-troubleshoot-cdn-working) or [Getting help and support](/docs/CDN?topic=CDN-gettinghelp#gettinghelp), or open a case in the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/).

## Where do I find the CNAME for my CDN if I didn't provide one?
{: #where-do-i-find-the-cname-for-my-cdn}
{: faq}

Click your CDN to access the **Overview** page in the portal. In the upper right corner, you can see a **Details** section with the `CName` information.

## My single file purge request for a given file path is in progress. Can I submit a new request for the same file path?
{: #my-single-file-purge-request-for-a-given-file-path}
{: faq}

No. There can be only one active purge request for a given file path at a time.

## Is Internet Protocol version 6 (IPv6) supported by the {{site.data.keyword.cloud_notm}} Content Delivery Network service? How does it work?
{: #is-ip-version-6-supported}
{: faq}

IPv6 (or dual stack support) is supported by Akamai's Edge servers. It is designed to help customers with an IPv4-only origin to accept connections from IPv6 clients, convert from IPv6 to IPv4 at the Edge, and go forward to the origin with IPv4.

Creating an {{site.data.keyword.cloud_notm}} CDN using an IPv6 address as the origin server address is not supported.
{: note}

## Are there any restrictions on what HTTP and HTTPS port numbers are allowed for Akamai?
{: #are-there-any-restrictions-on-what-port-numbers-are-allowed}
{: faq}

Yes. For the Akamai vendor, only the following port numbers are allowed:
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410, and 45002.

## What URL should I use for access to data under the CDN or origin path?
{: #what-url-should-be-used-for-access-to-data}
{: faq}

The path for a CDN mapping, or for the origin, is treated as a directory. Therefore, users trying to access the origin path should access it as a directory (with a slash). For example, if CDN `www.example.com` is created using the path that includes the `/images` directory, the URL to reach it should be `www.example.com/images/`

Omitting the slash, for example, using `www.example.com/images` results in a **Page Not Found** error.

## How do I set up my Content Delivery Network for {{site.data.keyword.cloud_notm}} Object Storage (COS)?
{: #how-do-i-set-up-my-cdn-for-cos}
{: faq}

See the [Accelerate delivery of static files using a CDN](/docs/solution-tutorials?topic=solution-tutorials-static-files-cdn) tutorial for information about creating a Content Delivery Network for {{site.data.keyword.cloud_notm}} Object Storage.

## I received notification that my origin certificate is expiring. What do I do now?
{: #notification-that-origin-cert-is-expiring}
{: faq}

Log in to the Akamai Community and follow the steps outlined in [this article](https://community.akamai.com/docs/DOC-7708){: external}.

## What security is included with the IBM CDN solution with Akamai?
{: #what-security-is-included-with-cdn-with-akamai}
{: faq}

Using the distributed Akamai platform, you get unparalleled scalability and resiliency with thousands of servers in over 50 countries. The Akamai Intelligent Platform stands between your infrastructure and your users, and it acts as first level of defense for sudden surges in traffic. Akamai Intelligent Platform also is a reverse proxy that listens and responds to requests on ports 80 and 443 only, which means that traffic on other ports is dropped at the Edge without being forwarded to your infrastructure.

## Are cookies from the origin server preserved by the Akamai CDN?
{: #are-cookies-preserved-by-the-akamai-cdn}
{: faq}

For non-cacheable content, or any content that is not cached, cookies are preserved from the origin. For content that is cached by Edge servers, cookies are not preserved.

## How do I use the {{site.data.keyword.cloud_notm}} console to give other users permission to create or manage a CDN?
{: #how-do-i-use-cloud-to-give-users-permission-to-create-or-manage-cdn}
{: faq}

The account's Master user can provide other users with permission to create and manage a CDN.

From the {{site.data.keyword.cloud_notm}} console main page, follow these steps to edit permissions:

* Select the **Manage** tab.
* Select **Access (IAM)**.
* Click the **Users** tab in the navigation pane.
* Click the wanted **User**.
* Select the **Classic infrastructure** tab.
* From the **Permissions** tab, expand the **Services** category.
* Select **Manage CDN Account**.
* Click the **Save** button.

From the legacy console main page, follow these steps to edit permissions:

* Select the **Account** tab.
* Select **Users > User List**.
* Click the wanted **Username**.
* Select the **Portal Permissions** tab.
* Select the **Services** tab.
* Select **Manage CDN Account**.
* Click the **Edit Portal Permissions** button.

## Why is the Create button not shown or disabled on the [Content Delivery Network](https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai) page?
{: #why-is-create-button-not-showing}
{: faq}

If you are the account's Master user, you must upgrade the account for the **Create** button to appear or be enabled on this page. From the {{site.data.keyword.cloud_notm}} console page, follow these steps as the account's Master user:

* Open the navigation pane by clicking the `triple bar` icon in the upper left of the web page.
* Select **Classic Infrastructure**.
* Click the **Upgrade Account** button and follow the instructions.

If you are one of the account's secondary users, the account's Master user must give you the `Add/Upgrade Services` permission for the **Create** button to appear or be enabled on this page. From the {{site.data.keyword.cloud_notm}} console page, the account's Master user can follow these steps to edit your permissions:

* Select the **Manage** tab.
* Select **Access (IAM)**.
* Click the **Users** tab from navigation pane.
* Click the wanted **User**.
* Select the **Classic infrastructure** tab.
* From the **Permissions** tab, expand the **Account** category.
* Select **Add/Upgrade Services**.
* Click the **Save** button.

## Why am I unable to reach my web page through my CDN after configuring Hotlink Protection with `protectionType` `ALLOW`?
{: #why-am-i-unable-to-reach-webpage-through-my-cdn}
{: faq}

Let's consider an example in which your website's domain for users is configured to be your CDN's domain/hostname: `cdn.example.com`. When someone attempts to reach a web page by navigating directly from the browser's navigation bar, the browser typically does not send Referer headers in its HTTP request. For example, when you directly navigate in this way to `https://cdn.example.com/`, your CDN considers that the request contains a non-match against the specified `refererValues`. When the CDN evaluates the appropriate effect or response through your Hotlink Protection, it determines that a non-match occurred. Therefore, your CDN denies access, rather than 'ALLOW'.

## Can I use private endpoint of object storage in CDN settings?
{: #can-i-use-private-endpoint-of-object-storage-in-cdn-setting}
{: faq}

No, CDN can only connect to object storage on public endpoints.

## Can I use the Brotli feature in the CDN service?
{: #can-i-use-brotli}
{: faq}

No, the Brotli feature is not supported by our CDN service with Akamai.

## How do I create a CDN endpoint without using the domain?
{: #how-do-i-create-a-cdn-endpoint-without-using-the-domain}
{: faq}

You can create a CDN endpoint without using the domain, but ONLY for a CDN of type **Wildcard HTTPS**. While creating a CDN of type **Wildcard HTTPS**, your **CNAME** acts as the CDN endpoint, and the **CNAME** is used to serve the traffic.

## Is HTTP/2 supported by the {{site.data.keyword.cloud_notm}} Content Delivery Network service?
{: #is-http2-supported-by-cdn}
{: faq}

Yes, HTTP/2 is supported by Akamai's Edge servers.

## Is WebSocket supported by the {{site.data.keyword.cloud_notm}} Content Delivery Network service?
{: #is-websocket-supported}
{: faq}

No, WebSocket is not supported by Akamai's Edge servers.

## With multiple file purges, what's the difference between a favorite group and an unfavorite group?
{: #what-different-two-type-favorite-group}
{: faq}

A favorite is a permanent group, which means that it will never be deleted unless you change it to an unfavorite group. An unfavorite group is a temporary group. This type of group is automatically deleted after 15 days of inactivity.

Favorite groups names must be unique. Unfavorite groups do not have this limitation.
{: note}

## In what status is a CDN allowed to perform multiple file purges?
{: #what-status-is-cdn-allowed-for-multiple-file-purge}
{: faq}

Multiple file purges are allowed in the following states:

* Running
* Running - HTTP only
* CNAME configuration required
* Stopped

## Is IBM CDN compliant with Payment Card Industry Data Security Standard (PCI DSS)?
{: #is-cdn-pci-dss-compliant}
{: faq}

Yes, IBM CDN is PCI DSS 3.2.1 compliant through our partner Akamai's certification. For more information, see the Akamai [Attestation of Compliance](https://www.akamai.com/uk/en/multimedia/documents/infosec/pci-dss-3.2-attestation-of-compliance.pdf){: external}.

## How to get the client IP address?
{: #get-client-ip}
{: faq}

Akamai Edge servers add the `True-Client-IP` and `X-Forwarded-For` headers in the requests to the origin. Then, in your backend origin server, you can get the client IP address from the value of the `True-Client-IP`, or extract the first IP in the chain of `X-Forwarded-For`.
