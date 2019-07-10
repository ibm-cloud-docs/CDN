---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: troubleshooting, support, reference, number, error, 503, 301, redirects, https, moved, akamai-x-cache, cloud object storage

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 문제점 해결
{: #troubleshooting}

이 문서에서 {{site.data.keyword.cloud}} CDN의 문제점을 해결하는 여러 방법을 확인할 수 있습니다. 지원 팀에 문의해야 하는 경우 CDN 참조 번호를 제공해야 합니다.

## 내 CDN이 작동 중인지 확인하려면 어떻게 해야 합니까?
{: #how-do-I-know-my-cdn-is-working}

`http://your.cdn.domain/uri`를 CDN의 각 파일 경로로 대체하여 다음 `curl` 명령을 실행하십시오.

`curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" http://your.cdn.domain/uri`

`curl` 명령의 출력이 다음 예제 형식과 유사한 경우 CDN은 예상대로 작동 중입니다.

```
    HTTP/1.1 200 OK

    Server: nginx/1.13.0

   ...

    X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

    X-Cache-Key: /L/1363/535014/1d/your.cdn.domain/uri

    X-True-Cache-Key: /L/your.cdn.domain/uri

    ...

    ...

    X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

    X-Serial: 1363

    Connection: keep-alive

    X-Check-Cacheable: YES
```
{: screen}

## 503 오류가 발생했습니다. 이유는 무엇입니까?
{: #i-received-a-503-error-why}

503 오류에 대해 확인된 가장 일반적인 이유는 SSL 인증서 체인에 있는 인증서 문제 때문입니다.

사용자에게 표시될 수 있는 오류는 `503 Service Unavailable`입니다.  

503 오류와 함께 다음과 유사한 메시지가 표시될 수도 있습니다. `An error occurred while processing your request. Reference #30.3598c0ba.1521745157.87201fff`(실제 참조 번호는 다를 수 있음). 이 경우 오류 문자열의 참조 번호는 SSL 핸드쉐이크 실패를 나타냅니다.

이 문제를 해결하려면, 원본 서버의 SSL 인증서가 다음 기준을 충족하는지 확인하십시오.
  * 인증서는 Akamai에서 신뢰하는 인증 기관에서 발행**되어야** 합니다. Akamai에서 신뢰할 수 있는 인증서의 목록은 [이 링크 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates)에서 볼 수 있습니다.
  * CDN에 구성된 *호스트 헤더*와 일치**해야** 합니다.
  * 자체 서명되지 **않아야** 합니다.
  * 만료되지 **않아야** 합니다.

이전 기준을 사용하여 원본의 인증서 체인을 확인했으나 동일한 오류가 계속 발생하는 경우 [도움 및 지원 받기](/docs/infrastructure/CDN?topic=CDN-gettinghelp) 페이지를 참조하십시오. 참조 오류 문자열을 적어 놓고 IBM과의 커뮤니케이션이 있는 경우 제공하십시오.

## IBM Cloud Object Storage(COS)가 원본인 경우 내 호스트 이름이 브라우저에 로드되지 않습니다.
{: #my-hostname-doesnt-load-on-the-browser-when-ibm-cloud-object-storage-cos-is-the-origin}

{{site.data.keyword.cloud_notm}} CDN이 COS를 Object Storage로 사용하도록 구성되어 있는 경우 웹 사이트에 대한 직접 액세스는 작동하지 않습니다. 브라우저의 주소 표시줄에 전체 요청 경로를 지정해야 합니다(예: `www.example.com/index.html`). 이 동작은 IBM COS의 인덱스 문서 제한사항으로 인해 발생합니다.

## HTTPS가 포함된 호스트 이름을 사용하여 브라우저 또는 `curl` 명령을 통해 연결할 수 없습니다.
{: #i-cant-conect-through-a-curl-command-or-browser-using-the-hostname-with-https}

CDN이 와일드카드 인증서로 HTTPS를 사용하여 작성된 경우 CNAME을 사용하여 연결해야 합니다(예: `https://www.exampleCname.cdnedge.bluemix.net`). 여기에는 2018년 6월 18일 이전에 HTTPS를 사용하여 작성된 **모든** CDN이 포함됩니다. 호스트 이름을 사용하여 연결을 시도하면 오류가 발생합니다.

## 지원되는 프로토콜에 대해 브라우저에서 CNAME 또는 호스트 이름을 로드할 때 예상되는 동작은 무엇입니까?
{: #what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols}

이 표는 웹 브라우저에서 **호스트 이름** 또는 **CNAME** 중 하나를 로드할 때 지원되는 프로토콜에 대한 예상 동작을 표시합니다.

<table>
<caption caption-side=“top”>예상 동작 표</caption>
<thead>
<tr>
<th rowspan=2 scope="col">브라우저 URL</th>
<th rowspan=2 scope="col">CDN (HTTP 프로토콜 전용)</th>
<th colspan=2 scope="col">CDN (HTTPS 프로토콜 전용)</th>
<th colspan=2 scope="col">CDN (HTTP 및 HTTPS 프로토콜 둘 다 가능)</th>
</tr>
<tr>
<th scope="col"> 와일드카드 </th>
<th scope="col"> 공유 SAN </th>
<th scope="col"> 와일드카드 </th>
<th scope="col"> 공유 SAN </th>
</tr>
</thead>
<tbody>
<tr>
<td> `http://hostname` </td>
<td> 로드 성공 </td>
<td> 301 Moved permanently </td>
<td> 로드 성공 </td>
<td> 301 Moved permanently </td>
<td> 로드 성공 </td>
</tr>
<tr>
<td> `https://hostname`</td>
<td> Access denied </td>
<td> IBM Cloud 웹 페이지로 경로 재지정 </td>
<td> 로드 성공 </td>
<td> IBM Cloud 웹 페이지로 경로 재지정 </td>
<td> 로드 성공 </td>
</tr>
<tr>
		<td> `http://cname` </td>
		<td> 301 Moved permanently </td>
		<td> 로드 성공 </td>
		<td> 301 Moved permanently </td>
		<td> 로드 성공 </td>
		<td> 301 Moved permanently </td>
</tr>
<tr>
		<td> `https://cname` </td>
		<td> IBM Cloud 웹 페이지로 경로 재지정 </td>
		<td> 로드 성공 </td>
		<td> 301 Moved permanently </td>
		<td> 로드 성공 </td>
		<td> IBM Cloud 웹 페이지로 경로 재지정 </td>
</tr>
</tbody>
</table>

**공통 오류 메시지:**

`301 Moved permanently` 메시지는 호스트 이름을 사용하여 `HTTPS` 또는 `HTTP_AND_HTTPS` 프로토콜로 CDN에 접속하려고 시도하고 있음을 나타낼 수 있습니다. HTTPS 와일드카드 인증서의 제한사항으로 인해 CDN에 액세스하기 위해서는 CNAME을 사용**해야** 합니다.

HTTP **전용** 프로토콜을 사용하면 CNAME을 사용하여 CDN에 접속하려고 시도하는 경우 `301 Moved permanently` 메시지를 수신합니다. 이 경우 호스트 이름을 사용하는 _경우에만_ CDN에 대한 액세스 권한을 얻을 수 있습니다.

올바르지 않은 프로토콜을 사용하여 CDN에 접속하려고 시도하는 경우 `Access denied` 메시지가 표시됩니다. HTTP 프로토콜을 사용하여 작성된 CDN에 `http`를 사용하고 HTTPS 프로토콜을 사용하여 작성된 CDN에 `https`를 사용하고 있는지 확인하십시오.

**가능 경로 재지정 오류:**

URL이 {{site.data.keyword.cloud_notm}} CDN 웹 페이지로 경로 재지정되는 동작은 프로토콜에 대해 URL이 올바르지 않은 경우에 자주 발생합니다. CDN이 HTTPS 또는 HTTPS_AND_HTTPS 프로토콜을 사용하여 작성되는 경우 CDN에 액세스하기 위해서는 CNAME을 사용해야 합니다. 예를 들어, HTTPS 맵핑의 경우 `https://examplecname.cdnedge.bluemix.net`이고 HTTP_AND_HTTPS 맵핑의 경우 `http://examplecname.cdnedge.bluemix.net` 또는 `https://examplecname.cdnedge.bluemix.net`입니다.

이 경우 URL이 {{site.data.keyword.cloud_notm}} CDN 웹 페이지로 경로 재지정되는 이유는 CDN 프로토콜에 대해 프로토콜과 도메인이 모두 올바르지 않기 때문입니다. HTTP _전용_ 프로토콜로 작성된 CDN의 경우 호스트 이름을 사용하는 _경우에만_ 접속할 수 있습니다. 예: `http://example.com`.
