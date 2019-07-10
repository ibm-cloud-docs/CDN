---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: origin, path, container, collection, attributes, query, arguments, class, API

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# 경로(원본) 컨테이너
{: #path-origin-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 콜렉션에는 (원본) 경로 API에서 이용하는 속성이 포함됩니다. 각 경로 API는 이 유형의 콜렉션을 리턴합니다.

**클래스** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`  

* `mappingUniqueId`: 이 원본 경로가 속한 맵핑의 고유 ID입니다.  
* `path`:  이 원본에 연결하는 데 사용할 수 있는 도메인의 상대 경로입니다.  
* `originType`: 원본 호스트의 유형(현재 ‘HOST\_SERVER’ 또는 ‘OBJECT\_STORAGE’)입니다.  
* `httpPort`: HTTP 프로토콜에 사용되는 포트의 번호입니다.  
* `httpsPort`: HTTPS 프로토콜에 사용되는 포트의 번호입니다.  
* `status`: (원본) 경로의 상태입니다. 올바른 상태는 _CREATING_, _STARTING_, _RUNNING_, _UPDATING_, _STOPPING_ 및 _DELETING_입니다.
* `fileExtension`: 캐시할 수 있는 파일 확장자입니다.  
* `header`: 원본 서버에서 사용되는 호스트 헤더 정보를 지정합니다.
* `cacheKeyQueryRule`: 다음 옵션은 캐시 키 동작을 구성하는 데 사용 가능합니다.
  * `include-all`: 모든 조회 인수를 포함합니다. **기본값**
  * `ignore-all`: 모든 조회 인수를 무시합니다.
  * `ignore: space separated query-args`: 특정 조회 인수를 무시합니다. 예를 들어, "ignore: query1 query2"입니다.
  * `include: space separated query-args`: 특정 조회 인수를 포함합니다. 예를 들어, "include: query1 query2"입니다.
