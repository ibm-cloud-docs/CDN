---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# CDN 주문
{: #order-a-cdn}

여기에서 Content Delivery Network 주문 방법을 알아볼 수 있습니다. CDN은 [{{site.data.keyword.cloud}} 콘솔](https://cloud.ibm.com/login)에서 주문할 수 있습니다.

## 주문 준비
{: #preparation-for-ordering}

주문을 위한 CDN 페이지 탐색 방법이 있습니다.

**1단계 **

* [IBM Cloud 콘솔](https://cloud.ibm.com/login)에서 사용자 계정에 로그인하십시오.

**2단계**

[IBM Cloud 카탈로그](https://cloud.ibm.com/catalog/)를 클릭하십시오. 왼쪽 탐색줄에서 **네트워크**를 선택하십시오.

   ![IBM Cloud CDN 탐색](images/bluemix_navigation.png)

**3단계**

**CDN 타일**을 클릭하십시오.

   ![IBM Cloud CDN 아이콘](images/bluemix_tile.png)


## 새 CDN 주문
{: #order-a-new-cdn}

주문 페이지로 이동하여 CDN을 작성하고 구성하는 방법입니다.

### 1단계: CDN 계정 작성
{: #create-your-cdn-account}

오른쪽 맨 아래에서 **작성**을 클릭하십시오. CDN 계정이 없는 경우 CDN 계정을 작성하고 CDN 구성 화면으로 경로를 재지정합니다.

   ![CDN 개요](images/content-delivery.png)

### 2단계: CND 이름 지정
{: #step-2-name-your-cdn}

**구성 이름** 필드를 채우십시오.  

  * CDN의 기본 ID 역할을 하는 **호스트 이름**(**필수**)을 지정하십시오(예: `example.testingcdn.net`).  
  * 선택적으로, 사용자 정의 **CNAME**을 제공할 수 있습니다(예: `myfirstcdn.cdnedge.bluemix.net`). CNAME이 제공되지 않으면 사용자를 위해 CNAME이 작성됩니다. `cdnedge.bluemix.net` 접미부가 CNAME에 자동으로 추가됩니다. 적절하지 않은 CNAME을 사용하면 서비스가 종료될 수 있습니다.

       ![구성 이름](images/configure-hostname-cname.png)  

새 CDN을 프로비저닝한 후 DNS 제공자를 사용하여 CNAME을 구성**해야 합니다**.
{: note}
### 3단계: 원본 구성
{: #step-3-configure-your-origin}

**원본 구성** 필드를 채우십시오. 이 필드를 구성하려면 **서버** 또는 **Object Storage** 옵션 중 하나를 선택해야 합니다.  

  * **단계 3, 옵션 1: 서버 옵션**

     ![원본 구성](images/configure-origin-server.png)

      * **원본 서버 주소**(원본 서버의 호스트 이름 또는 IPv4 주소)를 지정해야 합니다. **HTTPS 포트**도 선택하는 경우 **원본 서버 주소**는 IP 주소가 아닌 호스트 이름이어야 합니다.

      * **호스트 헤더**를 지정하십시오(선택사항). 헤더가 제공되지 않은 경우 기본값은 **호스트 이름**입니다. 호스트 헤더에 대한 자세한 정보는 [호스트 헤더 지원](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support)의 기능 설명을 참조하십시오.  

      * 원본 서버에서 컨텐츠를 검색할 수 있는 **경로**를 제공하십시오(선택사항). 지금 경로를 추가하는 경우 미치는 영향을 파악하려면 [경로 기반 CDN 맵핑](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings)의 기능 설명을 참조하십시오.

      * 또한 **HTTP 포트**, **HTTPS 포트 ** 또는 둘 다를 제공해야 할 수 있습니다. 이러한 필드는 원본 서버에 접속하는 데 사용할 수 있는 프로토콜 및 포트 번호를 표시합니다. 기본이 아닌 포트 번호의 경우, 허용되는 포트 번호의 목록은 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.

      * **SSL 인증서** 이 옵션은 HTTPS 포트가 선택된 _경우에만_ 표시됩니다. 서버 또는 Object Storage에 대해 **HTTPS 포트**를 선택하는 경우 SSL 인증서 옵션으로 **와일드카드** 또는 **DV SAN 인증서**를 선택할 수 있습니다. 둘 다 HTTPS에서 제공하는 고급 보안을 제공합니다.
        * **와일드카드 인증서**를 사용하면 **CNAME**을 사용할 때만 HTTPS 트래픽을 허용하고 사용자가 추가로 조치를 수행할 필요가 없습니다.
        * **DV SAN 인증서**를 사용하면 도메인에서 HTTPS 트래픽이 허용되지만, 확인을 위해 추가 단계를 수행해야 합니다. 이 옵션 선택과 관련하여 필요한 단계 및 시간 제한조건을 확인하려면 [HTTPS에 대한 도메인 제어 유효성 검증 완료](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https) 페이지를 참조하십시오.

	     ![원본 서버 구성](images/ssl-cert-options.png)

  * **단계 3, 옵션 2: Object Storage 옵션**

    ![Object Storage 구성](images/configure-origin-object-storage.png)

      * 오브젝트를 페치할 **엔드포인트**를 지정**해야** 합니다.

      * **호스트 헤더**를 지정하십시오(선택사항). 헤더가 제공되지 않은 경우 기본값은 **호스트 이름**입니다. 호스트 헤더에 대한 자세한 정보는 [호스트 헤더 지원](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support)의 기능 설명을 참조하십시오.  

      * 원본 서버에서 컨텐츠를 검색할 수 있는 **경로**를 제공하십시오(선택사항). 여기에서 경로를 추가하는 경우 미치는 영향을 파악하려면 [경로 기반 CDN 맵핑](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings)의 기능 설명을 참조하십시오.

      * 컨텐츠가 저장되는 **버킷**의 이름을 제공**해야** 합니다.

      * 또한 **HTTP 포트**, **HTTPS 포트 ** 또는 둘 다를 제공해야 할 수 있습니다. 이러한 필드는 원본 서버에 접속하는 데 사용할 수 있는 프로토콜 및 포트 번호를 표시합니다. 기본이 아닌 포트 번호의 경우, 허용되는 포트 번호의 목록은 [FAQ](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)를 참조하십시오.

      * **SSL 인증서** 이 옵션은 HTTPS 포트가 선택된 _경우에만_ 표시됩니다. 서버 또는 Object Storage에 대해 **HTTPS 포트**를 선택하는 경우 SSL 인증서 옵션으로 **와일드카드** 또는 **DV SAN 인증서**를 선택할 수 있습니다. 둘 다 HTTPS에서 제공하는 고급 보안을 제공합니다.
        * **와일드카드 인증서**를 사용하면 **CNAME**을 사용할 때만 HTTPS 트래픽을 허용하고 사용자가 추가로 조치를 수행할 필요가 없습니다.
        * **DV SAN 인증서**를 사용하면 도메인에서 HTTPS 트래픽이 허용되지만, 확인을 위해 추가 단계를 수행해야 합니다. 이 옵션 선택과 관련하여 필요한 단계 및 시간 제한조건을 확인하려면 [HTTPS에 대한 도메인 제어 유효성 검증 완료](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https) 페이지를 참조하십시오.

        ![HTTPS 구성](images/ssl-cert-options.png)

Cloud Object Storage 제공자를 사용하여 버킷의 각 오브젝트에 대한 **액세스 제어 목록**(ACL)을 "public-read"로 설정해야 합니다.
{: note}
      
### 4단계
{: #step-4}

* 오른쪽 맨 아래의 **작성** 단추 위에 있는 **마스터 서비스 계약을 읽고 이용 약관에 동의합니다.**를 선택해야 합니다.

* 그런 다음 오른쪽 모서리 맨 아래의 **작성** 단추를 선택하십시오.
