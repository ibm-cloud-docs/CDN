---

copyright:
  years: 2018, 2019
lastupdated: "2019-08-06"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

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

# Rules and Naming Conventions
{: #rules-and-naming-conventions}

## What are the rules for the CDN Hostname?
{: #what-are-the-rules-for-the-cdn-hostname}

The CDN `Hostname` input string **must**:
  * consist of alphanumeric characters
  * be less than 254 characters
  * end with a valid top-level domain name
  * must **not** contain more than 10 labels
  * must **not** end in `cdn.appdomain.cloud` or `cdnedge.bluemix.net` (that ending is used for CNAMES and is reserved)

Please refer to RFC 1035, section 2.3.4 for more details. 

Furthermore, we highly recommend using a fully-qualified domain name as your CDN Hostname. Choose a name of the form `www.example.com` rather than a root domain name (also referred as Zone Apex or Naked domain), of the form `example.com`. You'll need to create a CNAME Record for the CDN Hostname you use, and the DNS RFC 1033 requires the root domain record to be an A Record, not a CNAME. Further clarification is provided in RFC 2181, section 10.1

## What are the custom CNAME naming conventions?
{: #what-are-the-custom-cname-naming-conventions}

The `CNAME` input string must adhere to the following rules:
  * **must** be unique (it cannot be in use by any other IBM Cloud CDN)
  * less than 64 alphanumeric characters
  * special characters `! @ # $ % ^ & *` are **not** allowed
  * underscores `_` are **not** allowed
  * periods `.` are **not** allowed
  * dashes `-` are allowed, but the CNAME cannot begin or end with a dash

## What are the rules for Bucket names?
{: #what-are-the-rules-for-bucket-names}

Bucket names:
  * **must** be at least 1 character
  * must be less than 200 characters
  * may contain letters (both uppercase and lowercase letters are allowed), numbers, and hyphens
  * must **not** be formatted as an IP address (for example, 127.0.0.1)
  * may **not** start with a period (.)
  * may end with a period (.)
  * only one period (.) is allowed between labels (for example, my..ibmcloud.bucket is not allowed).

Although uppercase letters and periods can pass validation, we suggest that you always use DNS-compliant Bucket names.
{: note}

## What are the rules for the Path string for the Origin?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

The Path is optional when creating your CDN. However, if provided, the Path **must**:
  * be less than 1000 characters in length
  * begin with `/`

## What are the rules for the Path string for Purge?
{: #what-are-the-rules-for-the-path-string-for-purge}

The Purge path:
  * must be less than 1000 characters in length
  * must begin with `/`
  * cannot end with `/`
  * cannot end with a period (.)
  * cannot contain `*`

Purge is allowed for single files only. Directory-level purge is not supported at this time.
{; note}

## For the **Add Origin** command, are there any rules to follow for the Path string?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

For **Add Origin**, the path is **mandatory**. Also if your CDN was created with a path, the path for **Add Origin** must start with the CDN path as the prefix. For example, if the CDN path was specified as `/storage`, to invoke **Add Origin** with a path called `/examplePath`, the path provided would be `/storage/examplePath`.

## What are the rules for providing file extensions?
{: #what-are-the-rules-for-providing-file-extensions}

When creating an Origin with Object Storage, file extensions should be comma separated. For example, `txt, jpg, xml` is a valid list. Any one particular extension cannot be longer than 10 characters.
