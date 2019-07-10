---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: mapping, container, class, collection, object, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# 맵핑 컨테이너
{: #mapping-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping` 콜렉션에는 맵핑 API에서 이용하는 속성이 포함됩니다. 각 맵핑 API는 이 유형의 콜렉션을 리턴합니다.

**클래스** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`:

* `vendorName`: 올바른 {{site.data.keyword.cloud}} CDN 제공자의 이름입니다.
* `uniqueId`: 각 맵핑에 대해 고유한 10자리의 시스템 생성 ID입니다. 맵핑이 작성될 때 생성됩니다.
* `domain`: 기본 CDN 이름입니다. `호스트 이름`라고도 합니다.
* `protocol`: 서비스 설정에 사용되는 프로토콜입니다. HTTP, HTTPS 또는 이 두 가지의 조합(HTTP_AND_HTTPS)입니다.
* `originType`: 원본 호스트의 유형(현재 'HOST_SERVER' 또는 'OBJECT_STORAGE')입니다.
* `originHost`: 컨텐츠를 페치할 엔드포인트인 원본 서버 주소(원본 서버의 호스트 이름 또는 IPv4 주소) 또는 컨텐츠가 저장되는 버킷의 이름입니다. 511자 미만이어야 합니다.
* `httpPort`: HTTP 프로토콜에 사용되는 포트의 번호입니다. Akamai에는 HTTP 및 HTTPS 포트의 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호는 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
* `httpsPort`: HTTPS 프로토콜에 사용되는 포트의 번호입니다. Akamai에는 HTTP 및 HTTPS 포트의 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호는 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
* `certificateType`: 맵핑에서 사용하는 인증서의 유형입니다. `WILDCARD_CERT` 또는 `SHARED_SAN_CERT`일 수 있습니다. HTTP 맵핑의 경우 값은 0이 됩니다.
* `cname`: 호스트 이름에 별명을 지정하는 표준 이름 레코드입니다. 사용자가 제공하거나 시스템에서 생성할 수 있습니다. 사용자가 제공하는 경우 고유한 64자 미만의 영숫자 문자여야 합니다. 제공하지 않으면 맵핑 작성 시 생성됩니다.
* `respectHeaders`: 'true'로 설정된 경우 원본의 TTL 설정이 CDN TTL 설정을 대체하게 하는 부울 값입니다.
* `header`: 원본 서버에서 사용되는 호스트 헤더 정보를 지정합니다.
* `status`: 맵핑의 상태입니다. 상태는 CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED 또는 ERROR입니다.
* `bucketName`: S3 Object Storage의 버킷 이름입니다.
* `fileExtension`: 캐시할 수 있는 파일 확장자입니다.
* `path`: S3 Object Storage에 대한 경로입니다. 대개 서비스의 오브젝트 저장소 URL 또는 API 섹션에 있습니다.
* `cacheKeyQueryRule`: 다음 옵션은 캐시 키 동작을 구성하는 데 사용 가능합니다.
  * `include-all`: 모든 조회 인수를 포함합니다. **기본값**
  * `ignore-all`: 모든 조회 인수를 무시합니다.
  * `ignore: space separated query-args`: 특정 조회 인수를 무시합니다. 예를 들어, "ignore: query1 query2"입니다.
  * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, "include: query1 query2"입니다.

특히 주목할 것은 `uniqueId`이며, 맵핑이 작성될 때 생성되어 후속 API 호출에 대한 매개변수로 사용된다는 점입니다.

예를 들어, `listDomainMappingByUniqueid`에 대한 다음 호출을 통해  
```php  
$cdnMapping = $client->listDomainMappingByUniqueid('750352919747xxx');  
```

다음과 유사한 오브젝트가 리턴됩니다.

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
            [cacheKeyQueryRule] => include-all
            [certificateType] => NO_CERT
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
            [status] => RUNNING
            [uniqueId] => 750352919747xxx
            [vendorName] => akamai
        )

```
{: codeblock}

`stopDomainMapping` 및 `startDomainMapping`의 호출에서 동일한 맵핑 오브젝트를 리턴하지만 `status`는 다릅니다.

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => STOPPED
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}

```php  
[0] => SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping Object
                (
          ...

            [status] => RUNNING
            [uniqueId] => 750352919747xxx

          ...
        )

```
{: codeblock}
