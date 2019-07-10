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

# 使用 CDN API 的代码示例
{: #code-examples-using-the-cdn-api}

此文档包含大量 CDN API 的示例 API 调用和生成的输出。

## 所有 API 调用所需的常规步骤
{: #general-steps-needed-for-all-api-calls}

先决条件是从 [https://github.com/softlayer/softlayer-api-php-client ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/softlayer/softlayer-api-php-client) 下载并安装 SOAP 客户机

  * 您必须通过 `vendor/autoload` 获取对 SoapClient 的访问权。该路径相对于脚本运行位置，可能需要对其进行相应修改。使用 PHP 编写的语句将类似于：`require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * 所有 API 调用均使用您的用户名和 API 密钥进行认证。有关如何生成 API 密钥的更多信息可以在 [Softlayer API Getting Started ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer.github.io/article/getting-started/) 页面上的“Getting Your API Key”下找到。

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * 使用相应的类来初始化 SoapClient。

## 列出供应商的示例代码
{: #example-code-for-listing-vendors}

在本例中，这是 SoftLayer_Network_CdnMarketplace_Vendor 类，用于定义 `listVendors` API，并且必须作为参数传递给 `\SoftLayer\SoapClient::getClient()`。稍后创建域映射时，您将需要活动供应商的名称。

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}

```

`listVendor` 代码将显示一组供应商及其状态和功能。示例输出显示了一个活动供应商 Akamai。

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

## 验证订单的示例代码
{: #example-code-to-verify-order}

在下订单之前，调用 `verifyOrder` 不是必需的，但建议您执行此操作。它可用于验证对 `placeOrder` 的后续调用是否成功。有关 `verifyOrder` 的更多信息可以在 [SoftLayer API 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/) 中找到。

在本例中，这是 `SoftLayer_Product_Order` 类，用于定义 verifyOrder 方法，并且必须作为参数传递给 `\SoftLayer\SoapClient::getClient()`。在调用 `verifyOrder` 之前，需要使用 `SoftLayer_Product_Package` 来构建 `$orderObject`。

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


## 下订单的示例代码
{: #example-code-to-place-order}

此 API 调用与先前的代码示例相同，只是调用的是 `placeOrder`，而不是 `verifyOrder`。有关 `placeOrder` 的更多信息可以在 [SoftLayer API 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/) 中找到。

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


## 创建 CDN 或创建域映射的示例代码
{: #example-code-to-create-cdn-or-create-domain-mapping}

此示例显示如何使用 `createDomainMapping` API 来创建新的 CDN 映射。它采用了 `stdClass` 对象的单个参数。应该使用示例中所示的 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 类来初始化 SoapClient。

如果您选择提供定制 CNAME，那么它**必须**以 `.cdnedge.bluemix.net` 结尾，否则将抛出错误。请参阅[此描述](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-)，以获取有关提供您自己的 CNAME 的规则。
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

`createDomainMapping` 示例将显示新创建的 CDN 的属性。请记下 `uniqueId`，因为需要将其作为其他许多 API 的参数提供。输出应该类似于以下内容：

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

## 验证域映射的示例代码
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping 会检查 CNAME 配置是否完成，如果完成，会将 CDN 状态移至 RUNNING 状态。在调用 `verifyDomainMapping` 之前，必须向 DNS 服务器添加定制主机名的 CNAME 记录。

此示例使用作为 `createDomainMapping` 的一部分返回的 UniqueId 来调用 `verifyDomainMapping`。应该使用 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 类来初始化 SoapClient，如以下示例中所示。

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

如果 CNAME 记录已添加到 DNS 服务器，那么在调用 `verifyDomainMapping` 后，CDN 的 `status` 将更改为 RUNNING，如以下示例中所示。

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


如果 CNAME 记录尚未添加到 DNS 服务器，或者服务器尚未更新，那么 CDN 的 `status` 将为 CNAME_CONFIGURATION，如以下示例中所示。

完成 CNAME 链接可能需要几分钟时间（最长 30 分钟）。
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

要确保 CNAME 记录配置正确，请在命令行上运行 `dig <your domain>`。输出应该类似于以下内容：

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

在此我们可以看到域名已正确映射到 CNAME。
