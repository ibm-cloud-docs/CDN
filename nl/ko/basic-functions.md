---

copyright:
  years: 2018
lastupdated: "2018-06-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 실행 중 상태로 CDN 만들기

이 가이드라인을 따라 CDN을 실행 중 상태로 만드는 방법을 알아보십시오. 이 문서에서는 CDN 시작 및 중지 방법도 알려줍니다.

## 실행

CDN을 작성하면 CDN 대시보드에 즉시 표시됩니다. 여기서 CDN의 이름, 원본, 제공자 및 상태를 볼 수 있습니다.  

 ![맵핑 목록 스크린샷](images/mapping_list_cname.png)


와일드카드 인증서를 사용하는 HTTP 또는 HTTPS를 통해 CDN을 주문한 경우 1단계를 진행할 수 있습니다.

HTTPS DV SAN 인증서로 CDN을 작성한 경우 도메인을 확인하기 위해 [HTTPS의 도메인 제어 유효성 검증 완료](how-to-https.html#completing-domain-control-validation-for-https) 페이지에서 찾을 수 있는 추가 단계를 수행해야 할 수도 있습니다.

**단계 1:**

CDN을 주문한 후에는 DNS 제공자를 사용하여 **CNAME**을 구성해야 합니다. 대부분의 DNS 제공자는 CNAME 설정 또는 변경에 대한 지시사항을 제공할 수 있습니다.

   * 이 기간 동안 CDN의 상태는 **CNAME Configuration**으로 표시됩니다. 변경사항이 적용되는 시기를 알아보려면 DNS 제공자에게 문의하십시오.

   ![CNAME 구성](images/cname-config.png)  

**단계 2:**

DNS 제공자를 사용하여 CNAME을 구성한 후에는 언제든지 CDN 상태의 오른쪽에 있는 오버플로우 메뉴에서 **상태 가져오기**를 선택하여 상태를 확인할 수 있습니다.

  ![CNAME getStatus](images/cname-getstatus.png)  

**단계 3:**

CNAME 체인이 완료되면 **상태 가져오기**가 *실행 중*으로 변경되고 CDN을 사용할 수 있게 됩니다.

완료되었습니다! CDN은 이제 실행 중입니다. 여기에서 [CDN 관리](how-to.html#manage-your-CDN) 페이지에는 [TTL(Time to Live)](how-to.html#setting-content-caching-time-using-time-to-live-), [캐시된 컨텐츠 영구 제거](how-to.html#purging-cached-content) 및 [원본 경로 추가 세부사항](how-to.html#adding-origin-path-details)과 같은 구성 옵션에 대한 추가 정보가 있습니다.

## CDN 시작

'Stopped' 상태인 경우에만 CDN을 시작할 수 있습니다.  

**단계 1:**

CDN 행의 오른쪽에 세 개의 점으로 표시되는 오버플로우 메뉴에서 'CDN 시작'을 클릭하십시오.

  ![오버플로우 메뉴](images/start_cdn.png)

**단계 2:**

서비스를 시작할지 확인하는 대형 대화 상자 창이 표시됩니다. 진행하려면 **확인**을 선택하십시오.

**단계 3:**

조치가 성공적이면 화면의 오른쪽 상단 모서리에 시간 정보와 함께 사용자에게 성공했음을 알려주는 대화 상자가 표시됩니다.

**단계 4:**

이 단계에서는 상태가 'CNAME Configuration'으로 변경됩니다.

**단계 5:**

오버플로우 메뉴에서 '상태 가져오기'를 클릭하십시오. 이 단계에서는 상태가 'Running'으로 변경됩니다. CDN이 작동 가능하게 됩니다.

## CDN 중지

'Running' 상태인 경우에만 CDN을 중지할 수 있습니다.

**단계 1:**

오버플로우 메뉴(CDN 상태의 오른쪽에 수직으로 된 세 개의 점)에서 'CDN 중지'를 클릭하십시오.
 ![오버플로우 메뉴](images/stop_cdn.png)

**단계 2:**

서비스를 중지할 것인지 확인하는 대형 대화 상자 창이 표시됩니다. 진행하려면 **확인**을 선택하십시오.

**단계 3:**

약 5 - 15초 후에 상태를 'Stopped'으로 변경해야 합니다.

## CDN 삭제

CDN을 삭제하려면 다음 단계를 수행하십시오.

**참고**: 오버플로우 메뉴에서 `delete`를 선택하면 CDN만 삭제됩니다. 계정은 삭제되지 않습니다.

**단계 1:**

오버플로우 메뉴에서 '삭제'를 클릭하십시오.

 ![CDN 오버플로우 메뉴 삭제](images/delete_cdn.png)

**단계 2:**

삭제할지 확인하는 대형 대화 상자 창이 표시됩니다. 진행하려면 **삭제**를 클릭하십시오.

**참고**: DV SAN 인증서를 사용하는 HTTPS로 CDN을 구성한 경우 삭제 프로세스를 완료하는 데 최대 5시간이 걸릴 수 있습니다.

  ![경고를 표시하며 삭제](images/delete-with-warning.png)

**단계 3:**

1단계와 2단계를 완료하고 나면 CDN의 상태가 `Deleting`이 됩니다. 삭제 프로세스가 완료된 후 오버플로우 메뉴에서 '상태 가져오기'를 다시 클릭하면 CDN 목록에서 행이 제거됩니다. 삭제 프로세스가 완료되지 않으면 이 조치가 영향을 미치지 않습니다.
