---

copyright:
  years: 2018
lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Rules and Naming Conventions 

## What are the rules for the Hostname?
The `Hostname` input string **must**:
  * consist of alphanumeric characters
  * be less than 254 characters
  * end with a valid top-level domain name
  * must **not** end in `cdnedge.bluemix.net` (that ending is used for CNAMES and is reserved)

Please refer to RFC 1035, section 2.3.4 for more details.

## What are the custom CNAME naming conventions?
The `CNAME` input string must adhere to the following rules:
  * **must** be unique (it cannot be in use by any other IBM Cloud CDN)
  * less than 64 alphanumeric characters
  * special characters `! @ # $ % ^ & *` are **not** allowed
  * underscores `_` are **not** allowed
  * periods `.` are **not** allowed
  * dashes `-` are allowed, but the CNAME cannot begin or end with a dash

## What are the rules for Bucket names?
Bucket names:
  * **must** be at least 1 character
  * must be less than 200 characters
  * may contain letters (both uppercase and lowercase letters are allowed), numbers, and hyphens
  * must **not** be formatted as an IP address (for example, 127.0.0.1)
  * may **not** start with a period (.)
  * may end with a period (.)
  * only one period (.) is allowed between labels (for example, my..ibmcloud.bucket is not allowed). 

**NOTE**: Although uppercase letters and periods can pass validation, we suggest that you always use DNS-compliant Bucket names.

## What are the rules for the Path string for the Origin?
The Path is optional when creating your CDN. However, if provided, the Path **must**:
  * be less than 1000 characters in length
  * begin with `/`

## For the **Add Origin** command, are there any rules to follow for the Path string?
For **Add Origin**, the path is **mandatory**. Also if your CDN was created with a path, the path for **Add Origin** must start with the CDN path as the prefix. For example, if the CDN path was specified as `/storage`, to invoke **Add Origin** with a path called `/examplePath`, the path provided would be `/storage/examplePath`.

## What are the rules for providing file extensions?
When creating an Origin with Object Storage, file extensions should be comma separated. For example, `txt, jpg, xml` is a valid list. Any one particular extension cannot be longer than 10 characters.
