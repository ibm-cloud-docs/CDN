---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

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

# 규칙 및 이름 지정 규칙
{: #rules-and-naming-conventions}

## CDN 호스트 이름에 대한 규칙은 무엇입니까?
{: #what-are-the-rules-for-the-cdn-hostname}

CDN `Hostname` 입력 문자열은 다음과 **같아야 합니다**.
  * 영숫자 문자로 구성
  * 254자 미만임
  * 올바른 최상위 레벨 도메인 이름으로 끝남
  * 10개가 넘는 레이블을 포함하지 **않아야** 함
  * `cdnedge.bluemix.net`(CNAMES에 사용되고 예약되어 있음)으로 끝나지 **않아야** 함

세부사항은 RFC 1035, 섹션 2.3.4를 참조하십시오. 

또한 CDN 호스트 이름으로 완전한 도메인 이름을 사용하는 것이 좋습니다. `example.com` 양식의 루트 도메인(Zone Apex 또는 Naked domain이라고도 함) 이름보다는 `www.example.com` 양식의 이름을 선택하십시오. 사용하는 CDN 호스트 이름에 대한 CNAME 레코드를 작성해야 하며, DNS RFC 1033은 CNAME이 아닌 A 레코드가 될 루트 도메인 레코드가 필요합니다. RFC 2181, 섹션 10.1에 추가 설명이 제공됩니다.

## 사용자 정의 CNAME 이름 지정 규칙은 무엇입니까?
{: #what-are-the-custom-cname-naming-conventions}

`CNAME` 입력 문자열은 다음 규칙을 따라야 합니다.
  * 고유**해야 함**(다른 IBM Cloud CDN에서 사용할 수 없음)
  * 64자 미만의 영숫자 문자임
  * 특수 문자 `! @ # $ % ^ & *`가 허용되지 **않음**
  * 밑줄 `_`이 허용되지 **않음**
  * 마침표 `.`가 허용되지 **않음**
  * 대시 `-`가 허용되지만 CNAME을 대시로 시작하거나 끝낼 수 없음

## 버킷 이름에 대한 규칙은 무엇입니까?
{: #what-are-the-rules-for-bucket-names}

버킷 이름:
  * 한 자 이상**이어야** 함
  * 200자 미만이어야 함
  * 문자(대문자와 소문자 둘 다 허용됨), 숫자 및 하이픈을 포함할 수 있음
  * IP 주소(예: 127.0.0.1)로 형식화하면 **안 됨**
  * 마침표(.)로 시작할 수 **없음**
  * 마침표(.)로 끝날 수 있음
  * 레이블 사이에 하나의 마침표(.)만 허용됨(예: my..ibmcloud.bucket은 허용되지 않음)

대문자와 마침표가 유효성 검증을 통과할 수는 있지만 항상 DNS 준수 버킷 이름을 사용하는 것이 좋습니다.
{: note}

## 원본의 경로 문자열에 대한 규칙은 무엇입니까?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

CDN 작성 시 경로는 선택사항입니다. 하지만 제공되는 경우 경로는 다음과 **같아야 합니다**.
  * 1,000자 미만임
  * `/`로 시작

## 영구 제거할 경로 문자열의 규칙은 무엇입니까?
{: #what-are-the-rules-for-the-path-string-for-purge}

영구 제거 경로:
  * 길이가 1,000자 미만이어야 함
  * `/`로 시작해야 함
  * `/`로 끝나야 함
  * 마침표(.)로 끝날 수 있음
  * `*`를 포함할 수 없음

영구 제거는 단일 파일에만 허용됩니다. 현재 디렉토리 레벨 영구 제거는 지원되지 않습니다.
{: note}

## **원본 추가** 명령의 경우, 경로 문자열에 대해 따라야 하는 규칙이 있습니까?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

**원본 추가**의 경우 경로는 **필수**입니다. 또한, CDN이 경로와 함께 작성된 경우 **원본 추가**의 경로는 접두부로서 CDN 경로로 시작해야 합니다. 예를 들어, CDN 경로가 `/storage`로 지정된 경우, `/examplePath`라는 경로와 함께 **원본 추가**를 호출하려면 제공되는 경로는 `/storage/examplePath`가 됩니다.

## 파일 확장자를 제공하는 규칙은 무엇입니까?
{: #what-are-the-rules-for-providing-file-extensions}

Object Storage로 원본을 작성할 때 파일 확장자는 쉼표로 구분해야 합니다. 예를 들어 `txt, jpg, xml`은 올바른 목록입니다. 한 특정 확장자가 10자 이상일 수 없습니다.
