---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# HTTPS 정보
{: #about-https}

{{site.data.keyword.cloud}}에서는 HTTPS로 CDN을 보호하는 두 가지 방법, 즉 와일드카드 인증서 및 도메인 유효성 검증(DV) SAN 인증서를 제공합니다. 두 HTTPS 옵션 모두 CDN을 구성할 때 **HTTPS 포트**를 선택하여 구성할 수 있습니다. 기본 HTTPS 포트는 443입니다. 또는 다른 포트 번호를 선택하여 HTTPS 트래픽을 라우팅할 수 있습니다. 허용된 포트 번호 목록은 [FAQ](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-)에서 찾을 수 있습니다.

HTTPS에 대해 **와일드카드 인증서** 및 **SAN 인증서** 사용 중에 결정하려면, CDN CNAME 또는 CDN 도메인 이름에서 HTTPS 트래픽을 제공하시겠습니까? 라는 질문에 답변하십시오. HTTPS 트래픽을 CNAME에서 제공하려는 경우 **와일드카드 인증서**를 선택하십시오. CDN 도메인 이름에서 HTTPS 트래픽을 제공하려는 경우에는 **SAN 인증서**를 선택하십시오.

## 와일드카드 인증서 지원
{: #wildcard-certificate-support}

**참고**:
와일드카드 인증서를 사용하는 새 CDN 맵핑은 현재 지원되지 않습니다.

와일드카드 인증서는 일반 사용자에게 웹 컨텐츠를 안전하게 전달하는 가장 간단한 방법입니다. 와일드카드 인증서를 사용하려면 와일드카드 인증서 접미부를 비롯한 전체 CDN CNAME을 서비스 시작점(예: `https://example.cdnedge.bluemix.net`)으로 사용**해야** 합니다.

IBM Cloud CDN에서는 와일드카드 인증서 `*.cdnedge.bluemix.net`을 사용합니다. 사용자를 위해 작성되었는지 아니면 사용자가 제공했는지에 상관없이 CNAME과 접미부 `*.cdnedge.bluemix.net`은 CDN 에지 서버에서 유지보수되는 와일드카드 인증서에 추가됩니다. 따라서 일반 사용자가 CDN의 HTTPS를 사용하기 위해서는 CNAME만 사용해야 합니다.

![HTTP 및 와일드카드 다이어그램](images/state-diagram-wildcard.png)

## SAN(Subject Alternate Name) 인증서 지원
{: #san-certificate-suport}

SAN(Subject Alternative Name) 인증서는 단일 인증서를 통해 여러 도메인 또는 호스트 이름을 보호할 수 있게 하는 디지털 SSL 인증서입니다.

HTTPS용 SAN 인증서를 사용하면 인증 기관에서 발행한 인증서에 기본 CDN 호스트 이름이 추가됩니다. 따라서 사용자가 CNAME이 아니라 호스트 이름을 통해 안전하게 서비스에 액세스할 수 있습니다(예: `https://www.example.com`).

HTTPS SAN 인증서를 사용하여 CDN을 주문하면 인증서를 요청하고 도메인 제어 유효성 검증(DCV)을 작성하는 프로세스를 진행합니다. DCV는 인증 기관에서 사용자가 도메인에 액세스하고 제어할 권한이 있도록 설정하는 데 사용하는 프로세스입니다. 이 단계를 완료하려면 조치가 필요합니다. 제어가 설정되고 나면 인증서가 전 세계 CDN 에지 서버에 배치됩니다. 인증서가 성공적으로 배치되고 나면 인증서 갱신이 자동으로 처리됩니다. 이 기능에 대한 자세한 정보는 [기능 설명](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support)에 있습니다. 도메인 제어 유효성 검증 메소드는 [HTTPS용 도메인 제어 유효성 검증 완료](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation) 페이지에서 자세히 설명합니다.

CDN이 RUNNING 상태에 도달하면, CDN 호스트 이름 CNAME 레코드를 DNS에 보관하십시오. CNAME 레코드가 제거되는 경우 3일 이내에 SAN 인증서에서 CDN 호스트 이름이 제거될 수 있습니다. 제거되는 경우 HTTPS 트래픽이 더 이상 해당 CDN 호스트 이름으로 제공되지 않습니다.
{:note}

![SAN 인증서를 사용하는 HTTPS 다이어그램](images/state-diagram-san.png)
