---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-30"

keywords: limitations, Akamai, multiple files, purge, hotlink, limit

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# 알려진 제한사항
{: #known-limitations}

다음 제한사항은 Akamai 벤더를 위한 CDN 서비스에 적용됩니다.
* 디렉토리 레벨 컨텐츠 또는 다중 파일의 제거는 현재 지원되지 않습니다.
* CDN당 총 원본 수는 75개로 제한됩니다.
* DV SAN 인증서를 사용하는 HTTPS는 새로운 CDN에만 사용할 수 있습니다.
* 핫 링크 보호는 `://`를 포함하는 첫 번째 `.` 문자 앞의 문자 세트가 있는 `refererValues` URL 일치 용어를 지원하지 않습니다. 예를 들어, `http://www.example.com` 또는 `www.example.com http://www.example.com`입니다.
* 와일드카드 인증서를 사용한 HTTPS는 현재 지원되지 않습니다.
* 와일드카드 인증서 도메인의 경우 CDN 중지 기능이 지원되지 않습니다.

CDN 중지 기능은 7일을 초과하지 않는 유지보수 기간을 대상으로 합니다. 7일 후 CDN을 시작해야 합니다. 그렇지 않으면 CDN이 사용 안함으로 설정되고 CDN CNAME에 대한 트래픽이 원본 서버로 경로 재지정되지 않습니다.
{: important}
