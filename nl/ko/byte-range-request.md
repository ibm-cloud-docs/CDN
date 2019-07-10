---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: byte range request, byte-range request, origin server, range HTTP request, transfer-encoding

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}


# 바이트 범위 요청을 사용하여 작업
{: #working-with-byte-range-requests}

바이트 범위 요청을 사용하여 원본 서버에서 부분 컨텐츠를 검색할 수 있습니다. 이 문서를 사용하면 표시되는 응답 상태 코드를 이해할 수 있습니다.

{{site.data.keyword.cloud}} CDN with Akamai를 사용하여 **바이트 범위 요청**을 전송하는 경우, Akamai 에지 서버는 압축된 형식으로 원본에서 컨텐츠를 요청하므로 사용자가 첫 번째 요청에서 `200(확인)` 응답 코드를 받고 후속 요청 모두에 대해서는 `206` 응답 코드를 받을 수 있습니다. 따라서 에지 서버의 캐시에 오브젝트가 없거나 오브젝트의 컨텐츠 길이에 대한 정보가 없는 경우 이 에지 서버가 원본으로 이동하고 전체 오브젝트를 요청합니다. 이 경우 원본은 Akamai에 컨텐츠 길이 헤더가 없는 오브젝트를 제공하고, 바이트 범위 요청이더라도 일반 사용자에게 전체 오브젝트가 제공됩니다. 따라서 `200` 상태 코드가 표시됩니다. 후속 요청에서는 에지 서버 캐시에 오브젝트가 있으며 에지 서버가 `206` 상태 코드를 제공합니다.

**범위 HTTP 요청** 헤더는 서버가 리턴해야 하는 컨텐츠의 파트를 표시합니다. 한 번에 하나의 범위 헤더를 사용하여 여러 파트를 요청할 수 있으며 서버는 다중 파트 응답으로 이러한 범위를 반송할 수 있습니다. 서버가 범위를 반송하면 206(부분 컨텐츠) 상태로 응답합니다.

첫 번째 바이트 범위 요청에 대해서도 206 응답을 보장하는 한 가지 방법은 원본 서버에서 `전송-인코딩: 청크됨`을 사용 안함으로 설정하는 것입니다.
