---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: domain, control, validation, https, san certificate, challenge, apache, nginx, redirect

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# DV SAN으로 HTTP에 대한 도메인 제어 유효성 검증 완료
{: #completing-domain-control-validation-for-https-with-dv-san}

다음 다이어그램은 CDN이 작성된 시간부터 실행 중 상태가 될 때까지의 다양한 상태에 대해 간략히 정의합니다.

  ![SAN 상태 다이어그램](images/state-diagram-san.png)

## 도메인 제어 유효성 검증의 초기 단계
{: #initial-steps-to-domain-control-validation}

**단계 1:**

DV SAN 인증서를 사용하여 CDN을 주문하고 나면 인증서 요청 프로세스가 시작됩니다. 이 프로세스 중에 {{site.data.keyword.cloud}} CDN에서 Akamai의 인증서를 요청합니다. 인증서를 사용할 수 있게 되면 Akamai에서 인증 기관(CA)에 요청을 합니다.

  * 이 시간 동안 CDN 상태는 **인증서 요청 중**으로 표시됩니다.

    ![인증서 요청 중 상태](images/requesting-cert.png)

**단계 2:**

CA에서 요청을 받으면 도메인 유효성 검증 인증 확인을 발행합니다.

  * 이 경우 CDN의 상태가 **도메인 유효성 검증 필요**로 변경됩니다.

    ![도메인 유효성 검증 필요](images/domain-validation-needed.png)

**단계 3:**

유효성을 검증해야 할 CDN의 이름을 클릭하십시오. CDN의 전체 상태를 볼 수 있는 개요 페이지가 열립니다. 페이지 맨 위에 도메인 유효성 검증이 필요함을 미리 알리는 경보가 표시됩니다. **도메인 유효성 검증 보기** 단추를 선택하여 유효성 검증 프로세스를 완료하는 데 필요한 인증 확인 정보를 표시하는 창을 여십시오.

   ![도메인 유효성 검증 필요](images/view-domain-validation.png)

**단계 4:** 도메인 유효성 검증 인증 확인을 처리하는 방법에 대한 섹션에서 유효성 검증 단계 중 하나를 완료하고 나면, CDN이 **인증서 배치** 상태로 이동합니다. 이 시간 동안 Akamai에서 유효성 검증된 인증서를 에지 서버에 분배합니다. 인증서를 배치하는 데 2 - 4시간이 걸릴 수 있습니다.

  * 이 프로세스가 완료되면 사용한 유효성 검증 메소드와 상관없이 모든 도메인이 **CNAME 구성** 상태로 이동합니다.

CNAME 구성 완료 및 CDN 감독에 대한 추가 정보는 [실행하기](/docs/infrastructure/CDN?topic=CDN-getting-your-cdn-to-running-status#get-to-running) 페이지에서 찾을 수 있습니다.


## 도메인 제어 유효성 검증
{: #domain-control-validation}

SAN 인증서에 추가된 CDN 도메인 이름을 가져오려면, 도메인에 대한 관리 제어 권한이 있음을 증명해야 합니다. 이 증거 프로세스는 도메인 제어 유효성 검증(DCV) 처리라고 합니다. 48시간 이내에 DCV를 처리해야 합니다. 검증 처리에 실패하는 경우 요청이 만료되고 주문 프로세스를 다시 시작해야 합니다. DCV를 처리하는 세 가지 다른 방법이 다음 섹션에 설명되어 있습니다.


### CNAME
{: #cname}

이 메소드는 CDN이 라이브 트래픽을 제공하지 **않는** **경우에만** 사용하는 것이 좋습니다. 도메인에서 라이브 트래픽을 제공하는 경우에는 도메인의 유효성을 검증하는 데 표준 또는 경로 재지정 메소드 중 하나를 사용하는 것이 좋습니다.

이 메소드를 사용하기 위해 CDN 도메인에 대한 CNAME 레코드를 DNS 구성에 추가합니다. 사용할 CNAME 값은 CDN을 작성할 때 사용한 CNAME입니다. `cdnedge.bluemix.net` 도메인으로 끝나야 합니다. 사용자가 다른 조치를 수행할 필요는 없습니다. 이 때부터 DCV에서 자동으로 진행합니다. 유효성 검증은 2 - 4시간이 걸릴 수 있습니다. 인증서가 배치되고 나면 CDN은 바로 RUNNING 상태로 이동합니다.

대부분의 DNS 제공자는 CNAME 설정 또는 변경에 대한 지시사항을 제공할 수 있습니다. 다음은 일반 CNAME 레코드의 예입니다.

| **리소스 유형** | **호스트** | **지정 대상 (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
|CNAME | www.example.com | example.cdnedge.bluemix.net | 15분 |


---
### 표준
{: #standard}

도메인 유효성 검증의 표준 메소드를 선택하면 도메인 유효성 검증 창에 **인증 확인 URL** 및 **인증 확인 응답**이 표시됩니다. 도메인 유효성 검증 프로세스를 완료하려면, 제공된 **인증 확인 응답**을 원본 서버에 추가하십시오. 추가되고 나면, CA가 **인증 확인 URL**에 지정된 URL을 사용하여 원본 서버에서 **인증 확인 응답**을 검색할 수 있습니다. 원본 서버가 올바르게 구성되고 나면 도메인 유효성 검증을 수행하는 데 2 - 4시간이 걸릴 수 있습니다.

   ![도메인 유효성 검증 인증 확인 표준](images/domain-validation-standard.png)

표준 메소드를 통해 도메인 유효성 검증을 완료하려면, 특정 방식으로 원본 서버를 구성해야 합니다. _Apache(TM)_ 및 _Nginx(TM)_ 서버에 대한 예제 프로시저가 아래 요약되어 있습니다.

**예제 환경**
* 원본 서버: `www.example.com`
* CDN 도메인: `cdn.example.com`
* 인증 확인 URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* 인증 확인 응답: `examplechallenge`

#### Apache 구성
{: #apache-configuration}

  * **1단계:** Apache2 서버를 실행 중인 시스템에 로그인하십시오.

  * **2단계:** 웹 사이트 컨텐츠용 디렉토리의 `.well-known/acme-challenge/`에 인증 확인 응답의 인증 확인 응답 파일을 작성하십시오.  Apache2 웹 사이트 컨텐츠의 기본 위치는 `/var/www/html/`입니다. 이 예제의 경우 인증 확인 응답은 `/var/www/html/.well-known/acme-challenge/` 디렉토리에 있습니다.

      ```
mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **3단계:** 필요하면 Apache2 서버 구성 파일을 여십시오. `/etc/apache2/apache2.conf`와 `/etc/apache2/sites-enabled/`는 구성 파일의 기본 위치입니다.

  * **4단계:** 필요하면 CDN 도메인을 추가 **ServerAlias**로 원본의 가상 호스트에 추가하십시오.

  * **5단계:** Apache2 서버 구성을 수정해야 하는 경우 다음 명령을 사용하여 Apache2 서버를 다시 시작하고 가동 중단 시간을 최소화하십시오.

      ```
apachectl -k graceful
      ```

  * **6단계:** CDN 도메인과 원본 서버의 IP 주소 사이의 DNS에 A 레코드를 작성하십시오.

#### Nginx 구성
{: #nginx-configuration}

  * **1단계:** Nginx 서버를 실행 중인 시스템에 로그인하십시오.

  * **2단계:** 웹 사이트 컨텐츠용 디렉토리의 `.well-known/acme-challenge/`에 인증 확인 응답의 인증 확인 응답 파일을 작성하십시오.  Nginx 웹 사이트 컨텐츠의 기본 위치는 `/usr/share/nginx/html/`입니다.  이 예제의 경우 인증 확인 응답은 `/usr/share/nginx/html/.well-known/acme-challenge/` 디렉토리에 있습니다.
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **3단계:** 필요하면 Nginx 서버 구성 파일을 여십시오. `/etc/nginx/nginx.conf`와 `/etc/nginx/conf.d/`는 구성 파일의 기본 위치입니다.

  * **4단계:** 필요하면 CDN 도메인을 추가 **server_name**으로 원본의 서버 블록에 추가하십시오.

  * **5단계:** Nginx 서버 구성을 수정해야 하는 경우 다음 명령을 사용하여 Nginx 서버를 다시 시작하고 가동 중단 시간을 최소화하십시오.

      ```
nginx -s reload
      ```

  * **6단계:** CDN 도메인과 원본 서버의 IP 주소 사이의 DNS에 A 레코드를 작성하십시오.

#### 도메인 유효성 검증을 처리하는 이 표준 메소드가 CA에 준비되었는지 확인하십시오.
{: #verify-that-this-standard-method-to-address-domain-validation-is-ready-for-the-ca}

* 이 메소드가 `curl`을 통해 작동하는지 확인하려면 인증 확인 URL의 명령을 실행하십시오.
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* 이 메소드가 브라우저를 통해 작동하는지 확인하려면 브라우저에서 인증 확인 URL에 액세스하십시오.

두 경우 모두 원본 서버에 저장된 도메인 유효성 검증 인증 확인 파일 오브젝트의 사본을 검색할 수 있어야 합니다.

#### 표준 메소드 정리
{: #clean-up-for-the-standard-method}

CDN이 **인증서 배치 중** 상태가 되면 다음을 수행하십시오.
1. `examplechallenge-fileobject` 파일을 제거하십시오 (선택사항).
1. 필요한 경우 추가된 ServerAlias(Apache2) 또는 server_name(Nginx)을 서버 구성에서 제거하십시오 (선택사항).
1. CDN 도메인과 원본 서버 IP 사이의 A 레코드를 제거하십시오.

---
### 경로 재지정
{: #redirect}

**경로 재지정** 탭을 클릭하면 경로 재지정을 통해 도메인 유효성 검증을 처리하는 데 필요한 모든 정보가 표시됩니다. 이 정보를 사용하면 CA에서 원본 서버를 통해 Akamai에서 **인증 확인 응답**의 사본을 검색할 수 있습니다. 서버가 올바르게 구성되고 나면 도메인 유효성 검증을 수행하는 데 2 - 4시간이 걸릴 수 있습니다.

   ![도메인 유효성 검증 인증 확인 경로 재지정](images/domain-validation-redirect.png)

경로 재지정 메소드를 통해 도메인 유효성 검증을 완료하려면 특정 방식으로 웹 서버를 구성해야 할 수도 있습니다. Apache 및 Nginx 서버에 대한 예제 프로시저가 다음 섹션에 요약되어 있습니다.

**예제 환경**
* 원본 서버: `www.example.com`
* CDN 도메인: `cdn.example.com`
* 인증 확인 URL: `http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* URL 경로 재지정: `http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Apache 경로 재지정 구성
{: #apache-redirect-configuration}

  * **1단계:** Apache2 서버를 실행 중인 시스템에 로그인하십시오.

  * **2단계:** Apache2 서버 구성 파일을 여십시오. `/etc/apache2/apache2.conf` 및 `/etc/apache2/sites-enabled/`는 구성 파일의 기본 위치입니다.

  * **3단계:** 구성 파일의 적절한 위치에 경로 재지정 명령문을 추가하십시오. 필요하면 CDN 도메인을 추가 **ServerAlias**로 원본의 가상 호스트에 추가하십시오.

    ```
    Redirect /.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **4단계:** 다음 명령을 사용하여 Apache2 서버를 다시 시작하고 가동 중단 시간을 최소화하십시오.

    ```
apachectl -k graceful
    ```

  * **5단계:**
CDN 도메인과 원본 서버의 IP 주소 사이의 DNS에 A 레코드를 작성하십시오.

#### Nginx 경로 재지정 구성
{: #nginx-redirect-confguration}

  * **1단계:** Nginx 서버를 실행 중인 시스템에 로그인하십시오.

  * **2단계:** Nginx 서버 구성 파일을 여십시오. `/etc/nginx/nginx.conf` 및 `/etc/nginx/conf.d/`는 구성 파일의 기본 위치입니다.

  * **3단계:** 이 단계에서 사용할 수 있는 메소드는 두 가지가 있습니다.

    * 옵션 1: (권장) `return` 지시문이 있는 `location` 블록을 추가하여 적절한 `server` 블록의 지시문을 수행하여 경로를 재지정하십시오. 필요하면 CDN 도메인을 추가 **server_name**으로 원본의 서버 블록에 추가하십시오.

    ```
server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives
      # ...
    }
    ```

   * 옵션 2: `server` 블록에 `rewrite` 지시문을 추가하십시오. 필요하면 CDN 도메인을 추가 **server_name**으로 원본의 서버 블록에 추가하십시오.

    ```
server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives
      # ...
    }
    ```

  * **4단계:** 다음 명령을 사용하여 Nginx 서버를 다시 시작하고 가동 중단 시간을 최소화하십시오.

    ```
nginx -s reload
    ```

  * **5단계:** CDN 도메인과 원본 서버의 IP 주소 사이의 DNS에 A 레코드를 작성하십시오.

#### 경로 재지정이 수행되는지 확인
{: #verify-that-the-redirect-is-occurring}

이 단계를 완료하면 특정 인증 유형 URL에 대한 트래픽_만_ URL 경로 재지정으로 경로가 재지정됩니다. `curl` 또는 브라우저를 통해 경로 재지정이 제대로 작동하는지 확인할 수 있습니다.

* `curl`을 통해 경로 재지정이 작동하는지 확인하려면 인증 확인 URL의 명령을 실행하십시오.

    ```
curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* 경로 재지정이 작동하는지 브라우저를 통해 확인하려면 브라우저에서 인증 확인 URL에 접속하십시오.

두 경우 모두, 원본 요청의 경로가 재지정되는 `dcv.akamai.com`의 Akamai에서 도메인 유효성 검증 인증 확인 파일 오브젝트의 사본을 검색할 수 있어야 합니다.

#### 경로 재지정 메소드 정리
{: #clean-up-for-the-redirect-method}

CDN이 **인증서 배치 중** 상태가 되면 다음을 수행하십시오.
1. 구성 파일에서 경로 재지정 명령문 또는 블록을 제거하십시오 (선택사항).
1. 필요한 경우 추가된 ServerAlias(Apache2) 또는 server_name(Nginx)을 서버 구성에서 제거하십시오 (선택사항).
1. CDN 도메인과 원본 서버 IP 사이의 A 레코드를 제거하십시오.
