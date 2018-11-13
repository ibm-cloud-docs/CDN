---

copyright:
  years: 2018
lastupdated: "2018-10-04"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 한계 및 최대값

## TTL(Time To Live)의 최대값이 있습니까? 최소값은 있습니까?

TTL(Time To Live)의 최대값은 2,147,483,647초이며, 이 값은 대략 68년에 해당합니다. 최소값은 0초입니다.

## 원본 및 TTL 항목의 수에 대한 제한이 있습니까?

예. 결합된 제한은 CDN당 75개의 항목입니다.

## Akamai CDN을 통해 전달할 수 있는 가장 큰 파일 크기는 얼마입니까?

1.8GB보다 큰 파일을 검색하거나 전달하려고 시도하면 기본 성능 구성에 대해 `403 Access Forbidden` 응답이 수신됩니다. 대형 파일 최적화를 사용하면 최대 320GB의 파일 다운로드가 가능합니다.
