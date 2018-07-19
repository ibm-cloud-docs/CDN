---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-19"

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
* 제공된 {{site.data.keyword.BluSoftlayer_notm}} 계정에 현재 허용된 CDN은 10개로 제한됩니다.
* 원본 및 TTL 항목의 총 수는 CDN당 75개로 제한됩니다.
* 시간이 경과된(stale) 컨텐츠 제공 옵션 기능은 항상 **작동**입니다.
* 맵핑이 작성되고 나면(예: 와일드카드에서 DV SAN으로의 맵핑) HTTPS 인증서 유형을 변경할 수 없습니다.
* DV SAN 인증서를 사용하는 HTTPS는 새로운 CDN에만 사용할 수 있습니다.
* RUNNING, CNAME_Configuration, CREATE_ERROR 또는 DELETE_ERROR 상태에 있지 않으면 DV SAN 인증서로 작성한 CDN을 삭제할 수 없습니다.
