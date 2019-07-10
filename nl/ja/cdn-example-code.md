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

# CDN API 使用のコード例
{: #code-examples-using-the-cdn-api}

この文書では、多くの CDN API について、API 呼び出しの例および結果の出力を示します。

## すべての API 呼び出しに必要な一般的なステップ
{: #general-steps-needed-for-all-api-calls}

前提条件として、Soap Client を [https://github.com/softlayer/softlayer-api-php-client ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/softlayer/softlayer-api-php-client) からダウンロードしてインストールします

  * `vendor/autoload` を介して SoapClient にアクセスする必要があります。パスは、スクリプトが実行される場所に対する相対パスであり、場合によっては適切に変更する必要があります。 PHP では、ステートメントは `require_once './../vendor/autoload.php';` のようになります。

      ```php
      require_once __DIR__.'/vendor/autoload.php';
      ```

  * すべての API 呼び出しは、ユーザー名と API 鍵で認証されます。 API 鍵の生成方法について詳しくは、[Softlayer API Getting Started![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/article/getting-started/) ページの『Getting Your API Key』を参照してください。

      ```php
      $apiUsername = '<Your username>' ;
      $apiKey = '<Your apiKey>' ;
      ```
  * 適切なクラス用に SoapClient を初期化します。

## ベンダーのリスト表示のコード例
{: #example-code-for-listing-vendors}

このケースでは、SoftLayer_Network_CdnMarketplace_Vendor クラスが `listVendors` API を定義し、`\SoftLayer\SoapClient::getClient()` にパラメーターとして渡される必要があります。 後で、ドメイン・マッピングを作成するときに、アクティブ・ベンダーの名前が必要になります。

```php

$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```

`listVendor` コードは、ベンダーの配列と、各ベンダーの状況および機能を表示します。 出力例には、1 つのアクティブなベンダーである Akamai が示されています。

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

## 注文の確認のコード例
{: #example-code-to-verify-order}

注文する前に `verifyOrder` を呼び出すことは必須ではありませんが、推奨されます。 後続の `placeOrder` 呼び出しが正常終了することを検証するためにこれを使用できます。 `verifyOrder` については [SoftLayer API 資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/verifyOrder/) に詳しく説明されています。

このケースでは、`SoftLayer_Product_Order` クラスが verifyOrder メソッドを定義し、`\SoftLayer\SoapClient::getClient()` にパラメーターとして渡される必要があります。 `verifyOrder` を呼び出す前に、`SoftLayer_Product_Package` を使用して `$orderObject` を構築する必要があります。

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


## 発注のコード例
{: #example-code-to-place-order}

この API 呼び出しは、`verifyOrder` ではなく `placeOrder` を呼び出すことを除いて、前のコード例と同じです。 `placeOrder` については、[SoftLayer API 資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/reference/services/SoftLayer_Product_Order/placeOrder/) に詳しく説明されています。

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


## CDN の作成またはドメイン・マッピングの作成のコード例
{: #example-code-to-create-cdn-or-create-domain-mapping}

この例は、`createDomainMapping` API を使用して新規 CDN マッピングを作成する方法を示しています。 これは、`stdClass` オブジェクトの単一パラメーターを使用します。 例に示されているように、SoapClient は `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` クラスを使用して初期化される必要があります。

カスタム CNAME を指定することを選択する場合は、`.cdnedge.bluemix.net` で終わらなければ**なりません**。そうしないとエラーがスローされます。独自の CNAME を指定する際のルールについては、[この説明](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-custom-cname-naming-conventions-)を参照してください。
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

    // 以下の値が必須です
    $inputObject->vendorName = "akamai";
    $inputObject->origin = "origin.cdntesting.net";
    $inputObject->originType = "HOST_SERVER";
    $inputObject->domain = "api-testing.cdntesting.net";
    $inputObject->protocol = "HTTP";
    $inputObject->httpPort = 80;

    // 以下の値は HTTPS protocol の場合にのみ必須です
    $inputObject->certificateType = "SHARED_SAN_CERT";

    // 以下の値はオプションです
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

`createDomainMapping` 例は、新しく作成された CDN の属性を表示します。 `uniqueId` は、他の多くの API のパラメーターとして指定する必要があるため、メモしておいてください。 出力は次のようになります。

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

## ドメイン・マッピングの確認のコード例
{: #example-code-to-verify-domain-mapping}

VerifyDomainMapping は、CNAME 構成が完了しているかどうかをチェックし、完了している場合は CDN 状況を RUNNING 状況に移します。 `verifyDomainMapping` を呼び出す前に、カスタム・ホスト名の CNAME レコードを DNS サーバーに追加する必要があります。

この例では、`createDomainMapping` の一環として返された UniqueId を使用して `verifyDomainMapping` を呼び出しています。 以下の例に示されているように、SoapClient は `SoftLayer_Network_CdnMarketplace_Configuration_Mapping` クラスを使用して初期化される必要があります。

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

CNAME レコードが DNS サーバーに追加済みの場合、以下の例に示されているように、`verifyDomainMapping` を呼び出した後、CDN の `status` は RUNNING に変わります。

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


CNAME レコードが DNS サーバーに追加されていない場合、またはサーバーがまだ更新されていない場合、以下の例に示されているように、CDN の `status` は CNAME_CONFIGURATION になります。

CNAME チェーニングが完了するまで、数分 (最大 30 分) かかることがあります。
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

CNAME レコードが正しく構成されていることを確認するには、コマンド・ラインで `dig <your domain>` を実行します。 出力は以下のようになります。

```
;; ANSWER SECTION:
api-testing.cdntesting.net. 900	IN	CNAME	api-testing.cdnedge.bluemix.net.
```
{: codeblock}

ここでは、ドメイン・ネームが正しく CNAME にマッピングされているのが分かります。
