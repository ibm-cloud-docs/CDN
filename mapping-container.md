---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-08-06"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Mapping Container
{: #mapping-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` collection contains the attributes utilized by our Mapping APIs. Each of the Mapping APIs returns a collection of this type.

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: The name of a valid {{site.data.keyword.cloud}} CDN provider.
* `uniqueId`: A 10-digit, system-generated, identifier that is unique to each mapping. Generated when a mapping is created.
* `domain`: Primary CDN name. Also referred to as `hostname`.
* `protocol`: Protocol used to set up services. It can be HTTP, HTTPS or a combination of the two, HTTP_AND_HTTPS.
* `originType`: Type of the Origin host, currently 'HOST_SERVER' or 'OBJECT_STORAGE'.
* `originHost`: Origin server address (either the hostname or the IPv4 address of the Origin Server), which is the endpoint from which to fetch content, or the name of the Bucket where content is stored. It must be less than 511 characters.
* `httpPort`:  Number of the port used for HTTP protocol. Akamai has certain limitations on port numbers for HTTP and HTTPS ports. Please see the [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for allowed port numbers.
* `httpsPort`:  Number of the port used for HTTPS protocol. Akamai has certain limitations on port numbers for HTTP and HTTPS ports. Please the see [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) for allowed port numbers.
* `certificateType`: Type of certificate being used by a mapping. May be `WILDCARD_CERT` or `SHARED_SAN_CERT`. Value will be 0 for HTTP mappings.
* `cname`: Canonical name record that aliases the hostname. It may be provided by the user, or system-generated. If user-provided, it should be less than 64 alphanumeric characters, and it must be unique. If not provided, one will be generated when the mapping is created.
* `respectHeaders`: A Boolean value that, if set to 'true', causes TTL settings in the Origin to override CDN TTL settings.
* `header`: Specifies Host header information used by the Origin server.
* `status`: The mapping's status. Status can be CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED, or ERROR.
* `bucketName`: Bucket name for your S3 object storage.
* `fileExtension`: File extensions that are allowed to be cached.
* `path`: The path to your S3 object storage. Usually found in the object store URL or API section of your service.
* `cacheKeyQueryRule`: The following options are available to configure Cache Key behavior:
  * `include-all`: Include all query arguments **Default**
  * `ignore-all`: Ignore all query arguments
  * `ignore: space separated query-args`: Ignores those specific query arguments. For example, "ignore: query1 query2"
  * `include: space separated query-args`: Includes those specific query arguments. For example, "include: query1 query2"

Of particular note is the `uniqueId`, which is generated when a mapping is created and is used as a parameter to subsequent API calls.

For example, this call to `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

will return an object to similar to this one:

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
        (
            [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
            [cname] => api-testing.cdn.appdomain.cloud
            [domain] => api-testing.cdntesting.net
            [header] => origin.cdntesting.net
            [httpPort] => 80
            [httpsPort] =>
            [originHost] => origin.cdntesting.net
            [originType] => HOST_SERVER
            [path] => /media/
            [performanceConfiguration] => General web delivery
            [protocol] => HTTP
            [respectHeaders] => 1
            [serveStale] => 1
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

Calls to `stopDomainMapping` and `startDomainMapping` will return the same Mapping object, with the `status` being the difference.

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
        (
          ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
        (
          ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
