---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 메트릭
{: #metrics}

목록에서 처음으로 CDN을 선택하는 경우 개요 페이지가 열립니다. 선택한 기간(기본값 30일)에 대한 총 대역폭, 총 히트 수 및 히트 비율을 확인할 수 있습니다.

  ![메트릭 개요](images/metrics-overview.png)

개요 바로 아래에 총 대역폭, 지역별 대역폭, 총 히트 수 및 유형별 히트 수에 대한 그래픽 표현이 표시됩니다.

  ![메트릭 그래프](images/metrics-graphs.png)

**참고**: CDN을 작성한 후 메트릭이 표시되는 데 최대 48시간이 걸릴 수 있습니다.

## 메트릭을 볼 수 있는 최소 일 수가 있습니까? 최대 일 수가 있습니까?

{{site.data.keyword.cloud}} Content Delivery Network with Akamai를 사용하여 메트릭을 볼 수 있는 최대 일 수 및 최소 일 수가 있습니다. 최소 7일 동안 메트릭을 수집할 수 있습니다. 최대 90일 동안 메트릭을 볼 수 있습니다. API를 사용하는 사용자의 경우 시간대 차이를 처리하기 위해 최대 89일을 사용하는 것이 좋습니다.

## 히트 총계가 0인 경우 히트 비율이 0이 아닌 이유는 무엇입니까?
히트 비율은 컨텐츠가 원본 서버에서 전달되지 않고 에지 서버 캐시에서 전달된 시간의 백분율을 나타냅니다. 계산 방법은 다음과 같습니다.

`((Edge hits - Ingress hits)/Edge hits) * 100`

여기서

_Edge hits_는 "일반 사용자가 에지 서버에 대해 수행한 모든 히트"로 정의됩니다.  
_Ingress hits_는 "Origin 또는 Ingress hits가 원본에서 Akamai 에지 서버로의 트래픽임"으로 정의됩니다.

히트 비율이 CDN별이 아니라 계정 레벨에서 계산되기 때문에 히트 비율은 계정의 모든 CDN에 동일하게 됩니다. 이는 특정 CDN에 대한 에지 히트의 수가 0일 때 히트 비율이 0이 아닐 수 있는 이유도 설명합니다.

## 메트릭은 실시간으로 업데이트됩니까?

아니요. 메트릭은 24시간마다 업데이트됩니다.
