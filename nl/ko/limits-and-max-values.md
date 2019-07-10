---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: limits, maximum, values, time to live, entries, large file, size, optimization, downloads, years

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 한계 및 최대값
{: #limits-and-maximum-values}

## TTL(Time To Live)의 최대값이 있습니까? 최소값은 있습니까?
{: #is-there-a-maximum-value-for-time-to-live}

TTL(Time To Live)의 최대값은 2,147,483,647초이며, 이 값은 대략 68년에 해당합니다. 최소값은 0초입니다.

## 원본 및 TTL 항목의 수에 대한 제한이 있습니까?
{: #is-there-a-limit-on-the-number-of-origin-and-ttl-entries}

예. 결합된 제한은 CDN당 75개의 항목입니다.

## Akamai CDN을 통해 전달할 수 있는 가장 큰 파일 크기는 얼마입니까?
{: #what-is-the-largest-file-size-that-can-be-delivered-through-akamai-cdn}

1.8GB보다 큰 파일을 검색하거나 전달하려고 시도하면 기본 성능 구성에 대해 `403 Access Forbidden` 응답이 수신됩니다. 대형 파일 최적화를 사용하면 최대 320GB의 파일 다운로드가 가능합니다.

## 원본 응답 헤더의 최대 크기는 얼마입니까?
{: #what-is-the-maximum-size-for-the-orignin-response-headers}

최대 응답 헤더 크기는 16KB입니다. 특정 원본이 후속 요청에서 클라이언트가 리턴하기에 너무 큰 쿠키를 설정하려고 시도하는 경우 Akamai가 에지 서버에서 이 한계를 설정합니다. 응답 헤더가 16KB보다 크면 요청이 `502 Bad Gateway` 응답을 받습니다.

## 클라이언트 요청 헤더의 최대 크기는 얼마입니까?
{: #what-is-the-maximum-size-for-the-client-request-headers}

최대 요청 헤더 크기는 32KB입니다. 요청 헤더가 32KB보다 크면 Akamai 에지 서버가 `400 Bad Request` 응답을 리턴합니다.
