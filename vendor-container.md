---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Vendor Container
The `SoftLayer_Container_Network_CdnMarketplace_Vendor` collection contains the attributes utilized by our Vendor APIs.


class `SoftLayer_Container_Network_CdnMarketplace_Vendor`  
* `vendorName`: The name of a current IBM Cloud CDN provider.  
* `featureSummary`: A brief summary of the vendor features.  
* `features`: A list of features which are supported by the vendor.  
* `status`: Indicates whether a vendor is an available option through IBM Cloud.


Display a list of the available vendors as well as their features, with a call to the `listVendors` API:

```php
require_once __DIR__.'/vendor/autoload.php';

$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

The previous API call results in output similar to this:

```php
SoftLayer_Container_Network_CdnMarketplace_Vendor Object
(
    [vendorName] => akamai
    [featureSummary] => Performance, Reliability and Scale
    [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
    [status] => ACTIVE
)
```
{: codeblock}
