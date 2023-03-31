---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-20"

keywords: troubleshooting, case, questions

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Getting help and support for CDN
{: #gettinghelp}

If you experience an issue or have questions when using CDN, you can use the following resources before you open a support case.
{: shortdesc}

* Review [FAQs](/docs/CDN?topic=CDN-faqs) in the product documentation.
* Review [Troubleshooting](/docs/CDN?topic=CDN-troubleshoot-cdn-working) to diagnose and resolve common issues.
* Check the status of the {{site.data.keyword.cloud_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
* Review [Stack Overflow](https://stackoverflow.com/search?q=cdn+ibm-cloud){: external} to see whether other users ran into the same problem. If you're using the forum to ask a question, tag your question with `ibm-cloud` and `cdn` so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.

If you still can't resolve the problem, you can open a support case. For more information, see [Creating support cases](/docs/get-support?topic=get-support-open-case).

## Providing support case details for CDN
{: #support-case-details}

To ensure that the support team has all of the details for investigating your issue to provide a timely resolution, you must provide detailed information about your issue. Review the following tips about the type of information to include in your support case for issues with CDN.

Provide the following details:

1. Go to the [All CDNs page](https://{DomainName}/cdn).
2. From the UI, provide the following information:

    * (Required) Hostname, for example, `example.testingcdn.net`
    * CNAME, for example, `example.cdn.appdomain.cloud`
    * HTTPS status, for example, `Yes (Wildcard)`
    * Status, for example, `Running`

   From the API, run `listDomainMappings` or `listDomainMappingByUniqueId` to get the following domain mapping information:

    * (Required) `domain`
    * `cname`
    * `protocol`
    * `status`
