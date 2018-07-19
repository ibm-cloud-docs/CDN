---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# FAQ

## Content Delivery Network는 무엇입니까?

Content Delivery Network는 국가 또는 전세계의 여러 지역을 통해 분산되는 에지 서버의 콜렉션입니다. 웹 컨텐츠는 컨텐츠를 요청하는 고객과 가장 가까운 지역에 위치하는 에지 서버에서 제공됩니다. 이 기술을 통해 사용자는 하나의 중앙 위치에서 컨텐츠를 전달하여 수행하는 것 보다 지연 시간을 줄여 컨텐츠를 수신할 수 있습니다. 고객을 위해 개선된 전체 경험을 전달합니다.

## Content Delivery Network는 어떻게 작동합니까?

CDN은 전세계의 에지 서버에서 웹 컨텐츠를 캐싱하여 목적을 달성합니다. 사용자가 웹 컨텐츠를 요청하는 경우 컨텐츠 요청은 사용자와 지리적으로 가장 가까운 에지 서버로 라우팅됩니다. CDN은 컨텐츠가 이동해야 하는 거리를 줄여 최적화된 처리량, 최소화된 대기 시간 및 향상된 성능을 제공합니다.

## IBM Cloud Content Delivery Network 서비스 계정을 작성하는 방법은 무엇입니까?

"벤더 선택사항" 메뉴를 거친 후 **선택**을 클릭하면 CDN 주문 프로세스 중에 사용자 계정이 작성됩니다.

## 내 CDN의 상태가 CNAME_CONFIGURATION인 경우 수행해야 할 작업은 무엇입니까?

HTTP 기반 CDN의 경우 웹 사이트가 새 CDN 맵핑과 연관된 `CNAME`을 지정하도록 DNS 레코드를 업데이트하십시오. HTTPS 기반 CDN의 경우에는 이 DNS 업데이트가 필요하지 **않습니다**.

**참고**: 활성 상태가 되도록 업데이트하는 데 15 - 30분이 소요될 수 있습니다. 정확한 예상 시간을 알아 보려면 DNS 제공자에게 문의하십시오.

## 청구 항목은 무엇입니까?

IBM Cloud Content Delivery Network 인스턴스마다 사용되는 대역폭에 대해서만 비용이 청구됩니다. CDN에서 대역폭을 사용하지 않은 경우 금액이 청구되지 않습니다. 대역폭 비용은 에지 서버의 지리적 위치에 따라 달라집니다. 이 서비스에 대한 [Getting Started](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-) 섹션에서 지역별 대역폭 가격 책정을 볼 수 있습니다.

## 청구 시기는 언제입니까?

{{site.data.keyword.BluSoftlayer_notm}} 계정에 설정된 청구 기간에 따라 IBM Cloud Content Delivery Network 비용 청구가 발생합니다.

## CDN의 오버플로우 메뉴에서 `delete`를 선택한 경우 내 계정이 삭제됩니까?

아니오. 해당 CDN만 삭제됩니다. 사용자의 계정은 계속 유지되며 추가 CDN을 작성할 수 있습니다.

## 캐싱에서 푸시 또는 풀을 사용합니까?

컨텐츠 캐싱은 _원본 풀_ 모델을 사용하여 수행됩니다. 원본 풀은 컨텐츠를 에지 서버에 수동으로 업로드하는 것과는 반대로 데이터가 원본 서버에서 에지 서버로 "풀링"되는 메소드입니다.

## CDN 서비스 구성에 사용할 권장되는 브라우저가 있습니까?

예, Firefox 및 Chrome 브라우저가 권장됩니다. IBM Cloud Content Delivery Network에 대한 최신 버전을 사용하는 것이 좋습니다.

## 내 CDN을 작성할 때 경로를 제공하는 목적은 무엇입니까?

CDN을 작성하는 중에 경로를 제공하면 특정 원본 서버에서 CDN을 통해 제공할 수 있는 파일을 격리할 수 있습니다.

## 내 CDN은 오류 상태에 있습니다. 지금 어떠한 작업을 수행해야 합니까?

[문제점 해결](troubleshooting.html#troubleshooting) 또는 [도움 및 지원 받기](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#getting-help) 페이지를 참조하거나 [고객 포털 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에서 티켓을 여십시오.

## 내 CNAME을 제공하지 않은 경우 어디서 찾을 수 있습니까?

CDN을 클릭하여 포털의 **개요** 페이지에 액세스하십시오. 오른쪽 모서리에서 `CName` 정보가 있는 **세부사항** 섹션을 볼 수 있습니다.

## 제공된 파일 경로에 대한 내 제거 요청이 진행 중입니다. 동일한 파일 경로로 새 요청을 제출할 수 있습니까?

아니오. 제공된 파일 경로로 한 번에 하나의 활성 제거 요청만 가능합니다.

## IPv6(Internet Protocol version 6)이 IBM Cloud Content Delivery Network 서비스와 함께 지원됩니까? 어떻게 작동합니까?

IPv6(또는 이중 스택 지원)은 Akamai의 에지 서버에서 지원됩니다. IPv4 전용 원본의 고객이 IPv6 클라이언트에서 연결을 허용하는 것을 돕도록 디자인되었으며, 에지에서 IPv6에서 IPv4로 변환하고 IPv4로 원본으로 이동하십시오.

**참고:** 원본 서버 주소로 IPv6 주소를 사용하여 IBM Cloud CDN을 작성하는 것은 지원되지 않습니다.

## Akamai에 허용되는 HTTP 및 HTTPS 포트 번호에 대한 제한사항이 있습니까?

예. Akamai 벤더에 대해 다음 포트 번호만 허용됩니다.
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 및 45002.

## CDN 또는 원본 경로 아래 데이터에 액세스하기 위해 어떤 URL을 사용해야 합니까?
CDN 맵핑 또는 원본에 대한 경로가 디렉토리로 간주됩니다. 따라서 원본 경로에 액세스하려는 사용자는 디렉토리(슬래시 포함)로서 원본 경로에 액세스해야 합니다. 예를 들어, `/images` 디렉토리가 포함된 경로를 사용하여 CDN `www.example.com`이 작성되는 경우, 접속할 URL은 `www.example.com/images/`여야 합니다.

슬래시를 생략(예: `www.example.com/images` 사용)하면 **페이지를 찾을 수 없음** 오류가 발생합니다.

## IBM COS(Cloud Object Storage)의 내 Content Delivery Network를 설정하는 방법은 무엇입니까?

IBM Cloud Object Storage의 Content Delivery Network  작성 방법에 대한 [튜토리얼](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn)입니다.

## 내 원본 인증서가 만료된다는 알림을 수신했습니다. 지금 어떠한 작업을 수행해야 합니까?

Akamai의 [이 기사](https://community.akamai.com/docs/DOC-7708)에 설명된 단계를 따르십시오.

## Akamai를 사용한 IBM CDN 솔루션에 포함된 보안은 무엇입니까?

분산 Akamai 플랫폼을 사용하면 130개국 이상에서 240,000개가 넘는 서버를 통해 엄청난 규모와 복원력을 갖게 됩니다. Akamai 플랫폼은 인프라와 일반 사용자 사이에서 트래픽의 갑작스런 증가에 대한 첫 번째 레벨의 방어 수단이 됩니다. 또한 Akamai Intelligent Platform은 포트 80과 443에서만 요청을 청취하고 응답하는 리버스 프록시이며, 이는 다른 포트의 트래픽이 인프라로 전송되지 않고 에지에서 삭제된다는 것을 의미합니다.
