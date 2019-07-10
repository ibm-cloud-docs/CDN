---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

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
{:DomainName: data-hd-keyref="DomainName"}

# 기능 설명
{: #feature-descriptions}

이 페이지에서는 Akamai에서 제공하는 {{site.data.keyword.cloud}} CDN에 포함된 여러 기능을 강조표시합니다.

## 호스트 서버 원본 지원
{: #host-server-origin-support}

IBM Cloud Content Delivery Network는 원본 호스트 이름, 프로토콜, 포트 번호 및 컨텐츠를 제공하는 데 사용할 경로(선택사항)를 제공하여 호스트 서버 원본에서 컨텐츠를 제공하도록 구성할 수 있습니다. 기본 경로는 `/*`입니다. 프로토콜은 HTTP, HTTPS 또는 둘 다입니다. Akamai에서는 특정 포트 번호만 지원됩니다. 지원되는 포트 번호/범위는 [FAQ](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.

## Object Storage 원본 지원
{: #object-storage-file-support}

IBM Cloud CDN은 엔드포인트, 버킷 이름, 프로토콜 및 포트를 제공하여 Object Storage 엔드포인트에서 컨텐츠를 제공하도록 구성할 수 있습니다. 선택적으로 파일 확장자 목록은 해당 확장자의 파일 캐싱만 허용하도록 지정할 수 있습니다. 버킷의 모든 오브젝트는 익명 읽기 또는 공용 읽기 액세스로 설정해야 합니다.

[CDN에서 IBM Cloud Object Storage 설정 방법](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn)에 대한 이 튜토리얼에서는 자세한 정보를 제공합니다.

## 개별 경로가 있는 다중 원본에 대한 지원
{: #support-for-multiple-origins-with-distinct-paths}

어떤 경우에는 다른 원본 서버의 특정 컨텐츠를 전달할 수 있습니다. 예를 들어, 다른 원본 서버에서 제공되는 특정 사진이나 동영상을 원할 수 있습니다. IBM Cloud CDN은 여러 경로의 다중 원본 서버를 설정하는 옵션을 제공합니다. 이를 통해 데이터를 저장하는 방법과 위치에 대한 유연성을 갖게 됩니다. 원본 서버에 제공되는 경로는 CDN에 대해 고유해야 합니다. 원본 서버 자체는 고유하지 않아도 됩니다.

## 경로 기반 CDN 맵핑
{: #path-based-cdn-mappings}

IBM Cloud CDN 서비스는 CDN 작성 시 경로를 제공하여 원본 서버의 특정 디렉토리 경로로 제한할 수 있습니다. 일반 사용자에게는 이 디렉토리 경로의 해당 컨텐츠에 대한 액세스만 허용됩니다. 예를 들어, `/videos` 경로를 사용하여 CDN `www.example.com`을 작성하면 `www.example.com/videos/*`를 통하는 **경우에만** 액세스할 수 있습니다.

## 캐시된 컨텐츠 제거
{: #purge-cached-content}

IBM Cloud CDN은 에지 서버의 캐시된 컨텐츠를 편리하고 신속하게 제거(remove)하거나 "제거(purge)"하는 기능을 제공합니다. 이 때 제거(purge)는 파일 경로에만 허용됩니다. 현재, Akamai를 사용한 구현의 제거(purge)를 통해 약 5초 안에 파일을 제거할 수 있습니다. 또한 UI는 제거(purge) 히스토리를 보고 특정 제거 경로를 즐겨찾기로 태그 지정하는 기능을 제공합니다.

## TTL(Time to Live)
{: #time-to-live-ttl}

TTL(Time To Live)은 에지 서버가 특정 파일 또는 디렉토리 경로에 대해 캐시된 컨텐츠를 유지하는 시간(초)을 표시합니다. CDN이 먼저 작성된 후 글로벌 TTL(Time To Live)이 경로(`/\*`)에 대해 작성되며 기본 시간은 3,600초입니다. TTL의 최소값은 0초이고 최대값은 2,147,483,647초입니다. 각 항목에 대해 TTL 경로는 CDN에 대해 고유해야 합니다. 여러 경로가 지정된 컨텐츠와 일치하면 최근에 구성된 경로 일치가 이 컨텐츠에 적용됩니다. 예를 들어, 두 개의 TTL, 즉 먼저 TTL 값 3,000초로 작성된 `/example/file`과 나중에 4,000초 값으로 작성된 `/example/*`에 대해 생각해보십시오. `/example/file`이 보다 구체적이지만 `/example/*`가 최근에 작성되었으므로 `/example/file`의 TTL은 4,000초가 됩니다. 일단 작성된 TTL 항목은 경로 및/또는 시간에 대해 편집할 수 있습니다. 이러한 TTL 항목 또한 삭제할 수 있습니다.

## 그래픽 보기가 있는 메트릭
{: #metrics-with-graphical-views}

개별 CDN의 메트릭은 이 CDN 맵핑에 대한 고객 포털의 개요 탭에 제공됩니다. CDN 사용에서 두 가지 유형의 메트릭이 계산됩니다(시간에 따른 메트릭을 그래프로 표현하는 경우 및 집계 값으로 표시되는 경우).

시간에 따른 변화를 그래프로 표시하는 메트릭의 경우 세 개의 선 그래프 및 하나의 원형 차트를 볼 수 있습니다. 세 개의 선 그래프는 **대역폭**, **맵핑별 히트 수** 및 **유형별 히트 수**입니다. 이러한 그래프에는 지정한 시간 범위 동안의 일별 활동이 표시됩니다. **대역폭** 및 **맵핑별 히트 수**가 단일 선 그래프인 반면, **유형별 히트 수**의 분류에서는 제공된 히트 유형마다 하나의 선을 표시합니다. 원형 차트에서는 CDN 맵핑의 대역폭에 대한 지역 분류를 백분율로 표시합니다.

집계 값에 대해 표시된 메트릭에는 **대역폭 사용량**(GB), CDN 에지 서버에 대한 **총 히트 수** 및 **히트 비율**이 포함됩니다. 히트 비율은 원본을 통하지 _않고_ 에지 서버에서 컨텐츠를 전달한 횟수(백분율)를 표시합니다. 히트 비율은 현재 표시되는 하나의 CDN 맵핑이 아닌 모든 CDN 맵핑의 함수로 표시됩니다.

기본적으로, 집계 수와 그래프는 둘 다 마지막 30일 동안의 메트릭을 표시하도록(기본값) 설정되지만 [고객 포털 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)을 통해 이를 변경할 수 있습니다. 두 카테고리는 7, 15, 30, 60 또는 90일 동안의 메트릭을 표시할 수 있습니다.

## 호스트 헤더 지원
{: #host-header-support}

에지 서버에서는 원본 호스트와 통신할 때 **호스트 헤더**를 사용합니다. 이 기능은 원본 호스트에 웹 서비스를 구성하는 방법에 대한 유연성을 제공합니다. 특히 클라이언트가 동일한 원본 호스트에 구성된 다중 웹 서버를 가지고 있는 유스 케이스를 사용합니다. 호스트 헤더 입력이 제공되지 않으면, 원본 서버가 IP 주소가 아닌 호스트 이름으로 지정된 경우 서비스가 원본 서버 호스트 이름을 기본 HTTP 호스트 헤더로 사용합니다. 호스트 헤더가 입력으로 제공되지 않고 원본 서버가 IP 주소로 제공되면 CDN 호스트 이름(CDN 도메인 이름이라고도 함)이 기본 HTTP 호스트 헤더로 사용됩니다.

## HTTPS 프로토콜 지원
{: #https-protocol-support}

CDN은 HTTPS 프로토콜을 사용하여 일반 사용자에게 컨텐츠를 안전하게 제공하기 위해 구성할 수 있습니다. 이 구성에서는 SSL 인증서를 CDN 구성의 일부로 설정해야 합니다. [와일드카드 인증서](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) 및 [도메인 유효성 검증(DV) SAN(Subject Alternative Name) 인증서](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport)와 같은 두 가지 유형의 SSL 인증서 옵션을 HTTPS에 사용할 수 있습니다. 이 문서에서는 이 유형을 _SAN 인증서_라고도 합니다.

사용할 SSL Certificate 유형은 HTTPS CDN에 중요한 고려사항입니다. 와일드카드 인증서 구성 설정이 빠르지만 CDN은 CNAME을 통해서만 액세스할 수 있다는 단점이 있습니다. SAN 인증서 프로세스는 완료하는 데 4 - 8시간이 걸리지만 CDN 도메인(즉, 호스트 이름)과 CDN을 사용하는 기능을 제공합니다. SAN 인증서를 사용하려면 구성 중에 [**도메인 제어 유효성 검증**](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san)의 추가 단계도 수행해야 합니다. 해당 인증서를 사용하는 데 비용이 들지 않습니다. 제공된 인증서 유형을 선택하는 경우 미치는 영향을 파악하려면 [문제점 해결 문서](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-)를 참조하십시오.

원본 호스트에는 CDN 호스트 이름의 고유 SSL 인증서도 있어야 하며 공인된 인증 기관(CA)에서 서명해야 합니다.

업계 우수 사례로, Akamai는 신뢰되는 중간 인증서 세트가 자주 변경되기 때문에 중간 인증서가 아닌 루트 인증서만 신뢰합니다. [웹의 이 위치에서](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates) Akamai 신뢰 인증서도 찾을 수 있습니다.

## 헤더 준수
{: #respect-headers}

**헤더 준수** 옵션을 사용하면 원본의 HTTP 헤더 구성이 CDN 구성을 대체할 수 있습니다. 이 기능은 기본적으로 켜져 있지만 끌 수 있습니다.

## 시간이 경과된(stale) 컨텐츠 제공
{: #serve-stale-content}

CDN 에지 서버가 사용자 요청을 수신하고 요청된 컨텐츠가 캐시되지 않으면, 에지 서버가 원본 호스트에 접속하여 컨텐츠를 페치합니다. 그러면 이 컨텐츠가 컨텐츠에 지정된 TTL(Time To Live) 지속 기간 동안 캐시됩니다. TTL이 만료된 후 사용자 요청이 수신되면, 에지 서버가 원본 호스트에 접속하여 컨텐츠를 페치합니다. 어떤 이유로(예: 원본 호스트가 작동 중지되거나 네트워크 문제가 있는 경우) 원본 서버에 연결할 수 없는 경우 에지 서버는 요청에 대해 만료된(시간이 경과된) 컨텐츠를 제공합니다. 이 기능은 Akamai에서 지원되며 끌 수 **없습니다**.

## 캐시 키 최적화
{: #cache-key-optimization}

Akamai 에지 서버는 **캐시 저장소**에 컨텐츠를 캐시합니다. **캐시 저장소**의 컨텐츠를 사용하기 위해 에지 서버에서 **캐시 키**를 사용합니다. 일반적으로 **캐시 키**는 일반 사용자의 URL 부분을 기반으로 생성됩니다. 어떤 경우에는 URL에 개별 사용자마다 다른 조회 함수 인수가 포함되지만 제공되는 컨텐츠는 동일합니다. 기본적으로 Akamai는 조회 함수의 인수를 사용하여 캐시 키를 생성하므로 각 사용자를 위한 고유 캐시 키를 생성합니다. 이로 인해 에지 서버가 이미 캐시된 컨텐츠에 대해 원본 서버에 접속하게 되므로(하지만 다른 캐시 키를 사용하여) 이 방법이 최선은 아닙니다. **캐시 키 최적화** 기능을 사용하면 캐시 키를 생성할 때 포함하거나 무시할 조회 인수를 지정할 수 있습니다. 이 기능은 CDN 맵핑 구성의 `create` 또는 `update` 및 원본 경로의 `create` 또는 `update`에 적용됩니다.

**캐시 키 최적화**의 값은 CDN 맵핑을 작성한 후에 **설정** 탭에서 구성할 수 있습니다. 원본 경로의 경우 원본 경로의 `create` 또는 `update` 오퍼레이션 중에 구성할 수 있습니다.
{: note}

## 컨텐츠 압축
{: #content-compression}

**컨텐츠 압축**은 기본적으로 Akamai CDN에서 다음 컨텐츠 유형에 대해 사용 가능합니다.
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

압축이 에지 서버에서 처리되는 경우 컨텐츠가 10KB 이상이어야 합니다.  어떤 경우에는 압축이 원본 서버에서 처리되며 이 때 압축할 파일 크기의 제한은 없습니다. 컨텐츠가 원본 서버에서 이미 압축된 경우에는 다시 압축되지 않습니다. 컨텐츠 압축을 사용하려면 요청 헤더가 `Accept-Encoding: gzip`을 정의해야 합니다.

## 대형 파일 최적화
{: #large-file-optimization}

대형 파일 최적화를 사용하면 CDN 네트워크에서 10MB를 넘는 컨텐츠의 전달을 최적화할 수 있습니다. 또한 대형 파일의 성능이 향상되고 대기 시간과 다운로드 시간이 줄어듭니다. 이 기능을 사용하지 않으면 CDN이 1.8GB를 넘는 파일을 제공할 수 없습니다. 이 기능을 사용하면 1.8GB보다 큰 파일(최대 320GB)을 다운로드할 수 있습니다.

대형 파일 최적화가 작동하려면 바이트 범위 요청이 원본 서버에서 사용 가능**해야 합니다**. Akamai CDN은 이 최적화를 위해 _부분 오브젝트 캐싱_이라고 하는 기술을 사용합니다. 대형 파일이 요청되면 에지 서버가 파일이 크기 요구사항을 충족하는지 확인합니다. 즉, 10MB보다 큰 파일이 원본 서버에서 청크로 요청됩니다. 청크가 에지 서버에 도달하면 캐시되어 바로 사용자에게 제공됩니다. 다음 청크는 에지 서버에서 동시에 미리 페치되었으므로 대기 시간이 줄어듭니다. 이 프로세스는 전체 파일이 검색되거나 연결이 종료될 때까지 계속됩니다.

이 기능을 사용하는 경우 10MB 미만의 컨텐츠 제공과 연관해서 성능이 저하된다는 단점이 있습니다. 따라서 대형 파일을 제공하는 경우에만 이 기능이 권장됩니다. 일반 유스 케이스는 CDN 구성에서 새 원본 경로를 작성하고 이 경로에 대형 파일 최적화를 사용하는 것입니다.

## VoD(Video on Demand)
{: #video-on-demand}

**VoD(Video on Demand)** 성능 최적화를 통해 다양한 네트워크 유형에서 고품질의 스트림이 제공됩니다. 사전 구성된 캐시 제어 설정 및 로드를 동적으로 분배하는 분산 네트워크의 기능을 활용하여 IBM Cloud CDN with Akamai는 사용자가 계획했는지 여부에 관계없이 많은 시청자에 맞게 신속하게 스케일링하는 기능을 제공합니다.

**VoD(Video on Demand)**는 HLS, DASH, HDS 및 HSS와 같은 세그먼트화된 스트리밍 형식의 분배를 위해 최적화되어 있습니다. 이 경우 실시간 동영상 스트리밍은 지원되지 **않습니다**. 설정 탭의 **최적화** 아래 드롭 다운 메뉴에서 옵션을 선택하거나 새 원본 경로를 작성할 때 **VoD(Video on Demand)**를 사용으로 설정할 수 있습니다. 동영상 파일의 전달을 최적화하는 경우에만 이 기능을 사용해야 합니다.

## 지리적 액세스 제어
{: #geographical-access-control}

지리적 액세스 제어는 지리적 위치를 기반으로 사용자의 그룹에 대한 `access-type` 매개변수를 설정할 수 있는 규칙 기반 동작입니다. **허용** 및 **거부** 두 가지 유형의 동작이 사용 가능합니다.

액세스 유형 `Allow`를 통해 지역 유형을 기반으로 선택한 지역에 트래픽을 특별히 허용할 수 있습니다. 특정 지역에 대해 트래픽을 허용하면 암묵적으로 다른 모든 트래픽은 차단합니다. 예를 들어, 유럽 및 오세아니아 등의 선택한 대륙에 트래픽을 `Allow`하도록 선택할 수 있으며 이는 다른 모든 대륙에 대한 액세스를 차단합니다.

반면에 `Deny` 동작은 지정된 그룹의 서비스에 대한 액세스를 차단하지만 지정되지 않은 다른 모든 지역에 대한 액세스는 허용됩니다. 예를 들어, 지리적 액세스 제어의 액세스 유형을 유럽 및 오세아니아 대륙에 대해 `Deny`로 설정하는 경우, 이 대륙의 사용자는 서비스를 사용할 수 **없습니다**. 반면에 다른 모든 대륙의 사용자는 서비스에 대한 액세스를 보유합니다.

이 기능은 CDN 구성의 **설정** 페이지에서 액세스할 수 있습니다.

## 핫 링크 보호
{: #hotlink-protection}

핫 링크 보호는 특정 웹 사이트가 사용자의 CDN에서 컨텐츠에 액세스하도록 허용되는지 여부를 제어할 수 있게 해주는 규칙 기반의 동작입니다. 일반적으로 브라우저는 웹 페이지의 링크에서 HTTP 요청이 작성되고 해당 링크가 원격 자산을 지정할 때 `Referer` 헤더를 포함합니다. 한 웹 사이트가 다른 웹 사이트에서 자산에 액세스하기 위해 사용하는 링크를 _핫 링크_라고 합니다. **허용** 및 **거부** 두 가지 유형의 동작이 사용 가능합니다.

`protectionType`가 `ALLOW`로 설정된 경우:
* CDN에 전송된 요청 내의 `Referer` 헤더 값이 지정된 `refererValues` 중 하나와 일치하면 CDN이 요청된 컨텐츠를 **제공합니다**.
* 그렇지 않으면 CDN이 컨텐츠를 제공하지 **않습니다**.

`protectionType`가 `DENY`로 설정된 경우:
* CDN에 전송된 요청 내의 `Referer` 헤더 값이 지정된 `refererValues` 중 하나와 일치하면 CDN이 요청된 컨텐츠를 **제공하지 않습니다**.
* 그렇지 않으면 CDN이 컨텐츠를 제공합니다.
