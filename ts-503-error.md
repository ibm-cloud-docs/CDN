---

copyright:
  years: 2018, 2020
lastupdated: "2020-01-08"

keywords: troubleshooting

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# I received a 503 error message. Why?
{: #troubleshoot-cdn-503-error}
{: troubleshoot}
{: support}

If you received a 503 error message, you must ensure that your origin server's SSL certificates meet certain criteria.
{: shortdesc}

The most common reason observed for the 503 error is due to an issue with a certificate in the SSL certificate chain.  
{: tsCauses}

This is the error that you might see: `503 Service Unavailable`.
{: tsSymptoms}

With the 503 error, you might also see a message similar to the following: `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff` (the actual reference number might vary). In this case, the reference number in the error string translates to an SSL handshake failure.

To correct the issue, ensure that your origin server's SSL certificates meets the following criteria:
{: tsResolve}

* The certificate must be issued by a certificate authority trusted by Akamai. You can view the list of Akamai trusted certificates at [this link](https://community.akamai.com/customers/s/article/SSL-TLS-certificate-chains-for-Akamai-managed-certificates?language=en_US){: external}.
* It must have a complete certificate chain (leaf, intermediate, root).
* It must match the *Host header that is configured* on the CDN
* It must not be self-signed
* It must not be expired

If you have verified your origin's certificate chain using the previous criteria and you are still encountering the same error, see [Getting help and support](/docs/CDN?topic=CDN-gettinghelp). Make note of the Reference error string and include it in any communication with us.
