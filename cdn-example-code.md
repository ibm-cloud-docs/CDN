---

copyright:
  years: 2017
lastupdated: "2017-10-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Code Examples Using the CDN API

## Example Code of Listing Vendors

The pre-requisite is to download and install the Soap Client from https://github.com/softlayer/softlayer-api-php-client

```
// Example code to demonstrate CDN Soap API call
// This code prints the IBM CDN Service supported vendors on the terminal it is run from

// Step 1: Get access to the SoapClient via vendor/autoload. The path is relative to where
// the script is run from and may need to be modified appropriately
require_once './../vendor/autoload.php';

// Step 2: Provide valid API Username and API Key
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Step 3: Initialize SoapClient for the appropriate class. In this case, it is the
// SoftLayer_Network_CdnMarketplace_Vendor class which defines the listVendors method
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);

// Step 4: Call the listVendors method and print the vendors
// When successful the script will display the vendors on the terminal
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

## Example Code to Verify Order 

```
<?php

// This code verifies if a CDN order can be successfully placed

// Step 1: Get access to the SoapClient via vendor/autoload. The path is relative to where
// the script is run from and may need to be modified appropriately
require_once './../vendor/autoload.php';

// Step 2: Provide valid API Username and API Key
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// Step 3: Initialize an API client for the SoftLayer_Product_Package class
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Step 4: Set Filter and Mask to get the right package object
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

    // Step 5: Initialize OrderData object
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Step 6: Create SoapVar orderObject
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Step 7: Get Product Order Client and call verifyOrder
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

```
<?php

// This code is identical to VerifyOrder except Step 7 which calls placeOrder. 
// PlaceOrder should be called only after a successful verifyOrder 

// Step 1: Get access to the SoapClient via vendor/autoload. The path is relative to where
// the script is run from and may need to be modified appropriately
require_once './../vendor/autoload.php';

// Step 2: Provide valid API Username and API Key
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// Step 3: Initialize an API client for the SoftLayer_Product_Package class
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// Step 4: Set Filter and Mask to get the right package object
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

    // Step 5: Initialize OrderData object
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // Step 6: Create SoapVar orderObject
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // Step 7: Get Product Order Client and call placeOrder
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

```
<?php

// This is example code to show how to call createDomainMapping to create a new CDN

// Step 1: Get access to the SoapClient via vendor/autoload. The path is relative to where
// the script is run from and may need to be modified appropriately
require_once './../vendor/autoload.php';

// Step 2: Provide valid API Username and API Key
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Step 3: Initialize SoapClient for the appropriate class. In this case, it is the
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping class which defines the createDomainMapping method
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Step 4: Initialize the $input object and call createDomainMapping
try {
    $input = new stdClass();

    // Provide the vendor Name
    $input->vendorName = "akamai";

    // Specify Hostname and Cname
    $input->domain = "beta.testingcdn.net";
    $input->cname = "beta.cdnedge.bluemix.net";

    // Specify Origin Type. Here "HOST_SERVER" is chosen
    $input->originType = "HOST_SERVER";

    // Specify Origin address, protocol and Port number
    $input->origin = "testserver.testingcdn.net";
    $input->protocol = "HTTP";
    $input->httpPort = 80;

    // Specify Options
    $input->respectHeaders = true;

    $cdnMapping = $client->createDomainMapping($input);
    print_r($cdnMapping);
} catch (\Exception $e) {
    die('createDomainMapping failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}

## Example Code to Verify Domain Mapping

```
<?php

// Before calling verifyDomainMapping, the customer has to add a CNAME record of the custom Hostname to the DNS server.
// This code calls verifyDomainMapping with the UniqueId that was returned as part of createDomainMapping.
// VerifyDomainMapping checks if the CNAME configuration is complete and if so, moves the CDN status to RUNNING status

// Step 1: Get access to the SoapClient via vendor/autoload. The path is relative to where
// the script is run from and may need to be modified appropriately
require_once './../vendor/autoload.php';

// Step 2: Provide valid API Username and API Key
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// Step 3: Initialize SoapClient for the appropriate class. In this case, it is the
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping class which defines the verifyDomainMapping method
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// Step 4: Initialize the $uniqueId and call verifyDomainMapping
try {
    $uniqueId = 907987176256747;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}
