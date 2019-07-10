---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: application programming interface, api, slapi, reference, development interface

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}


# CDN API 참조
{: #cdn-api-reference}

{{site.data.keyword.cloud}} 인프라 API(Application Programming Interface)(일반적으로 SLAPI라고 함)는 {{site.data.keyword.cloud}}에서 제공되며, 개발자와 시스템 관리자에게 {{site.data.keyword.cloud_notm}} 인프라 백엔드 시스템과의 직접 상호작용을 제공하는 개발 인터페이스입니다. 

SLAPI는 고객 포털에서 다수의 기능을 구현합니다. 고객 포털에서 상호작용이 가능하면 SLAPI에서도 수행 가능합니다. SLAPI 내에서 {{site.data.keyword.cloud_notm}} 인프라 환경의 모든 부분과 상호작용할 수 있기 때문에 API를 사용하여 태스크를 자동화할 수 있습니다.

SLAPI는 원격 프로시저 호출(RPC) 시스템입니다. 각 호출에는 API 엔드포인트로 데이터 전송 및 그에 대한 구조화된 데이터 수신이 포함됩니다. SLAPI로 데이터를 전송하고 수신하는 데 사용되는 형식은 사용자가 선택하는 API의 구현에 따라 다릅니다. SLAPI는 현재 데이터 전송을 위해 SOAP, XML-RPC 또는 REST를 사용합니다.

SLAPI 또는 {{site.data.keyword.cloud_notm}} Content Delivery Network 서비스 API에 대한 자세한 정보는 {{site.data.keyword.cloud_notm}} 개발 네트워크에서 다음 리소스를 참조하십시오.

* [SLAPI 개요](https://softlayer.github.io/ )
* [SLAPI 시작하기](https://softlayer.github.io/article/getting-started/ )
* [SoftLayer_Product_Package API](https://softlayer.github.io/reference/services/SoftLayer_Product_Package/ )
* [PHP Soap API 안내서](https://softlayer.github.io/article/php/ )

----

시작하기 위해 따라야 하는 권장 API 호출 시퀀스는 다음과 같습니다.
* `listVendors` - 지원되는 벤더의 목록을 제공합니다.
* `verifyOrder` - 주문 여부를 확인합니다.
* `placeOrder` - 지정된 벤더로 CDN 계정을 작성합니다. placeOrder 호출 성공 후에 최대 10개의 CDN 맵핑을 작성할 수 있습니다.
* `createDomainMapping` - CDN 맵핑을 작성합니다.
* `verifyDomainMapping` - CDN 상태를 _RUNNING_으로 변경합니다.

이전 시퀀스를 따른 후에 나머지 API를 사용할 수 있습니다.

[예제 코드가 이 호출 시퀀스의 각 단계에 대해 사용 가능합니다.](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)

이 문서에 표시된 대부분의 API 호출에서 `CDN_ACCOUNT_MANAGE` 권한을 가진 사용자의 API 키와 API 사용자 이름을 사용**해야 합니다**. 이 권한을 사용해야 하는 경우 계정의 마스터 사용자에게 문의하십시오. (각 IBM Cloud 고객 계정은 하나의 마스터 사용자에게 제공됩니다.)
{: note}

----
## 벤더에 대한 API
{: #api-for-vendor}

### listVendors
이 API는 사용자가 지원되는 CDN 벤더를 나열하도록 허용합니다. CDN 계정을 작성하고 CDN 주문을 시작하려면 `vendorName`이 필요합니다.

* **필수 매개변수**: 없음
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Vendor`의 콜렉션

  벤더 컨테이너 및 사용 예제는 다음에서 볼 수 있습니다. [벤더 컨테이너](/docs/infrastructure/CDN?topic=CDN-vendor-container)

----
## 계정에 대한 API
{: #api-for-account}

### verifyCdnAccountExists
CDN 계정이 API를 호출하는 사용자를 위해 존재하는지 아니면 제공된 `vendorName`을 위해 존재하는지를 확인합니다.

* **필수 매개변수**: `vendorName`: 올바른 CDN 제공자의 이름을 제공하십시오.
* **리턴**: 계정이 있는 경우 `true`, 없는 경우 `false`입니다.

----
## 도메인 맵핑에 대한 API
{: #api-for-domain-mapping}

### createDomainMapping
이 기능은 제공된 입력을 사용하여 제공된 벤더를 위해 도메인 맵핑을 작성하고 이를 사용자의 {{site.data.keyword.cloud_notm}} 인프라 계정 ID와 연관시킵니다. 이 API를 작동하려면 먼저 `placeOrder`를 사용하여 CDN 계정을 작성해야 합니다([코드 예제](/docs/infrastructure/CDN?topic=CDN-code-examples-using-the-cdn-api)에서 `placeOrder` API 호출의 예제 참조). CDN을 작성한 후 `defaultTTL`은 3,600초의 값으로 작성됩니다.

* **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
  여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

  [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

  다음 속성은 입력 컨테이너의 일부이며 도메인 맵핑을 작성할 때 제공될 수 있습니다(별도의 언급이 없으면 속성은 선택사항임).
    * `vendorName`: **필수** 올바른 IBM Cloud CDN 제공자의 이름을 제공하십시오.
    * `origin`: **필수** 원본 서버 주소를 문자열로 제공하십시오.
    * `originType`: **필수** 원본 유형은 `HOST_SERVER` 또는 `OBJECT_STORAGE`입니다.
    * `domain`: **필수** 호스트 이름을 문자열로 제공하십시오.
    * `protocol`: **필수** 지원되는 프로토콜은 `HTTP`, `HTTPS` 또는 `HTTP_AND_HTTPS`입니다.
    * `certificateType`: HTTPS 프로토콜의 경우 **필수**입니다. `SHARED_SAN_CERT `또는 `WILDCARD_CERT`입니다.
    * `path`: 캐시된 컨텐츠를 제공하는 데 사용할 경로입니다. 기본 경로는 `/*`입니다.
    * `httpPort` 및/또는 `httpsPort`: (호스트 서버에 대해 **필수**) 이러한 두 옵션은 원하는 프로토콜에 해당해야 합니다. 프로토콜이 `HTTP`인 경우 `httpPort`를 설정해야 하고 `httpsPort`는 설정하면 _안 됩니다_. 마찬가지로 프로토콜이 `HTTPS`인 경우 `httpsPort`를 설정해야 하고 `httpPort`는 설정하면 _안 됩니다_. 프로토콜이 `HTTP_AND_HTTPS`인 경우 `httpPort`와 `httpsPort`를 _둘 다_ 설정_해야 합니다_. Akamai에는 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호는 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
    * `header`: 원본 서버에서 사용되는 호스트 헤더 정보를 지정합니다.
    * `respectHeader`: `true`로 설정된 경우 원본의 TTL 설정이 CDN TTL 설정을 대체하게 하는 부울 값입니다.
    * `cname`: 호스트 이름에 대한 별명을 제공하십시오. 제공하지 않으면 생성됩니다.
    * `bucketName`: (Object Storage에 대해서만 **필수**) S3 Object Storage의 버킷 이름입니다.
    * `fileExtension`: (Object Storage의 경우 선택사항) 캐시할 수 있는 파일 확장자입니다.
    * `cacheKeyQueryRule`: 다음 옵션은 캐시 키 동작을 구성하는 데 사용 가능합니다. `cacheKeyQueryRule` 인수가 제공되지 않으면 기본값 "include-all"로 설정됩니다.
      * `include-all` - 모든 조회 인수를 포함합니다. **기본값**
      * `ignore-all` - 모든 조회 인수를 무시합니다.
      * `ignore: space separated query-args` - 특정 조회 인수를 무시합니다. 예를 들어, `ignore: query1 query2`입니다.
      * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, `include: query1 query2`입니다.

* **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션.

  **참고**: 콜렉션은 `uniqueId` 값을 제공하며, 이 값은 맵핑 및 원본 경로와 관련된 후속 API 호출에 대한 입력으로 전송되어야 합니다.

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### deleteDomainMapping
`uniqueId`를 기반으로 한 도메인 맵핑을 삭제합니다. 도메인 맵핑의 상태는 _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME_CONFIGURATION_ 또는 _SSL_CONFIGURATION_ 중 하나여야 합니다.

* **필수 매개변수**: `uniqueId`: 삭제할 맵핑의 고유 ID
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 콜렉션
  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### verifyDomainMapping
CDN 상태를 확인하고 변경된 경우 CDN 맵핑의 `status`를 업데이트합니다. CDN 맵핑이 처음 작성되면 상태는 _CNAME_CONFIGURATION_으로 표시됩니다. 이 때 CDN 맵핑 호스트 이름이 CNAME을 가리키도록 DNS 레코드를 업데이트해야 합니다. 업데이트 수행 방법 및 변경사항이 인터넷에 전파되려면 시간이 얼마나 소요되는지에 대한 질문이 있는 경우 DNS 제공자와 함께 확인하십시오. 일반적으로 15분에서 30분이 소요됩니다. 이 시간 후에 CNAME 체인이 완료되었는지 확인하기 위해 이 `verifyDomainMapping` API를 호출해야 합니다. CNAME 체인이 완료되면 CDN 맵핑 상태가 _RUNNING_으로 변경됩니다.

최신 CDN 맵핑 상태를 가져오기 위해 언제라도 이 API를 호출할 수 있습니다. 도메인 맵핑은 _RUNNING_ 또는 _CNAME_CONFIGURATION_ 상태 중 하나여야 합니다.

* **필수 매개변수**: `uniqueId`: 확인할 맵핑의 고유 ID
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 콜렉션

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### startDomainMapping
`uniqueId`를 기반으로 한 CDN 도메인 맵핑을 시작합니다. 시작하려면 도메인 맵핑의 상태는 _STOPPED_여야 합니다.

* **필수 매개변수**: `uniqueId`: 시작할 맵핑의 uniqueId
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 콜렉션

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### stopDomainMapping
`uniqueId`를 기반으로 한 CDN 도메인 맵핑을 중지합니다. 중지를 시작하려면 도메인 맵핑의 상태는 _RUNNING_이어야 합니다.

* **필수 매개변수**: `uniqueId`: 중지할 맵핑의 uniqueId
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 콜렉션

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### updateDomainMapping
사용자가 `uniqueId`로 식별된 맵핑의 특성을 업데이트하도록 합니다. `originHost`, `httpPort`, `httpsPort`, `respectHeader`, `header` 및 `cacheKeyQueryRule` 인수 필드가 변경될 수 있으며, 원본 유형이 Object Storage인 경우 `bucketName` 및 `fileExtension`도 변경될 수 있습니다. 업데이트가 실행되려면 도메인 맵핑의 상태는 _RUNNING_이어야 합니다.

* **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
  다음에서 입력 컨테이너의 모든 속성을 볼 수 있습니다.
  [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

  다음 속성은 입력 컨테이너의 일부이며 도메인 맵핑을 업데이트할 때 제공**해야 합니다**.
    * `vendorName`: 이 맵핑에 대한 CDN 제공자의 이름을 제공하십시오.
    * `path`: 이 맵핑에 대한 현재 경로를 제공하십시오.
    * `origin`: 원본 서버 주소를 문자열로 제공하십시오.
    * `originType`: 원본 유형은 `HOST_SERVER` 또는 `OBJECT_STORAGE`입니다.
    * `domain`: 호스트 이름을 제공하십시오.
    * `protocol`: 지원되는 프로토콜은 `HTTP`, `HTTPS` 또는 `HTTP_AND_HTTPS`입니다.
    * `httpPort` 및/또는 `httpsPort`: 이러한 두 옵션은 원하는 프로토콜에 해당해야 합니다. 프로토콜이 `HTTP`인 경우 `httpPort`를 설정해야 하고 `httpsPort`는 설정하면 _안 됩니다_. 마찬가지로 프로토콜이 `HTTPS`인 경우 `httpsPort`를 설정해야 하고 `httpPort`는 설정하면 _안 됩니다_. 프로토콜이 `HTTP_AND_HTTPS`인 경우 `httpPort`와 `httpsPort`를 _둘 다_ 설정_해야 합니다_. Akamai에는 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호는 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
    * `header`: 원본 서버에서 사용되는 호스트 헤더 정보를 지정합니다.
    * `respectHeader`: `true`로 설정된 경우 원본의 TTL 설정이 CDN TTL 설정을 대체하게 하는 부울 값입니다.
    * `uniqueId`: 맵핑이 작성된 후 생성됩니다.
    * `cname`: cname을 제공하십시오. 제공하지 않으면 맵핑이 작성된 후 생성됩니다.
    * `bucketName`: (Object Storage에 대해서만 **필수**) S3 Object Storage의 버킷 이름입니다.
    * `fileExtension`: (Object Storage에 대해서만 **필수**) 캐시할 수 있는 파일 확장자입니다.
    * `cacheKeyQueryRule`: 캐시 키 동작 규칙은 2017년 11월 16일 _이후_ 작성된 CDN 맵핑에 대해서만 업데이트할 수 있습니다. 다음 옵션은 캐시 키 동작을 구성하는 데 사용 가능합니다.
      * `include-all` - 모든 조회 인수를 포함합니다. **기본값**
      * `ignore-all` - 모든 조회 인수를 무시합니다.
      * `ignore: space separated query-args` - 특정 조회 인수를 무시합니다. 예를 들어, `ignore: query1 query2`입니다.
      * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, `include: query1 query2`입니다.
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 콜렉션

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappings
현재 고객에 대한 모든 도메인 맵핑의 콜렉션을 리턴합니다.

* **필수 매개변수**: 없음
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 콜렉션

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
### listDomainMappingByUniqueId
CDN의 `uniqueId`를 기반으로 한 단일 도메인 오브젝트를 사용하여 콜렉션을 리턴합니다.

* **필수 매개변수**: `uniqueId`: 리턴할 맵핑의 고유 ID
* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`의 단일 오브젝트 콜렉션

  [맵핑 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-mapping-container)

----
## 원본에 대한 API
{: #apis-for-origin}

### createOriginPath
기존 CDN과 특정 고객에 대해 원본 경로를 작성합니다. 원본 경로는 호스트 서버 또는 Object Storage를 기반으로 할 수 있습니다. 원본 경로를 작성하려면 도메인 맵핑의 상태는 _RUNNING_ 또는 _CNAME_CONFIGURATION_ 중 하나여야 합니다.

* **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
  여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

  [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

  다음 속성은 입력 컨테이너의 일부이며 원본 경로를 작성할 때 제공될 수 있습니다(별도의 언급이 없으면 속성은 선택사항임).
    * `vendorName`: **필수** 올바른 IBM Cloud CDN 제공자의 이름을 제공하십시오.
    * `origin`: **필수** 원본 서버 주소를 문자열로 제공하십시오.
    * `originType`: **필수** 원본 유형은 `HOST_SERVER` 또는 `OBJECT_STORAGE`입니다.
    * `domain`: **필수** 호스트 이름을 문자열로 제공하십시오.
    * `protocol`: **필수** 지원되는 프로토콜은 `HTTP`, `HTTPS` 또는 `HTTP_AND_HTTPS`입니다.
    * `path`: 캐시된 컨텐츠를 제공하는 데 사용할 경로입니다. 맵핑 경로로 시작해야 합니다. 예를 들어, 맵핑 경로가 `/test`인 경우 원본 경로는 `/test/media`입니다.
    * `httpPort` 및/또는 `httpsPort`: **필수** 이러한 두 옵션은 원하는 프로토콜에 해당해야 합니다. 프로토콜이 `HTTP`인 경우 `httpPort`를 설정해야 하고 `httpsPort`는 설정하면 _안 됩니다_. 마찬가지로 프로토콜이 `HTTPS`인 경우 `httpsPort`를 설정해야 하고 `httpPort`는 설정하면 _안 됩니다_. 프로토콜이 `HTTP_AND_HTTPS`인 경우 `httpPort`와 `httpsPort`를 _둘 다_ 설정_해야 합니다_. Akamai에는 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호는 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
    * `header`: 원본 서버에서 사용되는 호스트 헤더 정보를 지정합니다.
    * `uniqueId`: **필수** 맵핑이 작성된 후 생성됩니다.
    * `cname`: 호스트 이름에 대한 별명을 제공하십시오. 고유 cname을 제공하지 않으면 맵핑이 작성될 때 생성됩니다.
    * `bucketName`: (Object Storage에 대해 **필수**) S3 Object Storage의 버킷 이름입니다.
    * `fileExtension`: (Object Storage의 경우 선택사항) 캐시할 수 있는 파일 확장자입니다.
    * `cacheKeyQueryRule`: 다음 옵션은 캐시 키 동작을 구성하는 데 사용 가능합니다.
      * `include-all` - 모든 조회 인수를 포함합니다. **기본값**
      * `ignore-all` - 모든 조회 인수를 무시합니다.
      * `ignore: space separated query-args` - 특정 조회 인수를 무시합니다. 예를 들어, `ignore: query1 query2`입니다.
      * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, `include: query1 query2`입니다.

* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`의 콜렉션

  [원본 경로 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### updateOriginPath
기존 맵핑과 특정 고객에 대해 기존 원본 경로를 업데이트합니다. 이 API를 사용하여 원본 유형을 변경할 수 없습니다. `path`, `origin`, `httpPort`, `httpsPort`, `header` 및 `cacheKeyQueryRule` 인수 특성을 변경할 수 있습니다. 업데이트하려면 도메인 맵핑의 상태는 _RUNNING_ 또는 _CNAME_CONFIGURATION_ 중 하나여야 합니다.

* **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
  여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

  [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

  다음 속성은 입력 컨테이너의 일부이며 원본 경로를 업데이트할 때 제공될 수 있습니다(별도의 언급이 없으면 속성은 선택사항임).
    * `oldPath`: **필수** 변경할 현재 경로
    * `origin`: (업데이트하는 경우 **필수**) 원본 서버 주소를 문자열로 제공하십시오.
    * `originType`: **필수** 원본 유형은 `HOST_SERVER` 또는 `OBJECT_STORAGE`입니다.
    * `path`: **필수** 추가할 새 경로입니다. 맵핑 경로에 상대적입니다.
    * `httpPort` 및/또는 `httpsPort`: (업데이트하는 경우 호스트 서버에 대해 **필수**) 이러한 두 옵션이 원하는 프로토콜에 해당해야 합니다. 프로토콜이 `HTTP`인 경우 `httpPort`를 설정해야 하고 `httpsPort`는 설정하면 _안 됩니다_. 마찬가지로 프로토콜이 `HTTPS`인 경우 `httpsPort`를 설정해야 하고 `httpPort`는 설정하면 _안 됩니다_. 프로토콜이 `HTTP_AND_HTTPS`인 경우 `httpPort`와 `httpsPort`를 _둘 다_ 설정_해야 합니다_. Akamai에는 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호는 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
    * `uniqueId`: **필수** 이 원본이 속한 맵핑의 고유 ID
    * `bucketName`: (Object Storage에 대해서만 **필수**) S3 Object Storage의 버킷 이름입니다.
    * `fileExtension`: (Object Storage에 대해서만 **필수**) 캐시할 수 있는 파일 확장자입니다.
    * `cacheKeyQueryRule`: (업데이트하는 경우 **필수**) 2017년 11월 16일 _이후_ 작성된 원본 경로에 대해서만 캐시 키 동작 규칙을 업데이트할 수 있습니다. 다음 옵션은 캐시 키 동작 구성에 사용 가능합니다.
      * `include-all` - 모든 조회 인수를 포함합니다. **기본값**
      * `ignore-all` - 모든 조회 인수를 무시합니다.
      * `ignore: space separated query-args` - 특정 조회 인수를 무시합니다. 예를 들어, `ignore: query1 query2`입니다.
      * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, `include: query1 query2`입니다.

* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`의 콜렉션

  [원본 경로 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
### deleteOriginPath
기존 CDN과 특정 고객에 대해 기존 원본 경로를 삭제합니다. 삭제하려면 도메인 맵핑의 상태는 _RUNNING_ 또는 _CNAME_CONFIGURATION_ 중 하나여야 합니다.

* **필수 매개변수**:
  * `uniqueId`: 이 원본 경로가 속한 맵핑의 고유 ID
  * `path`: 삭제할 경로

* **리턴**: 삭제가 성공한 경우 상태 메시지가 나타나고 삭제되지 않으면 예외가 발생합니다.

----
### listOriginPath
`uniqueId`를 기반으로 한 기존 맵핑의 원본 경로를 나열합니다.

* **필수 매개변수**:
  * `uniqueId`: 원본 경로를 나열할 맵핑의 고유 ID를 제공하십시오.
* **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`인 오브젝트의 콜렉션

  [원본 경로 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-path-origin-container)

----
## 제거에 대한 API
{: #api-for-purge}

### 제거를 위한 컨테이너 클래스:
```
class SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var string
     */
    public $status;

    /**
     * @var string
     */
    public $saved;

    /**
     * @var string
     */
    public $date;
}  
```

### createPurge
제거 레코드를 작성하고 데이터베이스에 이를 삽입합니다.

* **매개변수**:
  * `uniqueId`: 제거가 작성될 맵핑의 고유 ID
  * `path`: 작성할 제거의 경로

* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`의 콜렉션

----
### getPurgeHistoryPerMapping
`uniqueId` 및 `saved` 상태를 기반으로 한 CDN의 제거 히스토리를 리턴합니다. (기본적으로 `saved` 값이 _UNSAVED_로 설정됩니다.)

* **매개변수**:
  * `uniqueId`: 제거 히스토리를 검색할 맵핑의 고유 ID
  * `saved`: _SAVED_ 또는 _UNSAVED_인 제거 리턴

* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`의 콜렉션

----
### saveOrUnsavePurgePath
제거 경로를 저장할지 여부를 표시하도록 제거 경로 항목의 상태를 업데이트합니다. 제거 경로가 저장되면 새 `saved` 제거를 작성합니다. 경로가 `unsaved`이면 저장된 제거 레코드를 삭제합니다.

* **매개변수**:
  * `uniqueId`: 제거가 속한 맵핑의 고유 ID
  * `path`: 저장하거나 저장하지 않을 제거의 경로
  * `saved`: _SAVED_ 또는 _UNSAVED_

* **리턴**: 유형 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`의 콜렉션

----
## TTL(Time to Live)에 대한 API
{: #api-for-time-to-live}

### TimeToLive 클래스 변수:  
```  
class SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive  
{
    /**
     * @var string
     */
    public $path;

    /**
     * @var int
     */
    public $timeToLive;

    /**
     * @var timestamp
     */
    public $createDate;
}
```  
### createTimeToLive
새 `TimeToLive` 오브젝트를 작성하고 데이터베이스에 이를 삽입합니다.

 * **매개변수**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`인 오브젝트
___
### updateTtl
기존 `TimeToLive` 오브젝트를 업데이트합니다. _oldTtl_ 및 _newTtl_ 입력이 동일하면 조기에 종료됩니다.

 * **매개변수**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`인 오브젝트
___
### deleteTtl
데이터베이스에서 기존 `TimeToLive` 오브젝트를 삭제합니다.

 * **매개변수**: `string` `uniqueId`, `string` `pathName`
 * **리턴**: 삭제 상태의 문자열
___
### listTtl
CDN의 `uniqueId`를 기반으로 한 기존 `TimeToLive` 오브젝트를 나열합니다.

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`인 오브젝트의 배열

 ----
## 메트릭에 대한 API
{: #api-for-metrics}

[메트릭 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-container-class-for-metrics)

### getCustomerUsageMetrics
지정된 기간 동안 고객 계정의 직접적인 표시(그래프 없음)를 위해 미리 결정된 통계의 총 수를 리턴합니다.

 * **매개변수**:
   * `vendorName`
   * `startDate`
   * `endDate`
   * `frequency`

 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingUsageMetrics
지정된 맵핑의 직접적인 표시를 위해 미리 결정된 통계의 총 수를 리턴합니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingHitsMetrics
지정된 시간 범위 동안 특정 빈도로 도메인 맵핑당 히트의 총 수를 리턴합니다. 빈도는 '일', '주' 및 '월'이 될 수 있습니다. 여기서, 그래프의 각 간격은 하나의 플롯 점입니다. 리턴 데이터는 `startDate`, `endDate` 및 `frequency`에 따라 정렬됩니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingHitsByTypeMetrics
지정된 시간 범위 동안 특정 빈도로 히트의 총 수를 리턴합니다. 빈도는 '일', '주' 및 '월'이 될 수 있습니다. 여기서, 그래프의 각 간격은 하나의 플롯 점입니다. 리턴 데이터는 `startDate`, `endDate` 및 `frequency`에 따라 정렬되어야 합니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingBandwidthMetrics
개별 CDN에 대한 에지 히트의 수를 리턴합니다. 영역은 각 벤더별로 다를 수 있습니다. 맵핑당입니다.

 * **매개변수**:  
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingBandwidthByRegionMetrics
지정된 기간 동안 지정된 맵핑의 직접적인 표시(그래프 없음)를 위해 미리 결정된 통계의 총 수를 리턴합니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**:
   * `mappingUniqueId`
   * `startDate`
   * `endDate`
   * `frequency`

 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션

----
## 지리적 액세스 제어를 위한 API
{: #api-for-geographical-access-control}

### createGeoblocking
새 지리적 액세스 제어 규칙을 작성하고 새로 작성된 규칙을 리턴합니다.

  * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
    여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

    [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

    다음 속성은 입력 컨테이너의 일부이며 새 지리적 액세스 제어 규칙을 작성할 때 **필요합니다**.
    * `uniqueId`: 규칙을 지정할 맵핑의 고유 ID
    * `accessType`: 규칙이 지정된 지역에 트래픽을 `ALLOW` 또는 `DENY`할지 지정합니다.
    * `regionType`: 지리적 액세스 제어 규칙을 적용할 지역의 유형(`CONTINENT` 또는 `COUNTRY_OR_REGION`)
    * `regions`: `accessType`을 적용할 위치를 나열하는 배열

      가능한 지역 목록을 확인하려면 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) 페이지를 참조하십시오.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 유형의 오브젝트

    [지역 차단 클래스 보기](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### updateGeoblocking
기존 도메인 맵핑에 대한 기존의 지리적 액세스 제어 규칙을 업데이트하고 업데이트된 규칙을 리턴합니다.

  * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
    여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

    [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

    다음 속성은 입력 컨테이너의 일부이며 지리적 액세스 제어 규칙을 업데이트할 때 제공될 수 있습니다(별도의 언급이 없으면 매개변수는 선택사항임).
    * `uniqueId`: 업데이트될 규칙이 속하는 맵핑의 **필수** 고유 ID
    * `accessType`: 규칙이 지정된 지역에 트래픽을 `ALLOW` 또는 `DENY`할지 지정합니다.
    * `regionType`: 규칙을 적용할 지역의 유형(`CONTINENT` 또는 `COUNTRY_OR_REGION`)
    * `regions`: `accessType`을 적용할 위치를 나열하는 배열

      가능한 지역 목록을 확인하려면 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`](/docs/infrastructure/CDN?topic=CDN-geoblocking-class) 페이지를 참조하십시오.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 유형의 오브젝트

    [지역 차단 클래스 보기](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### deleteGeoblocking
기존 도메인 맵핑에서 기존의 지리적 액세스 제어 규칙을 제거합니다.

  * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
    여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

    [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

    다음 속성은 입력 컨테이너의 일부이며 지리적 액세스 제어 규칙을 삭제할 때 **필요합니다**.
    * `uniqueId`: 삭제될 규칙이 속하는 맵핑의 고유 ID를 제공합니다.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 유형의 오브젝트

    [지역 차단 클래스 보기](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblocking
데이터베이스에서 맵핑의 지리적 액세스 제어 동작을 검색합니다.

  * **매개변수**:
    * `uniqueId`: 규칙이 속하는 맵핑의 고유 ID.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 유형의 오브젝트

    [지역 차단 클래스 보기](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)

----
### getGeoblockingAllowedTypesAndRegions
지리적 액세스 제어 규칙 작성에 허용되는 유형 및 지역의 목록을 리턴합니다.

  * **매개변수**:
    * `uniqueId`: 도메인 맵핑의 고유 ID.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking_Type` 유형의 오브젝트

    [지역 차단 클래스 보기](/docs/infrastructure/CDN?topic=CDN-geoblocking-class)
----
## 핫 링크 보호 API
{: #api-for-hotlink-protection}

### createHotlinkProtection
새 핫 링크 보호를 작성하고 새로 작성된 동작을 리턴합니다.

  * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
    여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

    [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

    다음 속성은 입력 컨테이너의 일부이며 새 핫 링크 보호를 작성할 때 **필요합니다**.
    * `uniqueId`: 동작을 지정할 맵핑의 고유 ID
    * `protectionType`: 웹 페이지에서 지정된 refererValues의 조건 중 하나와 일치하는 Referer 헤더 값이 있는 컨텐츠의 요청을 작성할 때 컨텐츠에 대한 액세스를 "허용"하는지 "거부"하는지 지정합니다.
    * `refererValues`: `protectionType` 동작의 영향을 받으며 단일 공백으로 분리된 Referer URL 일치 용어의 목록입니다.

      유효한 핫 링크 보호 값 목록을 확인하려면 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) 페이지를 참조하십시오.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 유형의 오브젝트

    [핫 링크 보호 클래스 보기](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### updateHotlinkProtection
기존 도메인 맵핑에 대한 기존의 핫 링크 보호 동작을 업데이트하고 업데이트된 동작을 리턴합니다.

  * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
    여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

    [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

    다음 속성은 입력 컨테이너의 일부이며 기존 핫 링크 보호를 업데이트할 때 **필요합니다**.
    * `uniqueId`: 기존 동작이 속한 맵핑의 고유 ID
    * `protectionType`: 웹 페이지에서 지정된 refererValues의 조건 중 하나와 일치하는 Referer 헤더 값이 있는 컨텐츠의 요청을 작성할 때 컨텐츠에 대한 액세스를 "허용"하는지 "거부"하는지 지정합니다. 
    * `refererValues`: `protectionType` 동작의 영향을 받으며 단일 공백으로 분리된 Referer URL 일치 용어의 목록입니다.

      유효한 핫 링크 보호 값 목록을 확인하려면 [`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) 페이지를 참조하십시오.

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 유형의 오브젝트

    [핫 링크 보호 클래스 보기](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)

----
### deleteHotlinkProtection
기존 도메인 맵핑에서 기존의 핫 링크 보호 동작을 제거합니다.

  * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션.
    여기 입력 컨테이너에서 모든 속성을 볼 수 있습니다.

    [입력 컨테이너 보기](/docs/infrastructure/CDN?topic=CDN-input-container)

    다음 속성은 입력 컨테이너의 일부이며 새 핫 링크 보호를 작성할 때 **필요합니다**.
    * `uniqueId`: 동작을 제거할 맵핑의 고유 ID

  * **리턴**: null

----
### getHotlinkProtection
맵핑의 현재 핫 링크 보호 동작을 검색합니다.

  * **매개변수**:
    * `uniqueId`: 동작이 속한 맵핑의 고유 ID

  * **리턴**: `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 유형의 오브젝트

    [핫 링크 보호 클래스 보기](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)
