---

copyright:
  years: 2018, 2020
lastupdated: "2020-03-27"

keywords: hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for rules and naming conventions
{: #rules-and-naming-conventions}

Have a question about CDN rules and naming conventions? Review these frequently asked questions, which provide answers to common inquiries.
{: shortdesc}

## What are the rules for the CDN hostname?
{: #what-are-the-rules-for-the-cdn-hostname}

The CDN `Hostname` input string must:

* Consist of alphanumeric characters
* Be fewer than 254 characters
* End with a valid top-level domain name
* Must not contain more than 10 labels
* Must not end in `cdn.appdomain.cloud.` or `cdnedge.bluemix.net.` (that ending is used for CNAMES and is reserved)

Refer to RFC 1035, section 2.3.4 for more details.

Furthermore, we highly recommend the use of a fully qualified domain name as your CDN hostname. Choose a name of the form `www.example.com` rather than a root domain name (also referred as Zone Apex or Naked domain), of the form `example.com`. You must create a CNAME Record for the CDN hostname you use, and the DNS RFC 1033 requires the root domain record to be an A Record, not a CNAME. Further clarification is provided in RFC 2181, section 10.1

## What are the custom CNAME naming conventions?
{: #what-are-the-custom-cname-naming-conventions}

The `CNAME` input string must adhere to the following rules:

* Must be unique (it cannot be in use by any other IBM Cloud CDN)
* Less than 64 alphanumeric characters
* Special characters `! @ # $ % ^ & *` are not allowed
* Underscores `_` are not allowed
* Periods `.` are not allowed
* Dashes `-` are allowed, but the CNAME cannot begin or end with a dash

## What are the rules for Bucket names?
{: #what-are-the-rules-for-bucket-names}

Bucket names:

* Must be at least one character
* Must be fewer than 200 characters
* Can contain letters (both uppercase and lowercase letters are allowed), numbers, and hyphens
* Must not be formatted as an IP address (for example, 127.0.0.1)
* Cannot start with a period (.)
* Can end with a period (.)
* Only one period (.) is allowed between labels (for example, my..ibmcloud.bucket is not allowed).

Although uppercase letters and periods can pass validation, we suggest that you always use DNS-compliant Bucket names.
{: note}

## What are the rules for the Path string for the origin?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

The Path is optional when creating your CDN. However, if provided, the Path:

* Must be less than 1000 characters in length
* Must begin with `/`

## What are the rules for the Path string for Purge?
{: #what-are-the-rules-for-the-path-string-for-purge}

The purge path:

* Cannot contain `*`
* Cannot contain spaces
* Cannot end with a period (.)
* Must begin with `/`
* Must be English characters
* Must be separated by line breaks
* Must be less than 1000 characters in length
* Must be less than 2 M characters in total
* No duplicate paths

For the imported purge file, it is recommended to use a `txt` file extension, and all paths in the file follow these rules.

## For the **Add Origin** command, are there any rules to follow for the Path string?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

For **Add Origin**, the path is **mandatory**. Also, if your CDN was created with a path, the path for **Add Origin** must start with the CDN path as the prefix. For example, if the CDN path was specified as `/storage`, to invoke **Add Origin** with a path called `/examplePath`, the path that is provided would be `/storage/examplePath`.

## What are the rules for providing file extensions?
{: #what-are-the-rules-for-providing-file-extensions}

When creating an origin with Object Storage, file extensions should be comma that is separated. For example, `txt, jpg, xml` is a valid list. Any one particular extension cannot be longer than 10 characters.
