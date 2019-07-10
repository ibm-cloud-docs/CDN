---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

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

# 使用 CDN API 的程式碼範例
{: #code-examples-using-the-cdn-api}

本文件包含範例 API 呼叫以及眾多 CDN API 的產生輸出。

## 所有 API 呼叫都需要的一般步驟
{: #general-steps-needed-for-all-api-calls}

必要條件是從 [https://github.com/softlayer/softlayer-api-php-client ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/softlayer/softlayer-api-php-client) 下載並安裝 Soap Client

  * 您必須透過 `vendor/autoload` 來取得 SoapClient 的存取權。此路徑相對於從中執行 Script 的位置，而且可能需要適當地修改。在 PHP 中，此陳述式看起來如下：`require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * 所有 API 呼叫都會使用您的使用者名稱及 apiKey 進行鑑別。您可以在 [Softlayer API 起始頁 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/article/getting-started/) 的「取得 API 金鑰」下，找到如何產生 apiKey 的相關資訊。

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * 針對適當的類別，起始設定 SoapClient。

## 列出供應商的程式碼範例
{: #example-code-for-listing-vendors}

在此情況下，它是 SoftLayer_Network_CdnMarketplace_Vendor 類別，可定義 `listVendors` API 而且必須以參數形式傳遞至 `\SoftLayer\SoapClient::getClient()`。當您建立「網域對映」時，稍後會需要作用中供應商的名稱。

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}

```

`listVendor` 程式碼將顯示供應商陣列，以及其狀態和特性。範例輸出會顯示一個作用中供應商 Akamai。

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

## 驗證順序的程式碼範例
{: #example-code-to-verify-order}

在下訂單之前，不需要呼叫 `verifyOrder`，但建議您呼叫它。它可以用來驗證後續的 `placeOrder` 呼叫將成功。您可以在 [SoftLayer API 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/) 中，找到 `verifyOrder` 的相關資訊。

在此情況下，它是 `SoftLayer_Product_Order` 類別，可定義 verifyOrder 方法而且必須以參數形式傳遞至 `\SoftLayer\SoapClient::getClient()`。在呼叫 `verifyOrder` 之前，您需要使用 `SoftLayer_Product_Package` 來建置 `$orderObject`。

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


## 下單的程式碼範例
{: #example-code-to-place-order}

此 API 呼叫與前一個程式碼範例相同，但它呼叫 `placeOrder`，而非 `verifyOrder`。您可以在 [SoftLayer API 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/) 中，找到 `placeOrder` 的相關資訊。

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


## 建立 CDN 或建立網域對映的程式碼範例
{: #example-code-to-create-cdn-or-create-domain-mapping}

此範例顯示如何使用 `createDomainMapping` API 來建立新的 CDN 對映。它採用 `stdClass` 物件的單一參數。應該使用 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 類別來起始設定 SoapClient，如此範例所示。

如果您選擇提供自訂 CNAME，則其結尾**必須**是 `.cdnedge.bluemix.net`，否則將會擲出錯誤。如需提供您專屬 CNAME 的規則，請參閱[本說明](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-)。
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
    $inputObject->cname = "api-testing.cdnedge.bluemix.net";
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

`createDomainMapping` 範例將會顯示新建立 CDN 的屬性。請記下 `uniqueId`，因為您需要將它提供為許多其他 API 的參數。其輸出應類似如下：

```php
Array
(
    [0] => stdClass Object
        (
            [bucketName] => mybucket
            [cacheKeyQueryRule] => include-all
            [certificateType] => SHARED_SAN_CERT
            [cname] => api-testing.cdnedge.bluemix.net
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

## 驗證網域對映的程式碼範例
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping 會檢查 CNAME 配置是否完成，如果完成，則會將 CDN 狀態移至 RUNNING 狀態。呼叫 `verifyDomainMapping` 之前，您必須先將自訂「主機名稱」的 CNAME 記錄新增至 DNS 伺服器。

此範例會呼叫 `verifyDomainMapping`，而所含的 UniqueId 會傳回為 `createDomainMapping` 的一部分。應該使用 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 類別來起始設定 SoapClient，如下列範例所示。

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

如果您的 CNAME 記錄已新增至 DNS 伺服器，則在呼叫 `verifyDomainMapping` 之後，CDN 的 `status` 會變更為 RUNNING，如下列範例所示。

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


如果您的 CNAME 記錄尚未新增至 DNS 伺服器，或尚未更新您的伺服器，則 CDN 的 `status` 將會是 CNAME_CONFIGURATION，如下列範例所示。

可能需要幾分鐘（最多 30 分鐘）的時間，CNAME 鏈結才會完成。
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

若要確定已正確配置 CNAME 記錄，請在指令行上執行 `dig <your domain>`。其輸出應類似如下：

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

在這裡，我們會看到網域名稱已正確對映至 CNAME。
