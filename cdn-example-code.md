---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-08-06"

keywords: code examples, example API calls, CDN API, Soap, client, apiKey

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Code Examples Using the CDN API
{: #code-examples-using-the-cdn-api}

This document contains example API calls and the resulting output for numerous CDN APIs.

## General steps needed for all API calls
{: #general-steps-needed-for-all-api-calls}

The pre-requisite is to download and install the Soap Client from [https://github.com/softlayer/softlayer-api-php-client ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/softlayer/softlayer-api-php-client)

  * You must get access to the SoapClient via `vendor/autoload`. The path is relative to where the script is run from and may need to be modified appropriately. In PHP the statement will look similar to this: `require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * All API calls are authenticated with your username and an apiKey. More information on how to generate an apiKey can be found on the [Softlayer API Getting Started page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://softlayer.github.io/article/getting-started/) page, under "Getting Your API Key".

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * Initialize SoapClient for the appropriate class.

## Example Code for Listing Vendors
{: #example-code-for-listing-vendors}

In this case, it is the SoftLayer_Network_CdnMarketplace_Vendor class which defines the `listVendors` API and must be passed as a parameter to `\SoftLayer\SoapClient::getClient()`. You will need the name of an active vendor later, when you create a Domain Mapping

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

The `listVendor` code will display an array of vendors, as well as their status and features. The example output shows one active vendor, Akamai.

```php
Array
(
    [0] => stdClass Object
        (
            [featureSummary] => Performance, Reliability and Scale
            [features] => Web Delivery, Content Caching, Content Purge, HTTP/HTTPS Support
            [status] => ACTIVE
            [vendorName] => akamai
        )

)
```

{: codeblock}

## Example Code to Verify Order
{: #example-code-to-verify-order}

The call to `verifyOrder` is not mandatory prior to placing an order, but it is recommended. It can be used to verify that a subsequent call to `placeOrder` will be successful. More information about `verifyOrder` can be found in the [SoftLayer API documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/).

In this case, it is the `SoftLayer_Product_Order` class which defines the verifyOrder method and must be passed as a parameter to `\SoftLayer\SoapClient::getClient()`. Prior to the call to `verifyOrder`, you need to build the `$orderObject` using the `SoftLayer_Product_Package`.

```php
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

try {
    $filter = new stdClass();
    $filter->keyName = new stdClass();
    $filter->keyName->operation = 'CONTENT_DELIVERY_NETWORK_SERVICE';

    $client->setObjectFilter($filter);
    $client->setObjectMask(
        "mask[
            itemPrices.item[
                attributes.attributeType,
                bundleItems.activeUsagePrices[
                    priceType,
                    pricingLocationGroup.locations
                ]
            ]
        ]"
    );

    $package = $client->getAllObjects()[0];

    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    $productOrderClient = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Order', null, $apiUsername, $apiKey);

    $result = $productOrderClient->verifyOrder($orderObject);
    print_r($result);
    print_r("\n");
}

catch (\Exception $e) {
    die('Verify Order failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}


## Example code to Place Order
{: #example-code-to-place-order}

This API call is identical to the previous code example, except it calls `placeOrder`, rather than `verifyOrder.` More information about `placeOrder` can be found in the [SoftLayer API documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/).

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

try {
    $filter = new stdClass();
    $filter->keyName = new stdClass();
    $filter->keyName->operation = 'CONTENT_DELIVERY_NETWORK_SERVICE';

    $client->setObjectFilter($filter);
    $client->setObjectMask(
        "mask[
            itemPrices.item[
                attributes.attributeType,
                bundleItems.activeUsagePrices[
                    priceType,
                    pricingLocationGroup.locations
                ]
            ]
        ]"
    );

    $package = $client->getAllObjects()[0];

    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    $productOrderClient = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Order', null, $apiUsername, $apiKey);

    $result = $productOrderClient->placeOrder($orderObject);
    print_r($result);
    print_r("\n");
}

catch (\Exception $e) {
    die('Place order failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}


## Example code to create CDN or Create Domain Mapping
{: #example-code-to-create-cdn-or-create-domain-mapping}

This example shows you how to create a new CDN mapping using the `createDomainMapping` API. It takes a single parameter of a `stdClass` object. The SoapClient should be initialized using the `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` class as shown in the example.

If you choose to provide a custom CNAME, it **must** end with `.cdn.appdomain.cloud` or an error will be thrown. See [this description](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-) for rules on providing your own CNAME.
{: important}

```php

$client = \SoftLayer\SoapClient::getClient(
    'SoftLayer_Network_CdnMarketplace_Configuration_Mapping',
    null,
    $apiUsername,
    $apiKey
  );


try {
    $inputObject = new stdClass();

    // The following values are required
    $inputObject->vendorName = "akamai";
    $inputObject->origin = "origin.cdntesting.net";
    $inputObject->originType = "HOST_SERVER";
    $inputObject->domain = "api-testing.cdntesting.net";
    $inputObject->protocol = "HTTP";
    $inputObject->httpPort = 80;

    // The following value is required only for HTTPS protocol
    $inputObject->certificateType = "SHARED_SAN_CERT";

    // The following values are optional
    $inputObject->cname = "api-testing.cdn.appdomain.cloud";
    $inputObject->path = "/media";
    $inputObject->header = '';
    $inputObject->respectHeader = true;
    $inputObject->bucketName = 'mybucket';
    $inputObject->fileExtension = "txt, jpeg";
    $inputObject->cacheKeyQueryRule = "include-all";

    $cdnMapping = $client->createDomainMapping($inputObject);
    print_r($cdnMapping);

} catch (\Exception $e) {
    die('createDomainMapping failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}

The `createDomainMapping` example will display the attributes of the newly created CDN. Make note of the `uniqueId`, as you will need to provide it as a parameter for many other APIs. The output should look similar to this:

```php
Array
(
    [0] => stdClass Object
        (
            [bucketName] => mybucket
            [cacheKeyQueryRule] => include-all
            [certificateType] => SHARED_SAN_CERT
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
            [status] => CNAME_CONFIGURATION
            [uniqueId] => 610345992629xxx
            [vendorName] => akamai
        )

)
```
{: codeblock}

## Example Code to Verify Domain Mapping
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping checks if the CNAME configuration is complete and if so, moves the CDN status to RUNNING status. Before calling `verifyDomainMapping`, you must add a CNAME record of the custom Hostname to your DNS server.

This example calls `verifyDomainMapping` with the UniqueId that was returned as part of `createDomainMapping`. The SoapClient should be initialized using the `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` class as shown in the following example.

```php

$client = \SoftLayer\SoapClient::getClient(
    'SoftLayer_Network_CdnMarketplace_Configuration_Mapping',
    null,
    $apiUsername,
    $apiKey
  );

try {
    $uniqueId = 610345992629xxx;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}

If your CNAME record has been added to your DNS server, the `status` of your CDN will change to RUNNING after the call to `verifyDomainMapping`, as shown in the following example.

```php

    [0] => stdClass Object
        (
          ...

            [status] => RUNNING
            [uniqueId] => 610345992629xxx

          ...
        )
```
{: codeblock}


If your CNAME record has not been added to your DNS server, or your server has not yet updated, the `status` of your CDN will be CNAME_CONFIGURATION, as shown in the following example.

It may take several minutes (up to 30) for the CNAME chaining to complete.
{: note}

```php
Array
(
    [0] => stdClass Object
        (
          ...

            [status] => CNAME_CONFIGURATION
            [uniqueId] => 610345992629xxx

          ...
        )

)
```
{: codeblock}

To be sure your CNAME record is configured correctly, run `dig <your domain>` on the command line. The output should look similar to this one:

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdn.appdomain.cloud.
```
{: codeblock}

Here we see the domain name correctly mapped to the CNAME.
