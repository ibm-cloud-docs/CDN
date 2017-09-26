---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CDN 주문 방법

여기에서 CDN(Content Delivery Network) 주문 방법을 알아볼 수 있습니다. [고객 포털 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/) 또는 [Bluemix 카탈로그 포털](https://www.ibm.com/cloud-computing/bluemix/) 중 하나에서 CDN을 주문할 수 있습니다. 

## 제어 포털에서:

1. 시작하려면 고유 신임 정보를 사용하여 [고객 포털![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/)에 로그인하십시오. 

2. 표시장치의 상단의 탐색줄에서 **네트워크 -> CDN**을 선택하십시오.

   ![네트워크 메뉴 옵션](images/network-cdn.png)

3. **CDN(Content Delivery Networks)** 페이지의 오른쪽 상단 모서리에서 **CDN 주문** 단추를 선택하십시오. 

	![CDN 주문 선택](images/order-cdn-button.png)

## Bluemix 포털에서:

1. [Bluemix 포털](https://www.ibm.com/cloud-computing/bluemix/)에 로그온하십시오.

2. **IBM Bluemix 카탈로그**를 클릭하십시오. 왼쪽 탐색줄에서 **네트워크**를 선택하십시오.

   ![Bluemix CDN 탐색](images/bluemix_navigation.png)

3. 벤더 선택사항 화면으로 이동할 수 있도록 **CDN 타일**을 클릭하십시오. 

   ![Bluemix CDN 아이콘](images/bluemix_tile.png)

4. **CDN 제공자 선택** 화면에서 CDN 제공자 옵션을 선택하십시오. 선택한 옵션을 확인하려면 화면의 하단에서 **선택됨** 단추를 클릭하고, 그런 다음 **프로비저닝 시작**을 클릭하여 프로비저닝 프로세스를 시작하십시오. 

	![CDN 제공자 선택](images/newReducedSizeVendorSelectAndProvision.png)
	
5. **구성 이름** 필드를 채우십시오.  
      * CDN의 기본 ID 역할을 하는 _호스트 이름_(**필수**)을 지정하십시오(예: _example.testingcdn.net_).
      * 선택적으로, 사용자 정의 _CNAME_을 제공할 수 있습니다(예: _myfirstcdn.cdnedge.bluemix.net_). CNAME이 제공되지 않으면 사용자를 위해 CNAME이 작성됩니다. <여기에 포함될 유효성 검증 정보>
      
      ![구성 이름](images/configure-hostname-cname.png)
		
6. **원본 구성** 필드를 채우십시오. 이 필드를 구성하려면 **서버** 또는 **오브젝트 스토리지** 옵션 중 하나를 선택해야 합니다. (**경로** 선택은 선택사항입니다. <유효성 검증 정보>)
		
  * **서버 옵션**: **서버** 옵션을 선택한 경우 데이터를 캐싱할 원본 서버의 호스트 이름 또는 IP 주소를 입력하십시오.  
      * 이 옵션을 선택한 경우 원본 서버의 **IP 주소**를 지정해야 합니다. 
      * 또한 **HTTP 포트**, **HTTPS 포트 ** 또는 둘 다를 제공해야 할 수 있습니다. (이 필드는 원본 서버에 접속하는 데 사용할 수 있는 프로토콜 및 포트 번호를 표시합니다.)

	   ![원본 서버 구성](images/configure-origin-server.png)
		
  * **오브젝트 스토리지 옵션**: **오브젝트 스토리지** 옵션을 선택하는 경우 다음 정보를 제공해야 합니다. 
      * 오브젝트를 페치할 **엔드포인트**
      * 컨텐츠가 저장되는 **버킷**의 이름
      * **HTTPS 포트**.
      * CDN 서비스에 사용할 수 있는 파일 확장자(쉼표로 구분됨)를 제공할 수도 있습니다. (파일 이름 확장자가 지정되지 않으면 모든 파일 확장자가 허용됩니다.)
      * **버킷**에서 각 **오브젝트**에 대한 **액세스 제어 목록**(ACL)을 "public-read"로 설정해야 합니다.
		
	   ![오브젝트 스토리지 구성](images/configure-origin-object-storage.png)

7. **기타 옵션** 필드를 구성하십시오. 이 섹션에는 **시간이 경과된(stale) 컨텐츠 제공** 필드 및 **헤더 준수** 필드에 대한 구성 옵션이 포함됩니다. 
    
     * **시간이 경과된(stale) 컨텐츠 제공** 필드: **시간이 경과된(stale) 컨텐츠 제공**은 부울 값으로, 원본 서버에 도달할 수 없는 경우 만료된 캐싱된 컨텐츠의 제공 여부를 표시합니다. 현재 제한사항으로 인해 기본적으로 **시간이 경과된(stale) 컨텐츠 제공** 기능은 사용으로 설정됩니다(사용자가 이 기능을 **설정** 또는 **해제**로 설정한 것과 관계없음). 
     * **헤더 준수** 필드: **헤더 준수** 옵션이 **설정** 상태이면 원본으로 헤더에 정의된 TTL 설정은 기본 CDN TTL을 대체합니다. 기본적으로 **헤더 준수**는 **설정**으로 설정되지만 이 필드를 구성해야 합니다. 

		![기타 옵션](images/other-options.png)
		
8. CDN을 작성하려면 오른쪽 모서리 하단의 **작성** 단추를 선택하십시오. 
