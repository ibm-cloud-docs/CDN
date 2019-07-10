---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# 메트릭의 컨테이너 클래스
{: #container-class-for-metrics}

`SoftLayer_Container_Network_CdnMarketplace_Metrics` 콜렉션에 메트릭 API에서 사용되는 속성이 포함됩니다. `type` 속성에 가능한 값은 PIECHART, LINEGRAPH 또는 TOTALS입니다. 이 컨테이너의 나머지 속성에 대한 출력은 다음 추가 세부사항에 설명된 `type`에 따라 다릅니다. `type`을 제외한 모든 속성은 배열입니다. 모든 유형에 대해 모든 속성이 사용되는 것은 아닙니다.

**클래스** `SoftLayer_Container_Network_CdnMarketplace_Metrics`:
* `type`
* `names`
* `totals`
* `percentage`
* `time`
* `xaxis`
* `yaxis`
* `yaxis2`
* `yaxis3`
* `yaxis4`
* `yaxis5`
* `yaxis6`
* `yaxis7`
* `yaxis8`
* `yaxis9`
* `yaxis10`
* `yaxis11`
* `yaxis12`
* `yaxis13`
* `yaxis14`
* `yaxis15`
* `yaxis16`
* `yaxis17`
* `yaxis18`
* `yaxis19`
* `yaxis20`

PIECHART 유형은 원형 차트로 보낼 데이터의 유형입니다. 이 경우 사용되는 속성은 `names`, `xaxis` 및 `yaxis`입니다. `names` 속성은 지역 이름이 포함된 배열입니다. `xaxis` 속성에는 사용량(GB)이 포함되고 `yaxis1`은 테스트 백분율입니다.


LINEGRAPH 유형은 선 그래프로 보낼 데이터의 유형입니다. 이 경우 `names` 배열에 `xaxis`, `yaxis`, `yaxis2`...`yaxis20` 속성에 있는 메트릭의 이름이 포함되며, 실제 데이터는 해당 배열에 있게 됩니다.


`type`이 TOTALS인 경우, `names` 배열에 해당 `totals` 배열에 있는 메트릭의 이름이 포함되고 실제 데이터가 `totals` 배열에 포함됩니다.
