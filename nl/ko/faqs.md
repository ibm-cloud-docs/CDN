---

copyright:
  years: 2017
lastupdated: "2017-09-06"

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

## CDN은 어떻게 작동합니까?

CDN은 전세계의 에지 서버에서 웹 컨텐츠를 캐싱하여 목적을 달성합니다. 사용자가 웹 컨텐츠를 요청하는 경우 컨텐츠 요청은 사용자와 지리적으로 가장 가까운 에지 서버로 라우팅됩니다. CDN은 컨텐츠가 이동해야 하는 거리를 줄여 최적화된 처리량, 최소화된 대기 시간 및 향상된 성능을 제공합니다.  

## 내 CDN 계정이 어떻게 작성됩니까?

"벤더 선택사항" 메뉴를 통과한 후 **선택**을 클릭하면 CDN 주문 프로세스 중에 사용자의 계정이 작성됩니다. 

## 내 CDN의 상태가 CNAME_CONFIGURATION인 경우 수행해야 할 작업은 무엇입니까?

웹 사이트가 새 CDN 맵핑과 연관된 `CNAME`을 지정하도록 DNS 레코드를 업데이트하십시오.
**참고**: 활성 상태가 되도록 업데이트하는 데 15 - 30분이 소요될 수 있습니다. 정확한 예상 시간을 알아 보려면 DNS 제공자에게 문의하십시오. 

## 청구 항목은 무엇입니까? 

CDN당 사용된 대역폭에 대해 지불해야 합니다. CDN에서 대역폭을 사용하지 않은 경우 금액이 청구되지 않습니다. 대역폭 비용은 에지 서버의 지리적 위치에 따라 달라집니다. 

## 청구 시기는 언제입니까?

{{site.data.keyword.BluSoftlayer_notm}} 계정에 설정된 청구 주기에 따라 CDN 비용 청구가 발생합니다. 

## 메트릭 및 사용법을 볼 수 있는 방법은 무엇입니까?

**컨텐츠 전달 네트워크** 페이지에서 CDN을 선택하여 도달할 수 있는 **개요** 페이지에서 메트릭 및 사용법을 볼 수 있습니다. **참고**: CDN을 작성한 후 메트릭이 표시되는 데 최대 48시간이 걸릴 수 있습니다. 

## 메트릭을 볼 수 있는 최소 일 수가 있습니까? 최대 일 수가 있습니까?

예. 최소 7일 동안 메트릭을 수집할 수 있습니다. 최대 90일 동안 메트릭을 볼 수 있습니다. API를 사용하는 사용자의 경우 시간대 차이를 처리하기 위해 최대 89일을 사용하는 것이 좋습니다. 

## HTTPS 와일드카드 인증서는 어떻게 작동합니까?

와일드카드 인증서는 일반 사용자에게 웹 컨텐츠를 안전하게 전달하는 가장 경제적인 방법입니다. 와일드카드 인증서를 사용하려면 고객은 서비스의 시작점으로 CNAME 호스트 이름을 사용해야 합니다(예: _https://example.cdnedge.bluemix.net_). CDN 맵핑이 와일드카드 인증서를 통해 HTTPS에 사용된 후 에지 서버는 HTTPS를 통해 원본 서버에 접속합니다. 원본 서버가 호스트 이름으로 지정된 경우, 기본적으로 에지 서버는 원본 서버를 사용하여 TLS(Transport Layer Security) 조정에 대해 SNI(Server Name Indication) 헤더로 원본 도메인을 사용합니다. 그러나 원본 도메인이 IP 주소로 지정된 경우 원본 호스트 헤더가 제공되어야 합니다. 다른 팁은 공인된 인증 기관(CA)에서 원본 인증서를 서명해야 하는 것입니다. 

## CDN의 오버플로우 메뉴에서 `delete`를 선택한 경우 내 계정이 삭제됩니까? 

아니오. 해당 CDN만 삭제됩니다. 사용자의 계정은 계속 유지되며 추가 CDN을 작성할 수 있습니다. 

## 캐싱에서 푸시 또는 풀을 사용합니까?

컨텐츠 캐싱은 _원본 풀_ 모델을 사용하여 수행됩니다. 원본 풀은 컨텐츠를 에지 서버에 수동으로 업로드하는 것과는 반대로 데이터가 원본 서버에서 에지 서버로 "풀링"되는 메소드입니다.  

## TTL(Time To Live)의 최대값이 있습니까? 최소값은 있습니까?

TTL(Time To Live)의 최대값은 21,474,836,471초이며, 이 값은 대략 681년과 동일합니다! 최소값은 30초입니다.

## 시간이 경과된(stale) 옵션 제공은 무엇입니까?

컨텐츠에 대한 캐싱 시간이 만료된 경우 에지 서버는 원본 서버에서 컨텐츠 페치를 시도합니다. 어떠한 이유로 원본 서버가 작동 중지되거나 접속할 수 없는 경우 에지 서버는 시간이 경과된(stale) 컨텐츠를 제공할 수 있습니다. 해당 옵션이 설정되면 이 옵션은 대부분의 고객이 선호하는 옵션입니다. 현재 CDN은 기본적으로 **예**로 설정된 이 옵션만 사용하여 구성될 수 있습니다. 옵션을 **No**로 변경하면 어떠한 영향도 주지 않습니다. **시간이 경과된(stale) 컨텐츠 제공** 옵션은 계속해서 **예**로 설정됩니다.

## 원본 및 TTL 항목의 수에 대한 제한이 있습니까?

예. 결합된 제한은 CDN당 75개의 항목입니다. 

## 보유할 수 있는 CDN의 수에 대한 제한이 있습니까?

예. 계정당 10개의 CDN만을 보유할 수 있습니다. 

## CDN 서비스 구성에 사용할 권장되는 브라우저가 있습니까?

예. 최신 버전의 Firefox와 Chrome 브라우저를 권장합니다. 

## 내 CDN을 작성할 때 경로를 제공하는 목적은 무엇입니까?

CDN을 작성하는 중에 경로를 제공하면 특정 원본 서버에서 CDN을 통해 제공될 수 있는 파일을 격리할 수 있습니다. 

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

## 내 CName을 제공하지 않은 경우 어디서 찾을 수 있습니까? 
CDN을 클릭하여 **개요** 페이지로 이동하십시오. 오른쪽 모서리에서 `CName` 정보가 있는 **세부사항** 섹션을 볼 수 있습니다. 

## 한 번에 활성 상태가 될 수 있는 제거 요청 수에 대한 제한이 있습니까?
예. 한 번에 다섯 개의 제거 요청만 가능합니다. 

## 제공된 파일 경로에 대한 내 제거 요청이 진행 중입니다. 동일한 파일 경로로 새 요청을 제출할 수 있습니까?
아니오. 제공된 파일 경로로 한 번에 하나의 활성 제거 요청만 가능합니다. 
