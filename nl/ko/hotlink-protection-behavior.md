---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-25"

keywords: hotlink, protection, class, behavior, API, valid string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 핫 링크 보호 클래스
{: #hotlink-protection-class}

`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection 클래스`에는 핫 링크 보호 API에서 이용하는 속성이 포함됩니다. 이 오브젝트는 API를 호출하여 CDN에 대한 핫 링크 보호 동작을 설정하는 데 사용됩니다.  또한 API 호출이 성공한 후에 핫 링크 보호 API에 의해 리턴됩니다.

**class** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`:

* URL의 첫 번째 레이블에서 `://`는 지원되지 **않습니다**.
   * **유효함**: `http*www.example.com`
   * **유효하지 않음**: `http://www.example.com`

* `protectionType`: HTTP 요청에 `refererValues`의 용어 중 하나와 일치하는 Referer Header 값이 있는 경우에 컨텐츠에 대한 액세스를 허용 또는 거부하도록 지정합니다. 일치하지 않는 경우에는 반대 현상이 발생합니다.
  * protectionType에 대해 가능한 값:
    * `ALLOW`
    * `DENY`
* `refererValues`: 단일 공백으로 분리된 referer URL의 목록이며 문자열 양식에서 일치하는 와일드카드를 가질 수도 있습니다.
  * `refererValues`에 대해 **유효한** 문자열의 예는 다음과 같습니다.
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * `refererValues`에 대해 **유효하지 않은** 문자열의 예는 다음과 같습니다.
  

| **유효하지 않은 예제에 대한 설명** |예제 |
| -------|-----|
| 총 2100자를 초과하는 문자 포함 |`www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
| 255자를 초과하는 하나 이상의 URL 일치 용어 포함 |`www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
| 목록에 중복 포함 |`domain1.example.com domain1.example.com`|
| 비어 있는 refererValues | ` `|
| [RFC-3986 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://tools.ietf.org/html/rfc3986#section-2)에 지정되지 않은 문자 사용 |`domain1.exa}mple.com domain1.example.com`|
| `&` 문자가 지원되지 않음|`www.example.com/path&`|
| `://`를 포함하는 첫 번째 `.` 문자 앞의 문자 세트가 포함된 URL 일치 용어가 하나 이상인 `refererValues` 문자열이 지원되지 않음|`www.example.org http://www.example.com`|


