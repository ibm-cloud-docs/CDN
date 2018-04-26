---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# FAQ

## CDN(Content Delivery Network)은 무엇입니까?

CDN(Content Delivery Network)은 국가 또는 전세계의 여러 지역을 통해 분산되는 에지 서버의 콜렉션입니다. 웹 컨텐츠는 컨텐츠를 요청하는 고객과 가장 가까운 지역에 위치하는 에지 서버에서 제공됩니다. 이 기술을 통해 사용자는 하나의 중앙 위치에서 컨텐츠를 전달하여 수행하는 것 보다 지연 시간을 줄여 컨텐츠를 수신할 수 있습니다. 고객을 위해 개선된 전체 경험을 전달합니다.

## CDN(Content Delivery Network)은 어떻게 작동합니까?

CDN은 전세계의 에지 서버에서 웹 컨텐츠를 캐싱하여 목적을 달성합니다. 사용자가 웹 컨텐츠를 요청하는 경우 컨텐츠 요청은 사용자와 지리적으로 가장 가까운 에지 서버로 라우팅됩니다. CDN은 컨텐츠가 이동해야 하는 거리를 줄여 최적화된 처리량, 최소화된 대기 시간 및 향상된 성능을 제공합니다. 

## IBM Cloud Content Delivery Network 서비스 계정을 작성하는 방법은 무엇입니까?

"벤더 선택사항" 메뉴를 거친 후 **선택**을 클릭하면 CDN 주문 프로세스 중에 사용자 계정이 작성됩니다.

## 내 CDN의 상태가 CNAME_CONFIGURATION인 경우 수행해야 할 작업은 무엇입니까?

HTTP 기반 CDN의 경우 웹 사이트가 새 CDN 맵핑과 연관된 `CNAME`을 지정하도록 DNS 레코드를 업데이트하십시오. HTTPS 기반 CDN의 경우에는 이 DNS 업데이트가 필요하지 **않습니다**.

**참고**: 활성 상태가 되도록 업데이트하는 데 15 - 30분이 소요될 수 있습니다. 정확한 예상 시간을 알아 보려면 DNS 제공자에게 문의하십시오.

## 청구 항목은 무엇입니까?

IBM Cloud Content Delivery Network 인스턴스마다 사용되는 대역폭에 대해서만 비용이 청구됩니다. CDN에서 대역폭을 사용하지 않은 경우 금액이 청구되지 않습니다. 대역폭 비용은 에지 서버의 지리적 위치에 따라 달라집니다. 이 서비스에 대한 [Getting Started](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd) 섹션에서 지역별 대역폭 가격 책정을 볼 수 있습니다.

## 청구 시기는 언제입니까?

{{site.data.keyword.BluSoftlayer_notm}} 계정에 설정된 청구 기간에 따라 IBM Cloud Content Delivery Network 비용 청구가 발생합니다.

## 메트릭 및 사용법을 볼 수 있는 방법은 무엇입니까?

**컨텐츠 전달 네트워크** 페이지에서 CDN을 선택하여 도달할 수 있는 **개요** 페이지에서 메트릭 및 사용법을 볼 수 있습니다. **참고**: CDN을 작성한 후 메트릭이 표시되는 데 최대 48시간이 걸릴 수 있습니다.

## CDN을 작성했으며 CDN을 통한 데이터 트래픽이 있습니다. 메트릭이 표시되지 않는 이유는 무엇입니까?

CDN 작성 후 메트릭이 표시되는 데 48시간이 걸립니다.

## 메트릭을 볼 수 있는 최소 일 수가 있습니까? 최대 일 수가 있습니까?

IBM Cloud Content Delivery Network with Akamai를 사용하여 메트릭을 볼 수 있는 최대 일 수 및 최소 일 수가 있습니다. 최소 7일 동안 메트릭을 수집할 수 있습니다. 최대 90일 동안 메트릭을 볼 수 있습니다. API를 사용하는 사용자의 경우 시간대 차이를 처리하기 위해 최대 89일을 사용하는 것이 좋습니다.

## 히트 총계가 0인 경우 히트 비율이 0이 아닌 이유는 무엇입니까?
히트 비율은 컨텐츠가 원본 서버에서 전달되지 않고 에지 서버 캐시에서 전달된 시간의 백분율을 나타냅니다. 계산 방법은 다음과 같습니다.

```
((Edge hits - Ingress hits)/Edge hits) * 100

where: 
Edge hits is defined as "All hits to the edge servers from the end-users"
Ingress hits is defined as "Origin or Ingress hits are for traffic from your origin to Akamai edge servers"
```
 
히트 비율이 CDN별이 아니라 계정 레벨에서 계산되기 때문에 히트 비율은 계정의 모든 CDN에 동일하게 됩니다. 이는 특정 CDN에 대한 에지 히트의 수가 0일 때 히트 비율이 0이 아닐 수 있는 이유도 설명합니다.

## CDN의 오버플로우 메뉴에서 `delete`를 선택한 경우 내 계정이 삭제됩니까?

아니오. 해당 CDN만 삭제됩니다. 사용자의 계정은 계속 유지되며 추가 CDN을 작성할 수 있습니다.

## 캐싱에서 푸시 또는 풀을 사용합니까?

컨텐츠 캐싱은 _원본 풀_ 모델을 사용하여 수행됩니다. 원본 풀은 컨텐츠를 에지 서버에 수동으로 업로드하는 것과는 반대로 데이터가 원본 서버에서 에지 서버로 "풀링"되는 메소드입니다. 

## TTL(Time To Live)의 최대값이 있습니까? 최소값은 있습니까?

TTL(Time To Live)의 최대값은 2,147,483,647초이며, 이 값은 대략 68년에 해당합니다. 최소값은 30초입니다.

## 원본 및 TTL 항목의 수에 대한 제한이 있습니까?

예. 결합된 제한은 CDN당 75개의 항목입니다.

## 보유할 수 있는 CDN의 수에 대한 제한이 있습니까?

예. 계정당 10개의 CDN만을 보유할 수 있습니다.

## CDN 서비스 구성에 사용할 권장되는 브라우저가 있습니까?

예, Firefox 및 Chrome 브라우저가 권장됩니다. IBM Cloud Content Delivery Network에 대한 최신 버전을 사용하는 것이 좋습니다.

## 내 CDN을 작성할 때 경로를 제공하는 목적은 무엇입니까?

CDN을 작성하는 중에 경로를 제공하면 특정 원본 서버에서 CDN을 통해 제공할 수 있는 파일을 격리할 수 있습니다.

## 내 CDN이 작동 중임을 알 수 있는 방법은 무엇입니까?
"origin.cdntesting.net/assets/ibm_3d.gif"를 원본의 각 파일 경로로 바꿔 다음 'curl' 명령을 실행하십시오. 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
명령의 출력이 다음 형식과 일치하면 CDN은 예상대로 작동합니다. 
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## 내 CDN은 오류 상태에 있습니다. 지금 어떠한 작업을 수행해야 합니까?
['도움 및 지원 받기'](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp) 페이지를 참조하거나 [고객 포털![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에서 티켓을 여십시오.

## 내 CNAME을 제공하지 않은 경우 어디서 찾을 수 있습니까? 
CDN을 클릭하여 포털의 **개요** 페이지에 액세스하십시오. 오른쪽 모서리에서 `CName` 정보가 있는 **세부사항** 섹션을 볼 수 있습니다.

## 한 번에 활성 상태가 될 수 있는 제거 요청 수에 대한 제한이 있습니까?
예. 한 번에 다섯 개의 제거 요청만 가능합니다.

## 제공된 파일 경로에 대한 내 제거 요청이 진행 중입니다. 동일한 파일 경로로 새 요청을 제출할 수 있습니까?
아니오. 제공된 파일 경로로 한 번에 하나의 활성 제거 요청만 가능합니다.

## IPv6(Internet Protocol version 6)이 IBM Cloud Content Delivery Network 서비스와 함께 지원됩니까? 어떻게 작동합니까?
IPv6(또는 이중 스택 지원)은 Akamai의 에지 서버에서 지원됩니다. IPv4 전용 원본의 고객이 IPv6 클라이언트에서 연결을 허용하는 것을 돕도록 디자인되었으며, 에지에서 IPv6에서 IPv4로 변환하고 IPv4로 원본으로 이동하십시오.

**참고:** 원본 서버 주소로 IPv6 주소를 사용하여 IBM Cloud CDN을 작성하는 것은 지원되지 않습니다.

## 호스트 이름에 대한 규칙은 무엇입니까?
`호스트 이름` 입력 문자열은 다음과 **같아야 합니다**.
  * 영숫자 문자로 구성
  * 254자 미만임
  * 올바른 최상위 레벨 도메인 이름으로 끝남
  * 'cdnedge.bluemix.net`(이는 CNAMES에 사용되고 예약되어 있음)으로 끝나면 **안 됨**

세부사항은 RFC 1035, 섹션 2.3.4를 참조하십시오.

## 사용자 정의 CNAME 이름 지정 규칙은 무엇입니까?
`CNAME` 입력 문자열은 다음 규칙을 따라야 합니다.
  * 고유**해야 함**(다른 IBM Cloud CDN에서 사용할 수 없음)
  * 64자 미만의 영숫자 문자임
  * 특수 문자 `! @ # $ % ^ & *`가 허용되지 **않음**
  * 밑줄 `_`이 허용되지 **않음**
  * 마침표 `.`가 허용되지 **않음**
  * 대시 `-`가 허용되지만 CNAME을 대시로 시작하거나 끝낼 수 없음

## 버킷 이름에 대한 규칙은 무엇입니까?
버킷 이름:
  * 한 자 이상이어야 함
  * 199자 이하여야 함
  * 문자(대문자와 소문자 둘 다 허용됨), 숫자 및 하이픈을 포함할 수 있음
  * IP 주소(예: 127.0.0.1)로 형식화하면 **안 됨**
  * 마침표(.)로 시작할 수 **없음**
  * 마침표(.)로 끝날 수 있음
  * 레이블 사이에 하나의 마침표(.)만 허용됨(예: my..ibmcloud.bucket은 허용되지 않음) 

**참고**: 대문자와 마침표가 유효성 검증을 통과할 수는 있지만 항상 DNS 준수 버킷 이름을 사용하는 것이 좋습니다.

## 원본의 경로 문자열에 대한 규칙은 무엇입니까?
CDN 작성 시 경로는 선택사항입니다. 하지만 제공되는 경우 경로는 다음과 **같아야 합니다**.
  * 1000자 미만임
  * '/'로 시작

## **원본 추가** 명령의 경우, 경로 문자열에 대해 따라야 하는 규칙이 있습니까?
**원본 추가**에 대해서는 경로가 필수입니다. 또한, CDN이 경로와 함께 작성된 경우 **원본 추가**의 경로는 접두부로서 CDN 경로로 시작해야 합니다. 예를 들어, CDN 경로가 `/storage`로 지정된 경우, `/examplePath`라는 경로와 함께 **원본 추가**를 호출하려면 제공되는 경로는 `/storage/examplePath`가 됩니다.

## 이 CDN 서비스를 사용하여 오브젝트 스토리지 원본 유형을 작성하기 위해 파일 확장자를 어떤 형식으로 제공해야 합니까?

파일 확장자는 쉼표로 구분되어야 합니다. 예를 들어, "txt, jpg, xml" 목록이 올바른 목록입니다. 특정 확장자는 10자 이상일 수 없습니다.

## Akamai에 허용되는 HTTP 및 HTTPS 포트 번호에 대한 제한사항이 있습니까?
예. Akamai 벤더에 대해 다음 포트 번호만 허용됩니다.
72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111, 1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 및 45002.

## CDN 또는 원본 경로 아래 데이터에 액세스하기 위해 어떤 URL을 사용해야 합니까? 
CDN 맵핑 또는 원본에 대한 경로가 디렉토리로 간주됩니다. 따라서 원본 경로에 액세스하려는 사용자는 디렉토리(슬래시 포함)로서 원본 경로에 액세스해야 합니다. 예를 들어, `/images` 디렉토리가 포함된 경로를 사용하여 CDN `www.example.com`이 작성되는 경우, 접속할 URL은 `www.example.com/images/`여야 합니다.

슬래시를 생략(예: `www.example.com/images` 사용)하면 **페이지를 찾을 수 없음** 오류가 발생합니다.

## HTTPS의 경우, curl 명령 또는 호스트 이름을 사용하는 브라우저를 통해 연결할 수 없는 이유는 무엇입니까?

현재 HTTPS는 와일드카드 인증서를 통해서만 지원됩니다. 이 제한사항의 결과, CNAME을 사용하여 연결을 작성해야 하며 호스트 이름을 사용하여 연결하려고 하면 실패하게 됩니다.

## 지원되는 CDN 프로토콜의 CNAME 또는 호스트 이름을 브라우저에 로드할 때 예상되는 동작은 무엇입니까?

|브라우저 URL | HTTP 프로토콜만 사용하는 CDN | HTTPS 프로토콜만 사용하는 CDN | HTTP 및 HTTPS 프로토콜 둘 다 사용하는 CDN |
|-------|-----|-----|-----|
|http://hostname | 로드 성공 | 301 영구 이동 | 301 영구 이동 |
|https://hostname | 액세스 거부됨 | IBM Cloud 웹 페이지로 경로 재지정 | IBM Cloud 웹 페이지로 경로 재지정 | 
|http://cname| 301 영구 이동 | 액세스 거부됨 | 로드 성공 | 
|https://cname| IBM Cloud 웹 페이지로 경로 재지정 | 로드 성공 | 로드 성공 |

## IBM COS(Cloud Object Storage)의 내 CDN(Content Delivery Network)을 설정하는 방법은 무엇입니까?
IBM Cloud Object Storage의 CDN(Content Delivery Network) 작성 방법에 대한 [튜토리얼](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn)입니다.

## IBM COS(Cloud Object Storage)를 원본으로 선택할 때 브라우저에 내 호스트 이름을 로드하지 못하는 이유는 무엇입니까?

IBM Cloud CDN이 IBM COS를 오브젝트 스토리지로 사용하도록 구성된 경우 웹 사이트에 대한 직접 액세스가 작동하지 않습니다. 브라우저의 주소 표시줄에 전체 요청 경로를 지정해야 합니다(예: `www.example.com/index.html`). 이 동작은 IBM COS의 색인 문서 제한사항으로 인해 나타납니다.

## Akamai CDN을 통해 전달할 수 있는 가장 큰 파일 크기는 얼마입니까?

1.8GB보다 큰 파일을 검색하거나 전달하려고 시도하면 기본 성능 구성에 대해 `403 Access Forbidden` 응답이 수신됩니다. 대형 파일 최적화를 사용하면 최대 320GB의 파일 다운로드가 가능합니다.

## 내 원본 인증서가 만료된다는 알림을 수신했습니다. 지금 어떠한 작업을 수행해야 합니까?

Akamai의 [이 기사](https://community.akamai.com/docs/DOC-7708)에 설명된 단계를 따르십시오.

## 바이트 범위 요청은 무엇이고 Akamai CDN에서 어떻게 작동합니까?

바이트 범위 요청은 원본 서버에서 부분 컨텐츠를 검색하는 데 사용됩니다. 범위 HTTP 요청 헤더는 서버가 리턴해야 하는 컨텐츠의 파트를 표시합니다. 한 번에 하나의 범위 헤더를 사용하여 여러 파트를 요청할 수 있으며 서버는 다중 파트 응답으로 이러한 범위를 반송할 수 있습니다. 서버가 범위를 반송하면 206(부분 컨텐츠) 상태로 응답합니다.

바이트 범위 요청이 IBM Cloud CDN with Akamai를 사용하여 전송되면 사용자는 첫 번째 요청에 대해 200(확인) 응답 코드를 수신하고 이후 모든 요청에 대해서는 206 응답 코드를 수신합니다. 이는 Akamai 에지 서버가 원본에서 압축 형식으로 컨텐츠를 요청하기 때문입니다. 따라서 에지 서버의 캐시에 오브젝트가 없거나 오브젝트의 컨텐츠 길이에 대한 정보가 없는 경우 이 에지 서버가 원본으로 이동하고 전체 오브젝트를 요청합니다. 이 경우 원본은 Akamai에 컨텐츠 길이 헤더가 없는 오브젝트를 제공하고, 바이트 범위 요청이더라도 일반 사용자에게 전체 오브젝트가 제공됩니다. 따라서 상태 코드는 200입니다. 후속 요청에서는 에지 서버 캐시에 오브젝트가 있으며 에지 서버가 206 상태 코드를 제공합니다.

첫 번째 바이트 범위 요청에 대해서도 206 응답을 보장하는 한 가지 방법은 원본 서버에서 `전송-인코딩: 청크됨`을 사용 안함으로 설정하는 것입니다.

## Akamai를 사용한 IBM CDN 솔루션에 포함된 보안은 무엇입니까?

분산 Akamai 플랫폼을 사용하면 130개국 이상에서 240,000개가 넘는 서버를 통해 엄청난 규모와 복원력을 갖게 됩니다. Akamai 플랫폼은 인프라와 일반 사용자 사이에서 트래픽의 갑작스런 증가에 대한 첫 번째 레벨의 방어 수단이 됩니다. 또한 Akamai Intelligent Platform은 포트 80과 443에서만 요청을 청취하고 응답하는 리버스 프록시이며, 이는 다른 포트의 트래픽이 인프라로 전송되지 않고 에지에서 삭제된다는 것을 의미합니다.
