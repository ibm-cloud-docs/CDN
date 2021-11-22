---

copyright:
  years: 2018, 2020
lastupdated: "2020-01-08"

keywords: troubleshooting

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I connect through a curl command or browser using the hostname with HTTPS?
{: #troubleshoot-cdn-cannot-connect-https}
{: troubleshoot}
{: support}

If your CDN was created using HTTPS with a wildcard certificate, the connection must be made using the CNAME; for example, `https://www.exampleCname.cdn.appdomain.cloud`.
This includes all CDNs created with HTTPS before 18 June 2018. Trying to connect using the hostname results in an error.
{: shortdesc}
