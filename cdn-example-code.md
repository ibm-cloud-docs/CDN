---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-23"

keywords: code examples, example API calls, CDN API, Soap, client, apiKey

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Code examples using the CDN API
{: #code-examples-using-the-cdn-api}

This document contains example API calls and the resulting output for numerous CDN APIs.

## General steps that are needed for all API calls
{: #general-steps-needed-for-all-api-calls}

Before you begin, you must download and install the Soap Client from [https://github.com/softlayer/softlayer-api-php-client](https://github.com/softlayer/softlayer-api-php-client){:external}

  * You must get access to the SoapClient via `vendor/autoload`. The path is relative to where the script is run from and might need to be modified.

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * All API calls are authenticated with your username and an apiKey. More information on how to generate an apiKey can be found on the [Softlayer API Getting Started](https://softlayer.github.io/article/getting-started/){:external} page, under "Getting Your API Key".

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * Initialize SoapClient for the appropriate class.

## Example code for listing vendors
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

## Example code to verify an order
{: #example-code-to-verify-order}

The call to `verifyOrder` is not mandatory prior to placing an order, but it is recommended. It can be used to verify that a subsequent call to `placeOrder` is successful. More information about `verifyOrder` can be found in the [SoftLayer API documentation](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/){:external}.

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


## Example code to place an order
{: #example-code-to-place-order}

This API call is identical to the previous code example, except it calls `placeOrder`, rather than `verifyOrder.` More information about `placeOrder` can be found in the [SoftLayer API documentation](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/){:external}.

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


## Example code to create a CDN or create a domain mapping
{: #example-code-to-create-cdn-or-create-domain-mapping}

This example shows you how to create a new CDN mapping using the `createDomainMapping` API. It takes a single parameter of a `stdClass` object. The SoapClient should be initialized using the `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` class as shown in the example.

If you choose to provide a custom CNAME, it **must** end with `.cdn.appdomain.cloud` or an error will be thrown. See [this description](/docs/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions) for rules on providing your own CNAME.
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

The `createDomainMapping` example displays the attributes of the newly created CDN. Make note of the `uniqueId`, as you will need to provide it as a parameter for many other APIs. The output should look similar to this:

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

## Example code to verify a domain mapping
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping checks if the CNAME configuration is complete and if so, moves the CDN status to RUNNING status. Before calling `verifyDomainMapping`, you must add a CNAME record of the custom hostname to your DNS server.

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

It can take several minutes (up to 30) for the CNAME chaining to complete.
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

To be sure that your CNAME record is configured correctly, run `dig <your domain>` on the command line. The output should look similar to this one:

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdn.appdomain.cloud.
```
{: codeblock}

Here we see the domain name that is correctly mapped to the CNAME.

## Example code for getting real-time metrics
{: #example-code-for-getting-real-time-metrics}

This example shows you how to get the real-time metrics with `getCustomerRealTimeMetrics` and `getMappingRealTimeMetrics` in the class `SoftLayer_Network_CdnMarketplace_Metrics`.

To get the real-time metrics for a customer, you can call the `getCustomerRealTimeMetrics` API.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Metrics', null, $apiUsername, $apiKey);
try {
    $metrics = $client->getCustomerRealTimeMetrics(
        'akamai',
        1576627200,  // Start timestamp, must be later than 48 hours ago
        1576663200,  // End timestamp, must be later than start time
        60           // Time interval, must be a multiple of 60 seconds
    );
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to get customer real-time metrics: ' . $e->getMessage());
}
```

The `getCustomerRealTimeMetrics` API returns an array of metrics. The following is an example output:

```php

(
    [0] => SoftLayer_Container_Network_CdnMarketplace_Metrics: {
        type: 'REALTIME',
        names: [
            'TotalHits',
            'TotalBandwidth'
        ],
        totals: [
            1301,         # Total hits from the start time to the end time
            20.05         # Total bandwidth from the start time to the end time
        ],
        time: [
            1576627200,
            1576630800,
            1576634400,
            1576638000,
            1576641600,
            1576645200,
            1576648800,
            1576652400,
            1576656000,
            1576659600,
            1576663200
        ],
        yaxis1: [          # Hits divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis2: [          # Bandwidth divided with the given time interval
            1.21,
            2.32,
            0.45,
            1.03,
            3.01,
            2.55,
            4.98,
            2.45,
            10.23,
            2.12,
            8.97
        ]
    }
)
```

To get the real-time metrics for a domain mapping, you can call the `getMappingRealTimeMetrics` API.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Metrics', null, $apiUsername, $apiKey);
try {
    $metrics = $client->getMappingRealTimeMetrics(
        123456789123456,    // Mapping unique id
        1576627200,  // Start timestamp, must be later than 48 hours ago
        1576663200,  // End timestamp, must be later than start time
        60           // Time interval, must be a multiple of 60 seconds
    );
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to get mapping real-time metrics: ' . $e->getMessage());
}
```

The `getMappingRealTimeMetrics` API returns an array of metrics. The following is an example output:

```php

(
    [0] => SoftLayer_Container_Network_CdnMarketplace_Metrics: {
        type: 'REALTIME',
        names: [
            'TotalHits',
            'TotalBandwidth',
            '200',
            '206',
            '2XX',
            '302',
            '304',
            '3XX',
            '401',
            '403',
            '404',
            '412',
            '4XX',
            '5XX',
            'Error',
            'Others'
        ],
        totals: [
            1301,            # Total hits from the start time to the end time
            20.05,           # Total bandwidth from the start time to the end time
            100,             # Total Hits with hits type 200
            200,             # Total Hits with hits type 206
            200,             # Total Hits with hits type 2XX
            300,             # Total Hits with hits type 302
            100,             # Total Hits with hits type 304
            200,             # Total Hits with hits type 3XX
            300,             # Total Hits with hits type 401
            100,             # Total Hits with hits type 403
            200,             # Total Hits with hits type 404
            300,             # Total Hits with hits type 412
            100,             # Total Hits with hits type 4XX
            200,             # Total Hits with hits type 5XX
            200,             # Total Hits with hits type Error
            200              # Total Hits with hits type Others
        ],
        time: [
            1576627200,
            1576630800,
            1576634400,
            1576638000,
            1576641600,
            1576645200,
            1576648800,
            1576652400,
            1576656000,
            1576659600,
            1576663200
        ],
        yaxis1: [          # Total hits divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis2: [         # Total bandwidth divided with the given time interval
            1.21,
            2.32,
            0.45,
            1.03,
            3.01,
            2.55,
            4.98,
            2.45,
            10.23,
            2.12,
            8.97
        ],
        yaxis3: [              # Hits with hits type 200 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis4: [             # Hits with hits type 206 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis5: [             # Hits with hits type 2XX divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis6: [             # Hits with hits type 302 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis7: [             # Hits with hits type 304 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis8: [             # Hits with hits type 3XX divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis9: [             # Hits with hits type 401 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis10: [             # Hits with hits type 403 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis11: [             # Hits with hits type 404 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis12: [             # Hits with hits type 412 divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis13: [             # Hits with hits type 4XX divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis14: [             # Hits with hits type 5XX divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis15: [             # Hits with hits type Error divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ],
        yaxis16: [             # Hits with hits type Others divided with the given time interval
            123,
            23,
            345,
            12,
            33,
            78,
            89,
            87,
            234,
            12,
            43
        ]
    }
)
```

{: codeblock}


## Example code for creating a purge group
{: #create-group-example}

This example shows how to create a purge group.

```php
$client = \SoftLayer\SoapClient::getClient(
    'SoftLayer_Network_CdnMarketplace_Configuration_Cache_PurgeGroup',
    null,
    $apiUsername,
    $apiKey
  );

try {
    $purgeGroup = $client->createPurgeGroup(
        '750352919747xxx',
        'UNIQUE_GROUP_NAME',
        ['/test1.html', 'test2.html'],
        3 // save and also purge files during create
    );
    print_r($purgeGroup);
    print_r("\n");
} catch (\Exception $e) {
    die('createPurgeGroup failed with an exception: ' . $e->getMessage());
}
```

The call returns the following object:

```php
SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_PurgeGroup Object
    (
        [uniqueId] => 750352919747xxx,
        [groupUniqueId] => 618887378480xxx,
        [lastPurge] => '1582188622',
        [name] => 'UNIQUE_GROUP_NAME',
        [saved] => 'SAVED',
        [option] => 3,
        [pathCount] => 2,
        [paths] => ['/test1.html', 'test2.html'],
        [purgeStatus] => 'SUCCESS'
    )
```
{: codeblock}


The rate limit related headers also returned: 

{: #rate-limit-header}

```php
  lastResponseHeaders => 
    [
        X-RateLimit-Purge-Paths-Limit-Burst => 1000,
        X-RateLimit-Purge-Paths-Limit-Per-Second => 20,
        X-RateLimit-Purge-Paths-Remaining => 998
    ]
```
{: codeblock}
