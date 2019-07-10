---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 입력 컨테이너
{: #input-container}

입력 컨테이너는 맵핑과 (원본) 경로 클래스에서 이용하는 콜렉션입니다. 두 클래스에 대한 입력 속성의 일관된 세트를 제공합니다.

* `vendorName`: 올바른 {{site.data.keyword.cloud}} CDN 제공자의 이름입니다.
* `oldPath`: updateOriginPath()에서 사용됩니다. 이 특성은 현재 또는 '이전' 경로의 이름을 저장합니다.

다음 속성은 맵핑 및 (원본) 경로 클래스에 공통됩니다.
* `originType`: 원본 호스트의 유형(현재 'HOST_SERVER' 또는 'OBJECT_STORAGE')입니다.
* `origin`: 원본 서버 주소(원본 서버의 호스트 이름 또는 IPv4 주소), 컨텐츠를 페치할 엔드포인트 또는 컨텐츠가 저장되는 버킷의 이름입니다. 511자 미만이어야 합니다.
* `httpPort`: HTTP 프로토콜에 사용되는 포트의 번호입니다. Akamai에는 HTTP 및 HTTPS 포트의 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호 목록은 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
* `httpsPort`: HTTPS 프로토콜에 사용되는 포트의 번호입니다. Akamai에는 HTTP 및 HTTPS 포트의 포트 번호에 대한 특정 제한사항이 있습니다. 허용되는 포트 번호 목록은 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.
* `status`:  맵핑 또는 경로의 상태입니다. 상태는 CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED 또는 ERROR입니다.
* `path`: 캐시된 컨텐츠를 제공하는 데 사용할 경로입니다. 기본 경로는 /\*이며 `updateOriginPath` API에서 사용될 때 이 속성은 추가할 새 경로를 참조합니다.
* `performanceConfiguration`: 맵핑의 성능 구성에 대한 스펙입니다.
* `cacheKeyQueryRule`: 다음 옵션은 캐시 키 동작을 구성하는 데 사용 가능합니다.
  * `include-all`: 모든 조회 인수를 포함합니다.
  * `ignore-all`: 모든 조회 인수를 무시합니다.
  * `ignore: space separated query-args`: 특정 조회 인수를 무시합니다. 예를 들어, `ignore: query1 query2`입니다.
  * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, `include: query1 query2`입니다.
* `geoblockingRule`

다음 속성은 맵핑 클래스에 한정됩니다.

* `uniqueId`: 각 맵핑에 대해 고유한 10자리의 시스템 생성 ID입니다. 맵핑이 작성될 때 생성됩니다.
* `domain`: 기본 CDN 이름입니다. 또한 호스트 이름이라고도 합니다.
* `protocol`: 서비스 설정에 사용되는 프로토콜입니다. HTTP, HTTPS 또는 이 두 가지의 조합(HTTP_AND_HTTPS)입니다.
* `cname`: 호스트 이름에 별명을 지정하는 표준 이름 레코드입니다. 사용자가 제공하거나 시스템에서 생성합니다. 사용자가 제공하는 경우 고유한 64자 미만의 영숫자 문자여야 합니다. 제공하지 않으면 맵핑 작성 시 생성됩니다.
* `certificateType`: 맵핑에서 사용하는 인증서의 유형입니다. `WILDCARD_CERT` 또는 `SHARED_SAN_CERT`일 수 있습니다. HTTP 맵핑의 경우 값은 0이 됩니다.
* `respectHeaders`: 'true'로 설정된 경우 원본의 TTL 설정이 CDN TTL 설정을 대체하게 하는 부울 값입니다.
* `header`: 원본에서 사용되는 호스트 헤더 정보를 지정합니다.

다음 속성은 COS(Cloud Object Storage)와 관련되어 있습니다.  
* `bucketName`: S3 Object Storage에 대한 버킷의 고유 이름입니다.  
* `fileExtension`: 허용되는 파일 확장자입니다.

다음 속성이 핫 링크 보호 구성과 관련됩니다.
* `hotlinkProtection`: 세부사항은 [핫 링크 보호 클래스](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class)를 참조하십시오.
