---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# FAQs

## What is Content Delivery Network (CDN)?

The Content Delivery Network (CDN) is a collection of edge servers that are distributed through various parts of the country or the world. Their web content is served from an edge server, which is located in the geographic area closest to the customer who requests the content. This technique lets the users receive the content with less delay than we could achieve by delivering the content from one centralized location. It delivers a better overall experience for your customers.

## How does CDN work?

CDN achieves its purpose by caching web content on edge servers around the world. When a user requests web content, the content request is routed to the edge server that is geographically closest to that user. By reducing the distance the content must travel, the CDN offers optimized throughput, minimized latency, and increased performance. 

## How is my CDN account created?

Your account is created during the CDN order process, when you click on **Select** after going through the "Vendor selection" menu.

## What do I do when my CDN is in CNAME_CONFIGURATION Status?

Update your DNS record so that your website points to the `CNAME` associated with your new CDN mapping. 
**Note**: It may take up to 15-30 minutes for the update to become active. Check with your DNS provider to obtain an accurate time estimate.

## What will I be billed for?

You are billed for bandwidth used per CDN. If no bandwidth is used by your CDN, no charges are incurred. Bandwidth prices vary, depending on the regional location of the edge server.

## When will I be billed?

CDN billing occurs according to the billing period established in your {{site.data.keyword.BluSoftlayer_notm}} Account.

## How do I view the metrics and usage?

You can view metrics and usage on the **Overview** page, which can be reached by selecting your CDN from the **Content Delivery Networks** page. **Note**: After you create your CDN, it may take up to 48 hours for metrics to appear.

## Is there a minimum number of days for which I can view metrics? Is there a maximum?

Yes. Metrics can be gathered for a minimum of 7 days. Metrics can be viewed for a maximum of 90 days. For those using the API, it is recommended to use 89 days as the maximum, to account for any differences in time zones.

## How does the HTTPS wildcard certificate work?

The wildcard certificate is the most economical way to deliver web content to your end-users securely. To use the wildcard certificate, the customer must use the CNAME hostname as the service's entry point (for example, _https://example.cdnedge.bluemix.net_). After the CDN mapping is enabled for HTTPS using the wildcard certificate, the edge server contacts the Origin Server through HTTPS. If the Origin Server is specified as a hostname, the edge server uses the origin domain as the Server Name Indication (SNI) header for the Transport Layer Security (TLS) neogotiation with the Origin Server, by default. However, if the origin domain is specified as an IP address, the CDN's hostname will be used as the SNI header. The origin certificate must be signed by a recognized Certificate Authority (CA). 

When HTTPS Port is selected, the user can only use the CNAME to access the service.

## If I select `delete` from the CDN's overflow menu, does that delete my account?

No; it will only delete that CDN. Your account still exists, and you can create additional CDNs.

## Does caching use push or pull?

Content Caching is done using an _origin pull_ model. Origin Pull is a method by which data is "pulled" by the edge server from the Origin Server, as opposed to manually uploading the content onto the edge server. 

## Is there a maximum value for Time To Live? A minimum?

The maximum value for Time To Live is 2,147,483,647 seconds, which equates to roughly 68 years! The minimum value is 30 seconds.

## What is the Serve Stale Content option?

If the caching time expires for a content, the edge server will try to fetch the content from the Origin Server. If for some reason the Origin server is down or cannot be contacted, the edge server can serve the stale content, if that option is set, which is what most customers prefer. Currently our CDNs can only be configured with this option set to **Yes** as default. Changing the option to **No** will not have any impact: the **Serve Stale Conent** option will continue to be set to **Yes**.

## Is there a limit on the number of Origin and TTL entries?

Yes, the combined limit is 75 entries per CDN.

## Is there a limit on the number of CDNs I can have?

Yes, you can have a limit of 10 CDNs per account.

## Is there a recommended browser to use for CDN service configuration?

Yes, Firefox and Chrome are the recommended browsers, at their latest versions.

## What is the purpose of providing a Path when creating my CDN?

If you provide a path while creating a CDN, it allows you to isolate the files that can be served through CDN from a particular Origin Server.

## How do I know my CDN is working?
Run the following 'curl' command by replacing "origin.cdntesting.net/assets/ibm_3d.gif" with respective file path on your origin: 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
If the output of the command matches the following format, the CDN is working as expected: 
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## My CDN is in an Error State. What do I do now?
Please refer to the ['Getting Help and Support'](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp) page, or open a ticket in the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/).

## Where do I find my CName if I didn't provide one? 
Click on the CDN to get to the **Overview** Page. On the right corner you can see a **Details** section with the `CName` information.

## Is there a limit on the number of Purge requests that can be active at once?
Yes. There can only be 5 active purge requests at a time.

## My Purge Request for a given file path is in Progress. Can I submit a new request for the same file path?
No. There can only be one active Purge request for a given file path at a time.

## Is Internet Protocol version 6 (IPv6) supported with IBM CDN service? How does it work?
IPv6 (or dual stack support) is supported by Akamai's Edge servers. It is designed to help customers with IPv4 only origin to accept connections from IPv6 clients, convert from IPv6 to IPv4 at the Edge and go forward to the origin with IPv4.

**Note:** Creating a CDN using an IPv6 address as the Origin Server Address is not supported.

## For creating a CDN, what are the rules to follow for specifying the Hostname input string?

The `Hostname` input string should be alphanumeric, ending with a valid top-level domain name, and less than 512 characters in length.

## For creating a CDN, what are the rules to follow for making the CNAME input string?
The `CNAME` input string should be alphanumeric, less than 64 characters, and unique. (It cannot be in use by any other IBM CDN.)

## For creating a CDN, what are the rules to follow for specifying the Path string for the Origin?
For **Create CDN**, the path string is optional. If provided, the path must begin with a '/' and it must be less than 1000 characters in length

## For the **Add Origin** command, are there any rules to follow for the Path string?
For **Add Origin**, the path is mandatory. Also if the CDN was created with a path, the path for **Add Origin** must start with the CDN path as the prefix. For example, if the CDN path was specified as `/storage`, to invoke **Add Origin** with a path called `/examplePath`, the path provided would be `/storage/examplePath`'.

## In what format should I provide the file extensions for creating an Object Storage Origin Type?

File extensions should be comma separated. For example, the list "txt, jpg, xml" is a valid list. Any particular extension cannot be longer than 10 characters.

## Are there any restrictions on what HTTP and HTTPS port numbers are allowed for Akamai?
For the Akamai vendor, only certain port numbers are allowed: 
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410, and 45002


## What URL should be used for access to data under the CDN or Origin Path? 
The path for a CDN mapping or for the origin is treated as a directory. Therefore, users trying to access the origin path should access it as a directory (with a slash). For example, if CDN `www.example.com` is created using the path that includes the `/images` directory, the URL to reach it should be `www.example.com/images/`

Omitting the slash, for example, using `www.example.com/images` will result in a **Page Not Found** error.

## What is the hit ratio? Why is the hit ratio non-zero when total hits are zero?
Hit ratio represents the percentage of times the content was delivered from the Edge Server Cache, rather than being delivered from the Origin Server. It is calculated as follows:

```
((Edge hits - Ingress hits)/Edge hits) * 100

where: 
Edge hits is defined as "All hits to the edge servers from the end-users"
Ingress hits is defined as "Origin or Ingress hits are for traffic from your origin to Akamai edge servers"
```
 
Because Hit Ratio is calculated at the Account level, not per CDN, the Hit Ratio will be the same for all the CDNs in your account. This fact also explains why the Hit Ratio may be non-zero when the number of Edge hits for a particular CDN is zero.

## For HTTPS, why can't I connect through a curl command or browser using the Hostname?

Currently HTTPS is supported only through a Wildcard certificate. As a result of this limitation, the connection must be made using CNAME; trying to connect using the Hostname will result in failure.
