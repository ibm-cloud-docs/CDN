---

copyright:
  years: 2018, 2020
lastupdated: "2020-12-23"

keywords: troubleshooting

subcollection: CDN

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why doesn't my hostname load on the browser when {{site.data.keyword.cloud_notm}} Object Storage (COS) is the origin?
{: #troubleshoot-cdn-hostname-does-not-load}
{: troubleshoot}
{: support}

When your {{site.data.keyword.cloud}} CDN is configured to use COS as the object storage, direct access to the website does not work.
{: shortdesc}

This behavior is caused by the index document limitation in IBM COS.
{: tsCauses}

You must specify the complete request path in the browser's address bar (for example, `www.example.com/index.html`). For an alternative solution, see [How do I use the ICOS default index page with CDN](/docs/CDN?topic=CDN-configure-cdn-with-ibm-cloud-object-storage#using-icos-default-index-with-cdn).
{: tsResolve}
