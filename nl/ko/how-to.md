---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

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

# CDN 관리
{: #manage-your-cdn}

이 문서는 CDN 관리에 대한 공통 태스크에 대해 설명합니다.

## "TTL(Time To Live)"을 사용하여 컨텐츠 캐싱 설정
{: #setting-content-caching-time-using-time-to-live}

CDN이 실행된 후 TTL(Time To Live)을 사용하여 컨텐츠 캐싱 시간을 설정할 수 있습니다. 특정 파일 또는 디렉토리 경로의 TTL(Time To Live)은 컨텐츠가 캐싱되어야 하는 기간을 표시합니다. CDN 맵핑을 작성한 경우 3,600초(1시간)의 기본 글로벌 TTL이 작성됩니다.

**단계 1:**  

CDN 페이지에서 **개요** 페이지로 이동할 수 있도록 CDN을 선택하십시오.

**단계 2:**  

화살표를 사용하거나 새 시간을 입력하여 시간을 조정할 수 있습니다. 시간 값은 초로 지정됩니다. 예를 들어, 3,600초는 1시간과 같습니다. `timeToLive`에 선택할 수 있는 가장 작은 값은 0초이고, 가장 큰 값은 2,147,483,647초입니다(약 24,855일). 컨텐츠 캐싱 시간을 설정하려면 **저장** 단추를 선택하십시오.

  ![TTL 추가](images/adding-path.png)

**단계 3:**

저장 후 오버플로우 메뉴 옵션을 사용하여 TTL 설정을 **편집** 또는 **삭제**할 수 있습니다. (**참고**: TTL의 경로를 변경할 수 없습니다. 맵핑 경로가 변경되면 TTL 경로가 자동으로 업데이트됩니다.)

  ![TTL 편집 또는 삭제](images/edit-delete-ttl-setting.png)  

  * 컨텐츠가 다중 규칙과 일치하면 가장 최근에 추가된 구성이 우선합니다.

  * 특정 파일 이름 또는 디렉토리에 대한 TTL 값만 설정할 수 있습니다. 정규식으로 예상치 못한 동작이 발생할 수 있으므로 정규식은 지원되지 않습니다.

## 원본 경로 세부사항 추가
{: #adding-origin-path-details}

CDN의 상태가 *CNAME_Configuration* 또는 *Running*인 경우 원본 경로 세부사항을 추가할 수 있습니다. 다중 원본 서버에서 컨텐츠를 제공하도록 선택할 수 있습니다. 예를 들어, 비디오와 다른 서버에서 사진을 전달할 수 있습니다. 원본은 호스트 서버 또는 Object Storage를 기반으로 할 수 있습니다.

CDN은 원본 서버에 대한 URL이 변환되도록 합니다. 예를 들어, 사용자가 URL `www.example.com/example/*`를 열 때 원본 `xyz.example.com`이 경로 `/example/*`과 함께 추가되는 경우 CDN 에지 서버는 `xyz.example.com/*`에서 컨텐츠를 검색합니다.
{: note}

**단계 1:**

CDN 페이지에서 **개요** 페이지로 이동할 수 있도록 CDN을 선택하십시오.  

**단계 2:**

**원본** 탭을 선택한 후 **원본 추가** 단추를 선택하십시오. 이 단계에서는 새 대화 상자 창을 열며, 이 창에서 원본을 구성할 수 있습니다.  

   ![원본 추가 원본](images/add-origins.png)

**단계 3:**

경로를 *제공해야* 합니다. 선택적으로 호스트 헤더를 제공할 수 있습니다.  

   ![원본 추가 원본](images/add-origin-path.png)

**단계 4:**

**서버** 또는 **Object Storage** 중 하나를 선택하십시오.

  * **서버**를 선택한 경우, IPv4 주소로 원본 서버 주소 또는 _호스트 이름_을 입력하십시오. 호스트 이름을 제공하고 완전한 도메인 이름(FQDN)을 제공하는 것이 좋습니다. CDN 작성 시 선택한 프로토콜에 따라 HTTP 포트, HTTPS 포트 또는 둘 다 제공하십시오. HTTPS 포트를 사용하는 경우 원본 서버 주소는 IP 주소가 아닌 _호스트 이름_**이어야 합니다**.

       ![원본 서버 추가](images/add-origin-server-default.png)

  * **Object Storage**를 선택한 경우 엔드포인트, 버킷 이름 및 HTTPS 포트를 제공하십시오. 선택적으로, CDN 서비스에 사용할 수 있는 파일 확장자를 지정하십시오. 아무 것도 지정되지 않으면 모든 파일 확장자가 허용됩니다.

       ![원본 Object Storage 추가](images/add-origin-object-storage.png)

  * **최적화** 및 **캐시 키** 옵션은 서버 및 Object Storage 구성에 대해 동일합니다.

    * 드롭 다운 메뉴에서 **최적화** 옵션을 선택하십시오. **일반 웹 전달**은 기본 옵션이며, **대형 파일** 또는 **VoD(Video on demand)** 최적화를 선택할 수도 있습니다. **일반 웹 전달**을 사용하면 CDN이 최대 1.8GB의 컨텐츠를 제공할 수 있는 반면, **대형 파일** 최적화를 사용하면 1.8GB에서 320GB까지의 파일을 다운로드할 수 있습니다. **VoD(Video on demand)**는 세그먼트화된 스트리밍 형식의 전달을 위해 CDN을 최적화합니다. [대형 파일 최적화](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization) 및 [VoD(Video on Demand)](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand)에 대한 기능 설명은 추가 정보를 제공합니다.

        ![성능 구성 옵션](images/performance-config-options.png)

    * 드롭 다운 메뉴에서 **캐시 키** 옵션을 선택하십시오. 기본 옵션은 **Include-all**입니다. **Include specified** 또는 **Ignore specified**를 선택하면 포함하거나 무시할 조회 문자열을 쉼표로 구분하여 입력**해야 합니다**. 예를 들어, 단일 조회 문자열의 경우에는 `uuid=123456`을 입력하고 두 조회 문자열의 경우에는 `uuid=123456 issue=important`를 입력하십시오.  기능 설명에서 [캐시 키 조회 인수](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args)에 대해 자세히 알아볼 수 있습니다.

        ![캐시 키 옵션](images/cache-key-options.png)

UI에 표시되는 프로토콜 및 포트 옵션은 CDN 주문 시 선택한 사항과 일치합니다. 예를 들어, **HTTP 포트**를 CDN 주문의 일부로 선택한 경우 **HTTP 포트** 옵션만 원본 추가의 일부로 표시됩니다.
{: note}

**단계 5:**

**추가** 단추를 선택하여 원본 경로를 추가하십시오.

Object Storage 원본 경로에 대한 파일 확장자를 제공하는 경우 원본 경로와 동일한 URL이 있는 TTL 설정은 지정된 파일 확장자가 있는 모든 파일을 포함하도록 범위가 지정됩니다. 예를 들어, `/example`의 원본 경로를 작성하고 "jpg png gif"의 파일 확장자를 지정하면 TTL 경로 `/example`의 TTL 값 범위는 `/example` 디렉토리 및 하위 디렉토리 아래에 있는 모든 JPG/PNG/GIF 파일을 포함합니다.
{: note}

**단계 6:**

추가 후 오버플로우 메뉴 옵션을 사용하여 원본을 **편집** 또는 **삭제**할 수 있습니다.

  ![원본 편집 또는 삭제](images/edit-delete-origin.png)

## 캐싱된 컨텐츠 제거
{: #purging-cached-content}

CDN이 실행된 후 벤더의 서버에서 캐싱된 컨텐츠를 제거할 수 있습니다.

**단계 1:**

CDN 페이지에서 **개요** 페이지로 이동할 수 있도록 CDN을 선택하십시오.

**단계 2:**

**제거** 탭을 선택하십시오.

   ![페이지 제거](images/purge_tab.png)

**단계 3:**

제거할 파일을 표시하도록 표준 Unix 경로 구문을 입력한 후 **제거** 단추를 선택하십시오. 현재 제거는 단일 파일에만 허용됩니다. 영구 제거 경로에 허용되는 구문에 대한 자세한 정보는 [규칙 및 이름 지정 규칙](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-) 페이지를 참조하십시오.

**단계 4:**

제거 후 활동은 **제거 활동** 아래에 나열됩니다. 오버플로우 메뉴 옵션을 사용하여 경로에 대해 **제거 다시 실행** 또는 **즐겨찾기 추가**를 수행할 수 있습니다.

   ![제거 활동](images/purge-activity.png)

16개 이상의 제거 항목이 있는 경우 제거 활동은 자동으로 15분마다 축소됩니다.
{: note}

## CDN 구성 세부사항 업데이트
{: #updating-cdn-configuration-details}

CDN이 실행된 후 CDN 구성 세부사항을 업데이트할 수 있습니다.

**단계 1:**

CDN 페이지에서 **개요** 페이지로 이동할 수 있도록 CDN을 선택하십시오.

**단계 2:**

**설정** 탭을 선택하십시오. CDN 구성 세부사항이 표시됩니다.

   ![설정 탭](images/settings-tab.png)  

CDN이 HTTPS로 구성된 경우 SSL Certificate만 표시됩니다.
{: note}

**서버**의 경우 다음 필드를 변경할 수 있습니다.
  * 호스트 헤더
  * 원본 서버 주소
  * HTTP/HTTPS 포트
  * 시간이 경과된(stale) 컨텐츠 제공
  * 헤더 준수
  * 최적화 옵션
  * 캐시 조회    

**Object Storage**의 경우 다음 필드를 변경할 수 있습니다.
  * 호스트 헤더
  * 엔드포인트
  * 버킷 이름
  * HTTPS 포트
  * 허용되는 파일 확장자
  * 시간이 경과된(stale) 컨텐츠 제공
  * 헤더 준수
  * 최적화 옵션
  * 캐시 조회

**단계 3:**

필요한 경우 **원본** 또는 **기타 옵션** 세부사항을 업데이트한 후 오른쪽 모서리 하단의 **저장** 단추를 클릭하여 CDN 구성 세부사항을 업데이트하십시오.

   ![저장 단추](images/save-button.png)
