---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# FAQ
{: #faqs}

## Content Delivery Network는 무엇입니까?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Content Delivery Network는 국가 또는 전세계의 여러 지역을 통해 분산되는 에지 서버의 콜렉션입니다. 웹 컨텐츠는 컨텐츠를 요청하는 고객과 가장 가까운 지역에 위치하는 에지 서버에서 제공됩니다. 이 기술을 통해 사용자는 하나의 중앙 위치에서 컨텐츠를 전달하여 수행하는 것 보다 지연 시간을 줄여 컨텐츠를 수신할 수 있습니다. 고객을 위해 개선된 전체 경험을 전달합니다.

## Content Delivery Network는 어떻게 작동합니까?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

CDN은 전세계의 에지 서버에서 웹 컨텐츠를 캐싱하여 목적을 달성합니다. 사용자가 웹 컨텐츠를 요청하는 경우 컨텐츠 요청은 사용자와 지리적으로 가장 가까운 에지 서버로 라우팅됩니다. CDN은 컨텐츠가 이동해야 하는 거리를 줄여 최적화된 처리량, 최소화된 대기 시간 및 향상된 성능을 제공합니다.

## IBM Cloud Content Delivery Network 서비스 계정을 작성하는 방법은 무엇입니까?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

CDN 주문 프로세스 중에 계정이 작성됩니다. 레거시 포털에서 CDN을 작성 중인 경우 **네트워크 -> CDN 페이지** 아래의 **CDN 주문** 단추를 클릭하면 계정이 작성됩니다. {{site.data.keyword.cloud}} 포털에서 CDN을 작성 중인 경우 **카탈로그 -> 네트워크 -> Content Delivery Network** 페이지 아래의 **작성** 단추를 클릭하면 계정이 작성됩니다.

## 내 CDN 상태가 CNAME 구성인 경우 수행해야 할 작업은 무엇입니까?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

HTTPS CDN 기반 SAN 인증서 및 HTTP의 경우, 웹 사이트가 새 CDN 맵핑과 연관된 `CNAME`을 가리키도록 DNS 레코드를 업데이트하십시오. HTTPS CDN 기반 와일드카드 인증서의 경우에는 이 DNS 업데이트가 필요하지 **않습니다**.

**참고**: 활성 상태가 되도록 업데이트하는 데 15 - 30분이 소요될 수 있습니다. 정확한 예상 시간을 알아 보려면 DNS 제공자에게 문의하십시오.

## DNS 내의 내 CDN 도메인에 대한 CNAME 레코드를 추가하는 방법은 무엇입니까?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

CDN 도메인에 대한 DNS 구성 페이지에서 호스트로서 CDN 도메인 이름으로 CNAME 레코드를 작성하고 CNAME으로 CDN을 구성하는 데 사용한 IBM CNAME을 작성할 수 있습니다. IBM CNAME은 `cdnedge.bluemix.net`으로 끝납니다.

일반 CNAME 레코드는 DNS 구성 페이지에 다음과 같이 표시됩니다.

| **리소스 유형** | **호스트** | **지정 대상 (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
|CNAME | www.example.com | example.cdnedge.bluemix.net | 15분 |


## 내 CDN에서 비용이 청구되는 대상은 무엇입니까?
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

IBM Cloud Content Delivery Network 인스턴스마다 사용되는 대역폭에 대해서만 비용이 청구됩니다. CDN에서 대역폭을 사용하지 않은 경우 금액이 청구되지 않습니다. 대역폭 비용은 에지 서버의 지리적 위치에 따라 달라집니다. 이 서비스에 대한 [가격 문서](/docs/infrastructure/CDN?topic=CDN-pricing)에서 지역별 대역폭 가격을 볼 수 있습니다.

## 언제 내 CDN에 대해 청구됩니까?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

{{site.data.keyword.BluSoftlayer_notm}} 계정에 설정된 청구 기간에 따라 IBM Cloud Content Delivery Network 비용이 청구됩니다.

## CDN의 오버플로우 메뉴에서 `delete`를 선택한 경우 내 계정이 삭제됩니까?
{: faq}

아니요. 해당 CDN만 삭제됩니다. 사용자의 계정은 계속 유지되며 추가 CDN을 작성할 수 있습니다.

## 컨텐츠 캐싱에서 푸시 또는 풀을 사용합니까?
{: #does-content-caching-use-push-or-pull}
{: faq}

컨텐츠 캐싱은 _원본 풀_ 모델을 사용하여 수행됩니다. 원본 풀은 컨텐츠를 에지 서버에 수동으로 업로드하는 것과는 반대로 데이터가 원본 서버에서 에지 서버로 "풀링"되는 메소드입니다.

## CDN 서비스 구성에 사용할 권장되는 브라우저가 있습니까?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

예, Firefox 및 Chrome 브라우저가 권장됩니다. IBM Cloud Content Delivery Network에 대한 최신 버전을 사용하는 것이 좋습니다.

## 내 CDN을 작성할 때 경로를 제공하는 목적은 무엇입니까?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

CDN을 작성하는 중에 경로를 제공하면 특정 원본 서버에서 CDN을 통해 제공할 수 있는 파일을 격리할 수 있습니다.

## 내 CDN은 오류 상태에 있습니다. 지금 어떠한 작업을 수행해야 합니까?
{: faq}

[문제점 해결](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) 또는 [도움 및 지원 받기](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp) 페이지를 참조하거나 [고객 포털 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에서 티켓을 여십시오.

## 제공하지 않은 경우 내 CDN에 대한 CNAME을 어디서 찾을 수 있습니까?
{: faq}

CDN을 클릭하여 포털의 **개요** 페이지에 액세스하십시오. 오른쪽 모서리에서 `CName` 정보가 있는 **세부사항** 섹션을 볼 수 있습니다.

## 제공된 파일 경로에 대한 내 제거 요청이 진행 중입니다. 동일한 파일 경로로 새 요청을 제출할 수 있습니까?

아니요. 제공된 파일 경로로 한 번에 하나의 활성 제거 요청만 가능합니다.

## IPv6(Internet Protocol version 6)이 IBM Cloud Content Delivery Network 서비스와 함께 지원됩니까? 어떻게 작동합니까?
{: faq}

IPv6(또는 이중 스택 지원)은 Akamai의 에지 서버에서 지원됩니다. IPv4 전용 원본의 고객이 IPv6 클라이언트에서 연결을 허용하는 것을 돕도록 디자인되었으며, 에지에서 IPv6에서 IPv4로 변환하고 IPv4로 원본으로 이동하십시오.

**참고:** 원본 서버 주소로 IPv6 주소를 사용하여 IBM Cloud CDN을 작성하는 것은 지원되지 않습니다.

## Akamai에 허용되는 HTTP 및 HTTPS 포트 번호에 대한 제한사항이 있습니까?
{: faq}

예. Akamai 벤더에 대해 다음 포트 번호만 허용됩니다.
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 및 45002.

## CDN 또는 원본 경로 아래 데이터에 액세스하기 위해 어떤 URL을 사용해야 합니까?
{: faq}

CDN 맵핑 또는 원본에 대한 경로가 디렉토리로 간주됩니다. 따라서 원본 경로에 액세스하려는 사용자는 디렉토리(슬래시 포함)로서 원본 경로에 액세스해야 합니다. 예를 들어, `/images` 디렉토리가 포함된 경로를 사용하여 CDN `www.example.com`이 작성되는 경우, 접속할 URL은 `www.example.com/images/`여야 합니다.

슬래시를 생략(예: `www.example.com/images` 사용)하면 **페이지를 찾을 수 없음** 오류가 발생합니다.

## IBM COS(Cloud Object Storage)의 내 Content Delivery Network를 설정하는 방법은 무엇입니까?
{: faq}

IBM Cloud Object Storage의 Content Delivery Network 작성 방법에 대한 [튜토리얼](/docs/tutorials?topic=solution-tutorials-static-files-cdn)입니다.

## 내 원본 인증서가 만료된다는 알림을 수신했습니다. 지금 어떠한 작업을 수행해야 합니까?
{: faq}

Akami의 [이 기사 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://community.akamai.com/docs/DOC-7708)에 설명된 단계를 수행하십시오.

## Akamai를 사용한 IBM CDN 솔루션에 포함되는 보안은 무엇입니까?
{: faq}

분산 Akamai 플랫폼을 사용하면 50개국 이상에서 수천 개의 서버를 통해 엄청난 규모와 복원력을 갖게 됩니다. Akamai 플랫폼은 인프라와 일반 사용자 사이에서 트래픽의 갑작스런 증가에 대한 첫 번째 레벨의 방어 수단이 됩니다. 또한 Akamai Intelligent Platform은 포트 80과 443에서만 요청을 청취하고 응답하는 리버스 프록시이며, 이는 다른 포트의 트래픽이 인프라로 전송되지 않고 에지에서 삭제된다는 것을 의미합니다.

## 원본 서버의 쿠키는 Akamai CDN에서 유지됩니까?
{: faq}

캐시 가능하지 않거나 캐시되지 않는 컨텐츠의 경우 쿠키는 원본 서버에서 유지됩니다. 에지 서버에서 캐시하는 컨텐츠의 경우 쿠키는 유지되지 않습니다.

## IBM Cloud 콘솔을 사용하여 다른 사용자에게 CDN을 작성하거나 관리하는 권한을 부여하려면 어떻게 해야 합니까?
{: faq}

계정의 마스터 사용자는 다른 사용자에게 CDN을 작성하고 관리하는 권한을 제공할 수 있습니다.

IBM Cloud 콘솔 메인 페이지에서, 다음 단계에 따라 권한을 편집하십시오.
 * **관리** 탭을 선택합니다.
 * **액세스(IAM)**를 선택합니다.
 * 왼쪽 분할창에서 **사용자** 탭을 클릭합니다.
 * 원하는 **사용자**를 클릭합니다.
 * 그런 다음 **클래식 인프라** 탭을 선택합니다.
 * **권한** 탭에서 **서비스** 카테고리를 펼칩니다.
 * **CDN 계정 관리**를 선택하십시오.
 * **저장** 단추를 클릭합니다.

레거시 콘솔 메인 페이지에서, 다음 단계에 따라 권한을 편집하십시오.
 * **계정** 탭을 선택하십시오.
 * **사용자 -> 사용자 목록**을 선택하십시오.
 * 원하는 **사용자 이름**을 클릭하십시오.
 * 그런 다음 **포털 권한** 탭을 선택하십시오.
 * **서비스** 탭을 선택하십시오.
 * **CDN 계정 관리**를 선택하십시오.
 * **포털 권한 편집** 단추를 클릭하십시오.

## https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai 페이지에서 작성 단추가 표시되지 않거나 비활성화되는 이유
{: faq}

계정의 마스터 사용자인 경우, 이 페이지에 작성 단추를 표시하거나 활성화하려면 계정을 업그레이드해야 합니다. IBM Cloud 콘솔 페이지에서 계정의 마스터 사용자로서 다음 단계를 수행하십시오.
 * 웹 페이지 왼쪽 상단의 `3줄` 아이콘을 클릭하여 왼쪽 탐색 창을 엽니다.
 * **클래식 인프라**를 선택합니다.
 * **계정 업그레이드** 단추를 클릭하고 지시사항을 따릅니다.

계정의 보조 사용자인 경우, 이 페이지에 작성 단추를 표시하거나 활성화하려면 계정의 마스터 사용자가 `서비스 추가/업그레이드` 권한을 부여해야 합니다. IBM Cloud 콘솔 페이지에서 계정의 마스터 사용자가 다음 단계에 따라 권한을 편집해야 합니다.
 * **관리** 탭을 선택합니다.
 * **액세스(IAM)**를 선택합니다.
 * 왼쪽 분할창에서 **사용자** 탭을 클릭합니다.
 * 원하는 **사용자**를 클릭합니다.
 * 그런 다음 **클래식 인프라** 탭을 선택합니다.
 * **권한** 탭에서 **계정** 카테고리를 펼칩니다.
 * **서비스 추가/업그레이드**를 선택합니다.
 * **저장** 단추를 클릭합니다.

## `protectionType` `ALLOW`를 사용하여 핫 링크 보호를 구성한 후에 내 CDN을 통해 웹 페이지에 도달할 수 없는 이유는 무엇입니까?
{: faq}

일반 사용자용 웹 사이트 도메인이 CDN의 도메인/호스트 이름이 되도록 구성한 경우를 고려해 보십시오. `cdn.example.com`. 사용자가 웹 페이지의 탐색줄에서 직접 탐색하여 웹 페이지에 도달하려고 할 때 브라우저는 일반적으로 해당 HTTP 요청에서 Referer 헤더를 전송하지 않습니다. 예를 들어, 이런 방법으로 직접 `https://cdn.example.com/`을 탐색하는 경우, CDN이 요청에 지정된 `refererValues`와의 불일치가 포함된 것으로 간주합니다. CDN이 핫 링크 보호를 통해 적절한 영향 또는 응답을 평가할 때 불일치가 발생한 것으로 판단합니다. 따라서 CDN이 액세스를 '허용'하는 대신 거부합니다.

## CDN 설정에서 Object Storage의 개인용 엔드포인트를 사용할 수 있습니까?
{: faq}

아니요. CDN은 공용 엔드포인트의 Object Storage에만 연결할 수 있습니다.

## CDN 서비스에서 Brotli 기능을 사용할 수 있습니까?
{: faq}

아니요. Akamai를 사용하는 IBM CDN 서비스에는 Brotli 기능이 지원되지 않습니다.

## 도메인을 사용하지 않고 CDN 엔드포인트를 작성하는 방법은 무엇입니까?
{: faq}

도메인을 사용하지 않고 CDN 엔드포인트를 작성할 수 있지만 **와일드카드 HTTPS** 유형의 CDN의 경우에만 가능합니다. **와일드카드 HTTPS** 유형의 CDN을 작성하는 동안 **CNAME**이 CDN 엔드포인트 역할을 하고 **CNAME**이 트래픽을 제공하는 데 사용됩니다.
