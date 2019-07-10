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

# CDN API를 사용하는 코드 예제
{: #code-examples-using-the-cdn-api}

이 문서에는 다양한 CDN API의 결과 출력과 예제 API 호출이 포함되어 있습니다.

## 모든 API 호출에 필요한 일반 단계
{: #general-steps-needed-for-all-api-calls}

전제조건을 보려면 [https://github.com/softlayer/softlayer-api-php-client ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/softlayer/softlayer-api-php-client)에서 Soap 클라이언트를 다운로드하여 설치하십시오.

  * `vendor/autoload`를 통해 SoapClient에 액세스해야 합니다. 경로는 스크립트가 실행되는 위치에 상대적이며 적절하게 수정해야 할 수 있습니다. PHP에서 명령문은 다음과 비슷합니다. `require_once './../vendor/autoload.php';`

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * 모든 API 호출은 사용자 이름과 apiKey를 사용하여 인증됩니다. apiKey를 생성하는 방법에 대한 자세한 정보는 [Softlayer API 시작하기 페이지 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/article/getting-started/) 페이지의 "API 키 가져오기"에 있습니다.

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * 해당 클래스의 SoapClient를 초기화하십시오.

## 벤더 나열의 예제 코드
{: #example-code-for-listing-vendors}

이 경우 `listVendors` API를 정의하는 SoftLayer_Network_CdnMarketplace_Vendor 클래스이고 매개변수로 `\SoftLayer\SoapClient::getClient()`에 전달해야 합니다. 도메인 맵핑을 작성할 때 활성 벤더의 이름이 필요합니다.

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

`listVendor` 코드는 일련의 벤더와 해당 상태 및 기능을 표시합니다. 예제 출력에는 활성 벤더인 Akamai가 표시됩니다.

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

## 주문 확인을 위한 예제 코드
{: #example-code-to-verify-order}

주문하기 전에 `verifyOrder`를 호출하는 것이 권장되지만 필수는 아닙니다. `placeOrder`이 후속 호출이 성공적인지 확인하는 데 사용할 수 있습니다. `verifyOrder`에 대한 자세한 정보는 [SoftLayer API 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/)에 있습니다.

이 경우 verifyOrder 메소드를 정의하는 `SoftLayer_Product_Order` 클래스이고 매개변수로 `\SoftLayer\SoapClient::getClient()`에 전달해야 합니다. `verifyOrder`를 호출하기 전에 `SoftLayer_Product_Package`를 사용하여 `$orderObject`를 빌드해야 합니다.

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


## 주문 배치를 위한 예제 코드
{: #example-code-to-place-order}

이 API 호출은 `verifyOrder`가 아니라 `placeOrder`를 호출한다는 점을 제외하고는 이전 코드 예제와 동일합니다. `placeOrder`에 대한 자세한 정보는 [SoftLayer API 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/)에 있습니다.

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


## CDN 작성 또는 도메인 맵핑 작성을 위한 예제 코드
{: #example-code-to-create-cdn-or-create-domain-mapping}

이 예에서는 `createDomainMapping` API를 사용하여 새로운 CDN 맵핑을 작성하는 방법을 보여줍니다. `stdClass` 오브젝트의 매개변수 하나를 사용합니다. SoapClient는 다음 예에 표시된 대로 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 클래스를 사용하여 초기화해야 합니다.

사용자 정의 CNAME을 제공하도록 선택하는 경우 `.cdnedge.bluemix.net`으로 종료**되어야** 합니다. 그렇지 않으면 오류로 처리됩니다. 고유 CNAME을 제공하는 데 관한 규칙은 [이 설명](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-)을 참조하십시오.
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

`createDomainMapping` 예에서는 새로 작성한 CDN의 속성을 표시합니다. 여러 다른 API의 매개변수로 제공해야 하므로 `uniqueId`를 기록해 두십시오. 출력은 다음과 비슷해야 합니다.

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

## 도메인 맵핑 확인을 위한 예제 코드
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping에서는 CNAME 구성이 완료되었는지 확인하고, 그런 경우 CDN 상태를 RUNNING 상태로 이동합니다. `verifyDomainMapping`을 호출하기 전에 사용자 정의 호스트 이름의 CNAME 레코드를 DNS 서버에 추가해야 합니다.

이 예에서는 `createDomainMapping`의 일부로 리턴한 UniqueId로 `verifyDomainMapping`을 호출합니다. SoapClient는 다음 예에 표시된 대로 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` 클래스를 사용하여 초기화해야 합니다.

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

CNAME 레코드가 DNS 서버에 추가된 경우 다음 예에 표시된 대로 `verifyDomainMapping`을 호출한 후 CDN의 `status`가 RUNNING으로 변경됩니다.

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


CNAME 레코드가 DNS 서버에 추가되지 않거나 서버가 아직 업데이트되지 않은 경우 다음 예에 표시된 대로 CDN의 `status`가 CNAME_CONFIGURATION이 됩니다.

CNAME 체인이 완료되려면 몇 분(최대 30분)이 소요될 수 있습니다.
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

CNAME 레코드가 올바르게 구성되었는지 확인하려면 명령행에서 `dig <your domain>`을 실행하십시오. 출력은 다음과 같습니다.

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

여기에서 CNAME에 올바르게 맵핑된 도메인 이름이 표시됩니다.
