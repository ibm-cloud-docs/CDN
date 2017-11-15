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

# Input Container
The Input Container is a collection utilized by both the Mapping and (Origin) Path classes.  It provide a consistent set of input attributes for both classes. 

* `vendorName`: The name of a current IBM Cloud CDN provider.  
* `oldPath`: Used by updateDomainMapping(). This property stores the name of the current, or 'old', path.

The following attributes are common to both the Mapping and (Origin) Path classes:  

* `originType`: Type of the Origin host, currently 'HOST_SERVER' or 'OBJECT_STORAGE'.  
* `origin`: Origin server address (either the hostname or the IPv4 address of the Origin Server), the endpoint from which to fetch content, or the name of the Bucket where content is stored. Must be less than 511 characters.   
* `httpPort`: Number of the port used for HTTP protocol.  
* `httpsPort`: Number of the port used for HTTPS protocol.  
* `status`:  The status of the mapping or path, i.e. RUNNING, or CNAME_CONFIGURATION.  
* `path`: When used by the updateDomainMapping API, this attribute refers to the new path to be added.  
* `performanceConfiguration`

The following attributes are specific to the Mapping class:  

* `uniqueId`: A 10-digit, system-generated, identifier that is unique to each mapping.
* `domain`: Primary CDN name. Also referred to as host name.  
* `protocol`: Protocol used to set up services. Can be HTTP, HTTPS or a combination of the two, HTTP\_AND\_HTTPS.  
* `cname`: Canonical name record aliases the hostname. May be provided by the user, or system-generated. If user-provided, it should be less than 64 alphanumeric characters and must be unique.  
* `certificateType`: Type of certificate being used by a Mapping, i.e. Wildcard.  
* `respectHeaders`: A boolean value that, if set 'true', will cause TTL settings in the Origin to override CDN TTL settings.  
* `header`: Header for the mapping.

The following attributes are related to Cloud Object Storage (COS):  
* `bucketName`: Unique name of your bucket for S3 object storage.  
* `fileExtension`: File extensions that are allowed.
