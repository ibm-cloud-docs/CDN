---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: cache control, cache-control, cache duration, max-age,  edge server, edge-level, respect header, HTTP client

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# HTTP 클라이언트의 캐싱 기간을 제어하기 위해 캐시 제어 사용
{: #using-cache-control-to-control-an-http-client-s-cache-duration}

CDN을 사용할 때 두 가지 레벨의 캐싱을 사용할 수 있습니다.

  * **에지에서 캐싱**은 CDN 에지 서버에서 컨텐츠의 일부를 캐싱하는 경우에 발생합니다.
  * 서버의 에지 네트워크에서 **아래로 캐싱**은 일반 사용자 또는 HTTP 클라이언트(요청 브라우저 등)가 에지 서버에서 컨텐츠의 일부를 캐싱하는 경우에 발생합니다.

브라우저 등의 요청자에서 얼마나 오래 컨텐츠를 캐싱하는지 제어하기 위해 선택하는 방법은 다음 요소에 의해 결정됩니다.

  * [헤더 준수 설정](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn#updating-cdn-configuration-details)이 ON 또는 OFF인지 여부. 기본적으로 ON으로 설정됩니다.
  * 원본 서버가 특정 컨텐츠의 일부에 대한 Cache-Control 헤더에 `max-age` 값을 제공하는지 여부. 

이러한 요소가 어떻게 변경되는지에 상관없이, 에지 서버가 해당 컨텐츠에 대해 Cache-Control 헤더가 있는 HTTP 응답을 전송하도록 하려면 원본이 대상 컨텐츠에 대해 Cache-Control 헤더를 제공해야 합니다.

기본적으로 다운스트림 에지 서버에서 전송된 Cache-Control 헤더는 요청자가 캐싱 지시문 또는 에지 서버에 의해 지정된 값에 따라 연관된 컨텐츠를 캐싱하도록 요구합니다.

## 헤더 준수: 해제
{: #respect-header-off}

원본이 `max-age` 지시문 및 특정 컨텐츠의 일부에 대한 값이 있는 Cache-Control 헤더를 제공하는 경우, 에지에서 캐싱된 특정 컨텐츠의 일부에 대한 캐싱 기간이 여전히 CDN의 TTL 설정에서 파생됩니다. 또한 에지 서버는 다음 이하인 Cache-Control `max-age` 값을 사용하여 다운스트림 요청자에 반응합니다.
  * 원본의 Cache-Control `max-age` 값
  * 컨텐츠가 에지에서 손상되기까지 남아 있는 시간

그러나, 원본에서 에지 서버에 Cache-Control 헤더를 제공하지 않는 경우 에지 서버는 고유 Cache-Control 헤더를 요청자에 제공하지 않습니다. 컨텐츠에 대한 에지 캐시 기간은 여전히 사용자의 CDN의 TTL 설정에서 파생됩니다.

## 헤더 준수: 설정
{: #respect-header-on}

원본이 특정 컨텐츠의 일부에 대한 `max-age`가 있는 Cache-Control 헤더를 제공하는 경우, 원본의 Cache-Control `max-age` 값이 에지에서 캐싱된 특정 컨텐츠의 일부에 대한 캐싱 기간이 되고 해당 컨텐츠의 일부에 대해 적용되는 모든 TTL 설정을 대체합니다. 또한 에지는 컨텐츠가 에지 서버에서 손상되기 전까지 남아 있는 시간과 동일한 Cache-Control `max-age` 값을 사용하여 요청자에 반응합니다.

그러나, 원본에서 에지 서버에 Cache-Control 헤더를 제공하지 않는 경우 에지 서버는 고유 Cache-Control 헤더를 요청자에 제공하지 않습니다. 컨텐츠에 대한 에지 캐시 기간은 여전히 사용자의 CDN의 TTL 설정에서 파생됩니다.

## 요약
{: #summary}

|헤더 준수|원본에서 Cache-Control 제공|에지 서버에서 특정 컨텐츠의 캐시 기간|에지 서버에서 Cache-Control 제공|
|---|---|---|---|
|설정|예, 원본이 `max-age`를 지정함|원본의 `max-age` 값으로 대체된 에지 캐시 기간|예, 에지가 원본에서 컨텐츠를 새로 고쳐야 하기까지의 (대체된) 시간 값이 있는 `max-age`도 제공함|
|설정|예, 단, `max-age`를 지정하지 않음|CDN의 TTL 구성을 기반으로 하는 에지 캐시 기간|예, 에지가 원본에서 컨텐츠를 새로 고쳐야 하기까지의 시간 값이 있는 `max-age`도 제공함|
|설정|아니오|CDN의 TTL 구성을 기반으로 하는 에지 캐시 기간|아니오|
|해제|예, 원본이 `max-age`를 지정함|CDN의 TTL 구성을 기반으로 하는 에지 캐시 기간|예, 에지가 원본의 `max-age` 값 및 에지가 원본에서 컨텐츠를 새로 고쳐야 하기까지의 시간 미만인 `max-age` 값도 제공함|
|해제|예, 단, `max-age`를 지정하지 않음|CDN의 TTL 구성을 기반으로 하는 에지 캐시 기간|예, 에지가 원본에서 컨텐츠를 새로 고쳐야 하기까지의 시간 값이 있는 `max-age`도 제공함|
|해제|아니오|CDN의 TTL 구성을 기반으로 하는 에지 캐시 기간|아니오|

## 캐시 제어에 대한 자세한 정보
{: #more-information-on-cache-control}

* [CDN 관리](/docs/infrastructure/CDN?topic=CDN-manage-your-cdn) 방법
* [RFC 2616 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ietf.org/rfc/rfc2616.txt)의 섹션 14.9에 정의된 Cache-Control
