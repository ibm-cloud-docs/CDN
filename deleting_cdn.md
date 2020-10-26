---

copyright:
  years: 2020
lastupdated: "2020-10-26"

keywords: delete CDN

subcollection: CDN

---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Deleting a CDN
{: #deleting-a-cdn}

To delete a CDN, follow these steps:
{:shortdesc}

1. Click **Delete** from the Overflow ![Overflow menu](images/overflow.png) menu. A confirmation window appears.
2. Click **Delete** to proceed.
3. When the delete process is complete, click **Get status** from the Overflow menu to remove the row from the CDN list. If the delete process has not completed, this action has no effect.

## FAQs for deleting CDN
{: #faq-for-delete-cdn}

### Will deleting a CDN delete my account?
{: #delete-cdn-account}
{: faq}

No. Selecting **Delete** deletes only the CDN; it does not delete your account.

### Why is the DV SAN certificate CDN still there after I deleted it?
{: #delete-cdn-account}
{: faq}

If your CDN is configured with HTTPS with DV SAN certificate, it can take up to five hours to complete the deletion process.

### Why did the deletion of the DV SAN certificate domain fail? It shows that the domain is still live on the network.
{: #delete-cdn-failed}
{: faq}

If your CDN is configured with DV SAN certificate HTTPS, you must remove the DNS record that points your domain to [IBM CNAME](/docs/CDN?topic=CDN-getting-to-running-status#ibm-cname) or [Akamai CNAME](/docs/CDN?topic=CDN-getting-to-running-status#akamai-cname) before deleting your CDN. Otherwise, the deletion fails with the error message 'Delete CDN failed: the xxxxxx is still live on network, please remove the DNS record pointing to Akamai.'
