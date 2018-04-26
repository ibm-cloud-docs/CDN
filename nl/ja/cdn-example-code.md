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

# CDN API 使用のコード例

## ベンダーのリストのコード例

前提条件として、Soap Client を https://github.com/softlayer/softlayer-api-php-client からダウンロードしてインストールします

```
// CDN Soap API 呼び出しを示すコード例
// このコードは、IBM CDN サービスがサポートしているベンダーを、実行元の端末に出力します

// ステップ 1: vendor/autoload 経由で SoapClient にアクセスします。 パスは、スクリプトの実行元の
// 場所に対する相対パスであり、場合によっては適切に変更する必要があります
require_once './../vendor/autoload.php';

// ステップ 2: 有効な API ユーザー名と API キーを指定します
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// ステップ 3: 適切なクラスの SoapClient を初期化します。 この例の場合は、listVendors メソッドを定義する
// SoftLayer_Network_CdnMarketplace_Vendor クラスです
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Vendor', null, $apiUsername, $apiKey);

// ステップ 4: listVendors メソッドを呼び出し、ベンダーを出力します
// スクリプトが正常に実行されると、ベンダーが端末に表示されます
try {
    $vendors = $client->listVendors();
    print_r($vendors);
} catch (\Exception $e) {
    die('Unable to retrieve list of vendors: ' . $e->getMessage());
}
```
{: codeblock}

## 注文の確認のコード例 

```
<?php

// このコードは、CDN を正常に発注できるかどうかを確認します

// ステップ 1: vendor/autoload 経由で SoapClient にアクセスします。 パスは、スクリプトの実行元の
// 場所に対する相対パスであり、場合によっては適切に変更する必要があります
require_once './../vendor/autoload.php';

// ステップ 2: 有効な API ユーザー名と API キーを指定します
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// ステップ 3: SoftLayer_Product_Package クラスの API クライアントを初期化します
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// ステップ 4: 正しいパッケージ・オブジェクトを取得するために、フィルターとマスクを設定します
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

    // ステップ 5: OrderData オブジェクトを初期化します
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // ステップ 6: SoapVar orderObject を作成します
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // ステップ 7: Product Order Client を取得し、verifyOrder を呼び出します
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

```
<?php

// このコードは、ステップ 7 の placeOrder の呼び出しを除き、VerifyOrder と同一です。 
// PlaceOrder は、verifyOrder が正常に実行された後にのみ呼び出します 

// ステップ 1: vendor/autoload 経由で SoapClient にアクセスします。 パスは、スクリプトの実行元の
// 場所に対する相対パスであり、場合によっては適切に変更する必要があります
require_once './../vendor/autoload.php';

// ステップ 2: 有効な API ユーザー名と API キーを指定します
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;


// ステップ 3: SoftLayer_Product_Package クラスの API クライアントを初期化します
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Product_Package', null, $apiUsername, $apiKey);

// ステップ 4: 正しいパッケージ・オブジェクトを取得するために、フィルターとマスクを設定します
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

    // ステップ 5: OrderData オブジェクトを初期化します
    $orderData = new stdClass();
    $orderData->packageId = $package->id;
    $orderData->prices = $package->itemPrices;

    // ステップ 6: SoapVar orderObject を作成します
    $orderObject = new SoapVar(
        $orderData,
        SOAP_ENC_OBJECT,
        'SoftLayer_Container_Product_Order_Network_ContentDelivery_Service',
        'http://api.service.softlayer.com/soap/v3/'
    );

    // ステップ 7: Product Order Client を取得し、placeOrder を呼び出します
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

```
<?php

// これは、createDomainMapping を呼び出して新しい CDN を作成する方法を示すコード例です

// ステップ 1: vendor/autoload 経由で SoapClient にアクセスします。 パスは、スクリプトの実行元の
// 場所に対する相対パスであり、場合によっては適切に変更する必要があります
require_once './../vendor/autoload.php';

// ステップ 2: 有効な API ユーザー名と API キーを指定します
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// ステップ 3: 適切なクラスの SoapClient を初期化します。 この例の場合は、createDomainMapping メソッドを定義する
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping クラスです
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// ステップ 4: $input オブジェクトを初期化し、createDomainMapping を呼び出します
try {
    $input = new stdClass();

    // ベンダー名を指定します
    $input->vendorName = "akamai";

    // ホスト名と Cname を指定します
    $input->domain = "beta.testingcdn.net";
    $input->cname = "beta.cdnedge.bluemix.net";

    // オリジン・タイプを指定します。 ここでは「HOST_SERVER」が選択されています
    $input->originType = "HOST_SERVER";

    // オリジン・アドレス、プロトコル、およびポート番号を指定します
    $input->origin = "testserver.testingcdn.net";
    $input->protocol = "HTTP";
    $input->httpPort = 80;

    // オプションを指定します
    $input->respectHeaders = true;

    $cdnMapping = $client->createDomainMapping($input);
    print_r($cdnMapping);
} catch (\Exception $e) {
    die('createDomainMapping failed with an exception: ' . $e->getMessage());
}

```
{: codeblock}

## ドメイン・マッピングの確認のコード例

```
<?php

// verifyDomainMapping を呼び出す前に、お客様はカスタム・ホスト名の CNAME レコードを DNS サーバーに追加する必要があります。
// このコードは、createDomainMapping の一部として返された UniqueId を使用して verifyDomainMapping を呼び出します。
// VerifyDomainMapping は、CNAME 構成が完了しているかどうかを確認し、完了している場合は、CDN 状況を RUNNING 状況に遷移します

// ステップ 1: vendor/autoload 経由で SoapClient にアクセスします。 パスは、スクリプトの実行元の
// 場所に対する相対パスであり、場合によっては適切に変更する必要があります
require_once './../vendor/autoload.php';

// ステップ 2: 有効な API ユーザー名と API キーを指定します
$apiUsername = '<Your username>' ;
$apiKey = '<Your apiKey>' ;

// ステップ 3: 適切なクラスの SoapClient を初期化します。 この例の場合は、verifyDomainMapping メソッドを定義する
// SoftLayer_Network_CdnMarketplace_Configuration_Mapping クラスです
$client = \SoftLayer\SoapClient::getClient('SoftLayer_Network_CdnMarketplace_Configuration_Mapping', null, $apiUsername, $apiKey);

// ステップ 4: $uniqueId を初期化し、verifyDomainMapping を呼び出します
try {
    $uniqueId = 907987176256747;

    $cdnMapping = $client->verifyDomainMapping($uniqueId);

    print_r($cdnMapping);

} catch (\Exception $e) {
    die('verifyDomainMapping failed with an exception: ' . $e->getMessage());
}
```
{: codeblock}
