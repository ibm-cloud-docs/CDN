---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-31"

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

# Code examples that use the CDN API
{: #code-examples-using-the-cdn-api}

This document contains example API calls and the resulting output for numerous CDN APIs.
{:shortdesc}

## General steps that are needed for all API calls
{: #general-steps-needed-for-all-api-calls}

Before you begin, you must download and install the Soap Client from [https://github.com/softlayer/softlayer-api-php-client](https://github.com/softlayer/softlayer-api-php-client){:external}.

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

In this case, it is the SoftLayer_Network_CdnMarketplace_Vendor class, which defines the `listVendors` API and must be passed as a parameter to `\SoftLayer\SoapClient::getClient()`. You need the name of an active vendor later, when you create a Domain Mapping

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

The `listVendor` code displays an array of vendors, their status, and features. The example output shows one active vendor, Akamai.

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

The call to `verifyOrder` is not mandatory before placing an order, but it is recommended. It can be used to verify that a subsequent call to `placeOrder` is successful. More information about `verifyOrder` can be found in the [SoftLayer API documentation](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/){:external}.

In this case, it is the `SoftLayer_Product_Order` class, which defines the verifyOrder method and must be passed as a parameter to `\SoftLayer\SoapClient::getClient()`. Before the call to `verifyOrder`, you must build the `$orderObject` using the `SoftLayer_Product_Package`.

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

This example shows you how to create a new CDN mapping by using the `createDomainMapping` API. It takes a single parameter of a `stdClass` object. The SoapClient should be initialized by using the `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` class as shown in the example.

If you choose to provide a custom CNAME, it must end with `.cdn.appdomain.cloud.` or an error will be thrown. See [this description](/docs/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions) for rules on providing your own CNAME.
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
    $inputObject->cname = "api-testing.cdn.appdomain.cloud.";
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

The `createDomainMapping` example displays the attributes of the newly created CDN. Make note of the `uniqueId`, as you must provide it as a parameter for many other APIs. The output should look similar to this:

```php
Array
(
    [0] => stdClass Object
        (
            [bucketName] => mybucket
            [cacheKeyQueryRule] => include-all
            [certificateType] => SHARED_SAN_CERT
            [cname] => api-testing.cdn.appdomain.cloud.
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

This example calls `verifyDomainMapping` with the UniqueId that was returned as part of `createDomainMapping`. The SoapClient should be initialized by using the `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` class as shown in the following example.

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

If your CNAME record was added to your DNS server, the `status` of your CDN will change to RUNNING after the call to `verifyDomainMapping`, as shown in the following example.

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


If your CNAME record was not added to your DNS server, or your server has not yet updated, the `status` of your CDN is CNAME_CONFIGURATION, as shown in the following example.

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

## Example code for getting integrated metrics
{: #example-code-for-getting-integrated-metrics}

This example shows you how to get the integrated metrics for a domain mapping with `getMappingIntegratedMetrics` API in the class `SoftLayer_Network_CdnMarketplace_Metrics`.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Metrics', null, $apiUsername, $apiKey);
try {
    $metrics = $client->getMappingIntegratedMetrics(
        123456789123456,    // Mapping unique id
        1590969600,         // Start timestamp, must be later than 90 days ago
        1591315200,         // End timestamp, must be later than start time
        'day'               // The data frequency, can be 'aggregate' and 'day', default is 'day'.
    );
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to get mapping integrated metrics: ' . $e->getMessage());
}
```

The `getMappingIntegratedMetrics` API returns an array of metrics. The following is an example output:

```php

{
    "0": {
        "type": "INTEGRATED",
        "names": [
            "TotalHits",
            "TotalBandwidth",
            "0XX",
            "200",
            "206",
            "2XX",
            "302",
            "304",
            "3XX",
            "404",
            "4XX",
            "5XX",
            "Other",
            "Australasia",
            "EMEA",
            "India",
            "Japan",
            "North America",
            "Rest Of APAC",
            "South America"
        ],
        "descriptions": [
            "All hits to the edge servers from the end-users.",
            "Total number of megabytes transferred between the edge to the end user.",
            "Number of hits that returned response code - 0XX",
            "Number of hits that returned response code - 200",
            "Number of hits that returned response code - 206",
            "Number of hits that returned response code - 2XX",
            "Number of hits that returned response code - 302",
            "Number of hits that returned response code - 304",
            "Number of hits that returned response code - 3XX",
            "Number of hits that returned response code - 404",
            "Number of hits that returned response code - 4XX",
            "Number of hits that returned response code - 5XX",
            "Number of hits that returned response code not within 2XX to 5XX",
            "Total number of megabytes transferred between the edge to the end user in the region - Australasia",
            "Total number of megabytes transferred between the edge to the end user in the region - EMEA",
            "Total number of megabytes transferred between the edge to the end user in the region - India",
            "Total number of megabytes transferred between the edge to the end user in the region - Japan",
            "Total number of megabytes transferred between the edge to the end user in the region - North America",
            "Total number of megabytes transferred between the edge to the end user in the region - Rest Of APAC",
            "Total number of megabytes transferred between the edge to the end user in the region - South America"
        ],
        "totals": [
            15,             // Total Hits from start time to end time.
            4.9301e-5,      // Total Bandwidth from start time to end time.
            0,              // Hits by type 0XX
            0,              // Hits by type 200
            0,              // Hits by type 206
            0,              // Hits by type 2XX
            0,              // Hits by type 302
            0,              // Hits by type 304
            0,              // Hits by type 3XX
            0,              // Hits by type 404
            13,             // Hits by type 4XX
            2,              // Hits by type 5XX
            0,              // Hits by type Other
            0,              // Bandwidth by region Australasia
            3.6554e-5,      // Bandwidth by region EMEA
            0,              // Bandwidth by region India
            0,              // Bandwidth by region Japan
            1.1524e-5,      // Bandwidth by region North America
            1.223e-6,       // Bandwidth by region Rest Of APAC
            0               // Bandwidth by region South America
        ],
        "percentage": [ // The percentage of the bandwidth by regions
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            null,
            0,                  // Australasia
            74.14454067868806,  // EMEA
            0,                  // India
            0,                  // Japan
            23.374779416239022, // North America
            2.480679905072919,  // Rest Of APAC
            0                   // South America
        ],
        "time": [
            "1590969600",
            "1591142400",
            "1591228800",
            "1591315200"
        ],
        "xaxis": null,
        "yaxis1": [     // TotalHits per day
            4,
            1,
            6,
            4
        ],
        "yaxis2": [     // TotalBandwidth per day
            6.371999999999999e-6,
            4.525e-6,
            2.7596e-5,
            1.0808e-5
        ],
        "yaxis3": [     // Hits by type 0XX per day
            0,
            0,
            0,
            0
        ],
        "yaxis4": [     // Hits by type 200 per day
            0,
            0,
            0,
            0
        ],
        "yaxis5": [     // Hits by type 206 per day
            0,
            0,
            0,
            0
        ],
        "yaxis6": [     // Hits by type 2XX per day
            0,
            0,
            0,
            0
        ],
        "yaxis7": [     // Hits by type 302 per day
            0,
            0,
            0,
            0
        ],
        "yaxis8": [     // Hits by type 304 per day
            0,
            0,
            0,
            0
        ],
        "yaxis9": [     // Hits by type 3XX per day
            0,
            0,
            0,
            0
        ],
        "yaxis10": [    // Hits by type 404 per day
            0,
            0,
            0,
            0
        ],
        "yaxis11": [    // Hits by type 4XX per day
            2,
            1,
            6,
            4
        ],
        "yaxis12": [    // Hits by type 5XX per day
            2,
            0,
            0,
            0
        ],
        "yaxis13": [    // Hits by type Other per day
            0,
            0,
            0,
            0
        ],
        "yaxis14": null,
        "yaxis15": null,
        "yaxis16": null,
        "yaxis17": null,
        "yaxis18": null,
        "yaxis19": null,
        "yaxis20": null
    }
}
```

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

## Example code for creating a token authentication
{: #create-token-auth-example}

This example shows how to create a token authentication.

```php
$client = \SoftLayer\SoapClient::getClient(
    'SoftLayer_Network_CdnMarketplace_Configuration_Behavior_TokenAuth',
    null,
    $apiUsername,
    $apiKey
  );

try {
    $inputObject = new stdClass();

    $inputObject->uniqueId = "351752925563xxx"; // CDN Domain mapping unique ID
    $inputObject->path = "/path/to/protect/*"; // Path you want to protect

    // CDN Token authentication configurations
    $inputObject->tokenKey = "abc123"; // Primary encryption key
    $inputObject->transitionKey = "def987"; // Transition encryption key (optional)
    $inputObject->name = "__token__"; // Token name (optional)

    $tokenAuth = $client->createTokenAuth($inputObject);
    print_r($tokenAuth);
    print_r("\n");
} catch (\Exception $e) {
    die('createTokenAuth failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}

The call returns the following object:

```php
SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth Object
    (
        [mappingId] => '351752925563xxx',
        [path] => '/private/*',
        [tokenKey] => 'abc123',
        [transitionKey] => 'def987',
        [name] => '__token__'
    )
```
{: codeblock}
