---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 알려진 제한사항

다음 제한사항은 Akamai 벤더를 위한 새 CDN 서비스에 적용됩니다.
* 디렉토리 레벨 컨텐츠 또는 다중 파일의 제거는 현재 지원되지 않습니다.
* CDN당 총 원본 수는 75개로 제한됩니다.
* DV SAN 인증서를 사용하는 HTTPS는 새로운 CDN에만 사용할 수 있습니다.
* 핫 링크 보호는 `://`를 포함하는 첫 번째 `.` 문자 앞의 문자 세트가 있는 `refererValues` URL 일치 용어를 지원하지 않습니다. 예를 들어, `http://www.example.com` 또는 `www.example.com http://www.example.com`입니다.
