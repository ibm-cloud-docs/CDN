---

copyright:
  years: 2018, 2019
lastupdated: "2019-08-09"


keywords: wildcard certificate, https, san certificate

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

# About HTTPS
{: #about-https}

{{site.data.keyword.cloud}} offers two ways to secure your CDN with HTTPS - Wildcard certificate and Domain Validation (DV) SAN certificate. Both HTTPS options can be configured by selecting **HTTPS Port** when configuring your CDN.
{: shortdesc}

The default HTTPS Port is 443, or you can choose a different port number to route your HTTPS traffic through. A list of allowed port numbers can be found in the [FAQs](/docs/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-port-numbers-are-allowed).

To decide between using **Wildcard Certificate** and **SAN Certificate** for HTTPS, answer this question: Do you want to serve HTTPS traffic from the CDN CNAME or the CDN domain name? If you would like to serve HTTPS traffic from the CNAME, select **Wildcard Certificate**. If you want to serve HTTPS traffic from the CDN domain name, select **SAN certificate**.

## Wildcard certificate support
{: #wildcard-certificate-support}

The Wildcard certificate is the simplest way to deliver web content to your users securely. The full CDN CNAME, including the Wildcard certificate suffix, must be used as the service entry point (for example, `https://example.cdn.appdomain.cloud`) to use the Wildcard certificate.

IBM Cloud CDN uses the Wildcard certificate `*.cdn.appdomain.cloud.`. The CNAME, regardless of whether it was created for you or provided by you, and ending in suffix `*.cdn.appdomain.cloud.` is added to the wildcard certificate maintained on the CDN Edge server. And thus CNAME becomes the only way for users to use HTTPS for your CDN.

![Diagram for HTTP and Wildcard](images/state-diagram-wildcard.png){: caption="Figure 1: HTTP and Wildcard" caption-side="top"}

## Subject Alternate Name (SAN) certificate support
{: #san-certificate-suport}

A Subject Alternative Name (SAN) certificate is a digital SSL certificate that allows multiple domains, or hostnames, to be protected by a single certificate.

With SAN certificate for HTTPS, your primary CDN hostname is added to a certificate that was issued by a certificate authority. This allows your users to access your service securely by using the hostname rather than the CNAME; for example, `https://www.example.com`.

When the CDN order is placed that uses an HTTPS SAN certificate, it goes through the process of requesting a certificate and creating a Domain Control Validation (DCV). DCV is the process a certificate authority uses to establish that you are authorized to access and control the domain. Your action is required to complete this step. After control is established, the certificate is deployed to the CDN Edge servers around the world. After the certificate is successfully deployed, the renewal of the certificate is handled automatically. For more information, see [HTTPS protocol support](/docs/CDN?topic=CDN-about-content-delivery-networks-cdn-#https-protocol-support). Domain Control Validation methods are explained in more detail in [Initial steps to Domain Control Validation](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

After the CDN reaches RUNNING status, you must keep the CDN hostname CNAME record in your DNS. If the CNAME record is removed, the CDN hostname can be removed from the SAN certificate within three days. If that happens, HTTPS traffic is no longer served with that CDN hostname.
{: note}

![Diagram for HTTPS with SAN Certificate](images/state-diagram-san.png){: caption="Figure 2: HTTPS with SAN certificate" caption-side="top"}
