---

copyright:
  years: 2018
lastupdated: "2018-11-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{: faq: data-hd-content-type=‘faq’}

# FAQ for HTTPS 

## For my CDN, what is the difference between HTTPS with Wildcard Certificate and HTTPS with SAN certificate?
{: faq}

With Wildcard certificate, all customers use the same certificate deployed on the vendor's CDN networks. The CNAME, including the IBM suffix `.cdnedge.bluemix.net`, must be used to access the service. For example, `https://www.example-cname.cdnedge.bluemix.net`

In the case of SAN certificate, multiple customer domains share a single SAN certificate by adding their domain names into the SAN entries. The service can then be accesses using the hostname, for instance `https://www.example.com`

## How is Domain Validation with redirect accomplished?
{: faq}

It depends on your server. The procedure for completing Domain Validation for Apache and Nginx servers can be found on the [Completing Domain Control Validation for HTTPS](how-to-https.html#redirect) page.

## How long does Domain Validation take?
{: faq}

Domain Validation normally takes from 2 - 4 hours, but varies depending on the method chosen for validation. DV with CNAME validation is the fastest, typically taking under an hour. DV using the Standard and Redirect methods typically take ~4 hours, after the challenge has been addressed.

## How long does it take to create and enable HTTPS for my CDN take with a DV SAN certificate?
{: faq}

A normal request to enable HTTPS takes an average of 3 - 9 hours, from the initial request to running.

## How long does it take to delete a CDN with a DV SAN certificate?
{: faq}

Deleting your CDN requires that your domain be removed from the certificate on all of the edge servers. This process can take up to 8 hours to complete.

## Is there any additional cost associated with using a DV SAN certificate for my CDN?
{: faq}

No. DV SAN certificate configurations are provided to you at no additional charge compared with HTTP or HTTPS with Wildcard Certificate.

## Can my CDN created using HTTPS with Wildcard be updated to use a DV SAN certificate?
{: faq}

No, a wildcard mapping cannot be changed to SAN certificate at this time.

## What is a Certificate Authority?
{: faq}

A Certificate Authority (CA) is an entity that issues Digital Certificates.

## Which CA does IBM Cloud CDN service for issuing a DV SAN certificate?
{: faq}

IBM Cloud CDN service uses LetsEncrypt Certficate Authority.

## What SSL certificates are supported for IBM Cloud CDN?
{: faq}

The SSL certificates that are supported are Wildcard certificate and Domain Validation (DV) Subject Alternate Name (SAN) certificate. The SAN certificate is shared across multiple customers. IBM Cloud CDN does not support uploading custom certificates.

## I received an email asking me to address a Domain Validation challenge related to my CDN. What do I do now?
{: faq}

Domain Validation can be addressed in one of three ways: CNAME, Standard or Direct.

For details on how to address any of these, please refer to the [Completing Domain Control Validation for HTTPS](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation) document.

## What will happen if I don't address the challenge for domain validation of my CDN?
{: faq}

If the mapping's state is in DOMAIN_VALIDATION_PENDING state for more than 48 hours, the mapping creation will be cancelled, and the mapping's state will be CREATE_ERROR. And in this state, you can choose to Retry creation or delete the mapping.

## Does a Wildcard certificate need to validate a domain for my CDN?
{: faq}

No, but you can only use the CNAME to retrieve content from your origin. `https://www.example-cname.cdnedge.bluemix.net`

## If I use the SAN certificate type for my CDN, can I still use the CNAME to access my service?
{: faq}

No. For the SAN certificate you can only use the custom domain to access the content from the origin. `https://www.example.com`

## Are all of my IBM Cloud CDN domains added into one certificate?
{: faq}

Not necessarily. Certificate selection is handled by Akamai, in order to ensure the certificates are in the most efficient state. Domains are added into different certificates in a well-proportioned manner, so we cannot guarantee that all of your domains will be on the same certificate.

## Why do I see "wildcard" when I perform a `dig` or when I try to access content when my CDN is in `Requesting Certificate`, `Domain Validation pending` or `Deploying Certificate` status?
{: faq}

During the DV SAN certificate requesting process, the DNS record chain for your CDN is chained to a wildcard certificate, temporarily. Until the process is complete, content temporarily is served through this wildcard certificate. Once the requesting process is complete, the DNS record chain is updated to chain to your CDN's DV SAN certificate.
