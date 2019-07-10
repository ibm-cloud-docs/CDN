---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: content delivery network, web content, caching, edge servers, streaming content

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Content Delivery Network 정보
{: #about-content-delivery-networks-cdn-}

Content Delivery Network는 국가 또는 전세계의 여러 지역을 통해 분산되는 에지 서버의 콜렉션입니다. 웹 컨텐츠는 컨텐츠를 요청하는 고객과 가장 가까운 지역에 위치하는 에지 서버에서 제공됩니다. 이 기술을 사용하면 일반 사용자가 컨텐츠를 수신할 때 발생하는 지연을 단축할 수 있고 고객에게 향상된 경험을 제공할 수 있습니다.

## CDN은 어떻게 작동합니까?
{: #how-does-a-cdn-work}

CDN은 전세계의 에지 서버에서 웹 컨텐츠를 캐싱하여 목적을 달성합니다. 고객이 웹 컨텐츠를 요청하는 경우 컨텐츠 요청은 고객과 지리적으로 가장 가까운 에지 서버로 라우팅됩니다. CDN은 컨텐츠가 이동해야 하는 거리를 줄여 최적화된 처리량, 최소화된 대기 시간 및 향상된 성능을 제공합니다. {{site.data.keyword.cloud}} Content Delivery Network with Akamai를 사용하면 컨텐츠 제공자는 최소한의 구성으로 전세계에서 요청된 컨텐츠를 효율적으로 전달할 수 있습니다.

![상위 레벨 CDN 다이어그램](images/high-level-cdn-diagram.png)

## 기능
{: #features}

{{site.data.keyword.cloud}} Content Delivery Network 서비스의 주요 기능은 다음과 같습니다.
  * 호스트 서버 원본 지원
  * Object Storage 원본 지원
  * 개별 경로가 있는 다중 원본에 대한 지원
  * 경로 기반 CDN 맵핑
  * 캐시된 컨텐츠 제거
  * TTL(Time to Live)
  * 그래픽 보기가 있는 메트릭
  * 와일드카드와 DV SAN 인증서를 사용하는 HTTPS 프로토콜 지원
  * 호스트 헤더 지원
  * 시간이 경과된(stale) 컨텐츠 제공
  * 캐시 키 조회 인수
  * 컨텐츠 압축
  * 대형 파일 최적화
  * VoD(Video on Demand)
  * 지리적 액세스 제어
  * 핫 링크 보호

전체 기능 설명은 [기능 설명 문서](/docs/infrastructure/CDN?topic=CDN-feature-descriptions)를 참조하십시오.

## 아키텍처 다이어그램
{: #architectural-diagram}

다음 다이어그램은 IBM Cloud CDN의 3계층 아키텍처에 대한 도식화된 개요를 제공합니다.

![아키텍처 다이어그램](images/3-tier-architecture.png)
