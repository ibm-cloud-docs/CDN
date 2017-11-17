---

copyright:
  years: 2017
lastupdated: "2017-11-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Mapping Container  
The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` collection contains the attributes utilized by our mapping APIs. Each one of the mapping APIs returns a collection of this type. 

class `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:  

* `vendorName`: The name of a current {{site.data.keyword.BluSoftlayer_notm}} provider.  
* `uniqueId`: A 10-digit, system-generated, identifier that is unique to each mapping.  
* `domain`: Primary CDN name. Also referred to as `hostname`.  
* `protocol`: Protocol used to set up services. It can be HTTP, HTTPS or a combination of the two, HTTP\_AND\_HTTPS.  
* `originType`: Type of the Origin host, currently 'HOST_SERVER' or 'OBJECT_STORAGE'.  
* `originHost`: Origin server address (either the hostname or the IPv4 address of the Origin Server), which is the endpoint from which to fetch content, or the name of the Bucket where content is stored. It must be less than 511 characters.  
* `httpPort`: Number of the port used for HTTP protocol.  
* `httpsPort`: Number of the port used for HTTPS protocol.  
* `cname`: Canonical name record that aliases the hostname. It may be provided by the user, or system-generated. If user-provided, it should be less than 64 alphanumeric characters, and it must be unique.   
* `respectHeaders`: A Boolean value that, if set to 'true', causes TTL settings in the Origin to override CDN TTL settings.  
* `header`: Specifies Host header information used by the Origin server.  
* `status`: The mapping's status. Valid statuses include: RUNNING, STOPPED, ERROR, DELETED, CNAME\_CONFIGURATION and SSL\_CONFIGURATION.  
* `bucketName`: Unique name of your bucket for S3 object storage.
* `fileExtension`: File extensions that are allowed to be cached.  
* `path`: The path to your S3 object storage. Usually found in the object store URL or API section of your service.  

Of particular note is the `uniqueId`, which is generated when a mapping is created and is used as a parameter to subsequent API calls.

For example, this call to `listDomainMappingByUniqueid`  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid(d'750352919747xxx');  
```

will return an object to similar to this one:

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
                    [vendorName] => akamai
                    [uniqueId] => 750352919747xxx
                    [domain] => test.testingcdn.net
                    [protocol] => HTTP_AND_HTTPS
                    [originType] => HOST_SERVER
                    [originHost] => test.testingcdn.net
                    [httpPort] => 80
                    [httpsPort] => 443
                    [cname] => test.cdnedge.bluemix.net
                    [performanceConfiguration] => 
                    [certificateType] => 
                    [respectHeaders] => 1
                    [header] =>
                    [status] => RUNNING
                    [bucketName] =>
                    [fileExtension] =>
                    [path] => /
                )
```

