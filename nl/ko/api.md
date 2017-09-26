---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# API 참조

구성에 관련된 PHP 및 SOAP에 대한 자세한 정보를 보려면 https://sldn.softlayer.com/article/PHP를 방문하십시오.  
 
## 계정에 대한 API
### verifyCdnAccountExists
CDN 계정이 API를 호출하는 사용자를 위해 존재하는지 아니면 제공된 `vendorName`을 위해 존재하는지를 확인합니다.

 * **매개변수**: `vendorName`
 * **리턴**: `true`(계정이 존재하는 경우), `false`(계정이 존재하지 않는 경우)
___

## 도메인 맵핑에 대한 API
### createDomainMapping
이 기능은 제공된 입력을 사용하여 제공된 벤더를 위해 도메인 맵핑을 작성하고 이를 사용자의 {{site.data.keyword.BluSoftlayer_notm}} 계정 ID와 연관시킵니다. 작업할 이 API에 대해 `createCustomerSubAccount`를 사용하여 CDN 계정을 작성해야 합니다. CDN을 작성한 후 `defaultTTL`은 3600초의 값으로 작성됩니다. 

 * **매개변수**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input`인 콜렉션. 콜렉션에는 `vendorName`, `hostname`, `protocol`, `originType`, `originHost`, `originHostPort`, `respectHeader`, `serveStale`, `cname`, `performanceConfiguration`, `header`, `certificateType`, `path`의 항목이 포함되어야 합니다.
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션. 콜렉션은 맵핑에 관련된 후속 API에 대해 입력으로 전송되어야 하는 `uniqueId` 값을 제공합니다. 
___ 
### deleteDomainMapping
`uniqueId`를 기반으로 한 도메인 맵핑을 삭제합니다. 도메인 맵핑의 상태는 _RUNNING_, _STOPPED_, _DELETED_, _ERROR_, _CNAME\_CONFIGURATION_ 또는 _SSL\_CONFIGURATION_ 중 하나여야 합니다.

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션
___
### verifyDomainMapping
CDN의 상태를 확인하고 필요한 경우 `status`, `cname` 및/또는 `vendorCname`을 업데이트합니다. 해당하는 경우 업데이트된 값을 리턴합니다. 도메인 맵핑의 상태는 _RUNNING_, _CNAME\_CONFIGURATION_ 또는 _SSL\_CONFIGURATION_ 중 하나여야 합니다. 

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션
___ 
### startDomainMapping
`uniqueId`를 기반으로 한 CDN 도메인 맵핑을 시작합니다. 시작하려면 도메인 맵핑의 상태는 _STOPPED_여야 합니다. 

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션
___ 
### stopDomainMapping
`uniqueId`를 기반으로 한 CDN 도메인 맵핑을 중지합니다. 중지를 시작하려면 도메인 맵핑의 상태는 _RUNNING_이어야 합니다. 

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션
___ 
### updateDomainMapping
사용자가 `uniqueId`로 식별된 맵핑의 특성을 업데이트하도록 합니다. `originHost`, `performanceConfiguration`, `header`, `httpPort`, `httpsPort`, `certificateType`, `respectHeader`, `serveStale`, `path` 필드가 변경될 수 있으며 원본 유형이 오브젝트 스토리지인 경우 `bucketName` 및 `fileExtension`도 변경될 수 있습니다. 업데이트가 실행되려면 도메인 맵핑의 상태는 _RUNNING_이어야 합니다. 

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 콜렉션
___
### listDomainMappings
특정 고객을 위해 모든 도메인의 콜렉션을 리턴합니다. 

 * **매개변수**: _none_ 
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 오브젝트의 콜렉션
___ 
### listDomainMappingByUniqueId
CDN의 `uniqueId`를 기반으로 한 단일 도메인 오브젝트를 사용하여 콜렉션을 리턴합니다.

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Mapping`인 오브젝트의 단일 오브젝트 콜렉션
___

## 제거에 대한 API
### createPurge
제거 레코드를 작성하고 데이터베이스에 이를 삽입합니다. 

 * **매개변수**: `string` `uniqueId`, `string` `path`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`인 오브젝트의 콜렉션
___
### getPurgeHistoryPerMapping
`uniqueId` 및 `saved` 상태를 기반으로 한 CDN의 제거 히스토리를 리턴합니다. (기본적으로, `saved`의 값은 _unsaved_로 설정됩니다.)

 * **매개변수**: `string` `uniqueId`, `int` `saved`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`인 오브젝트의 콜렉션
___
### saveOrUnsavePurgePath
제거 경로를 저장해야 하는지 여부를 표시하도록 제거 경로의 상태를 업데이트합니다. 제거 경로가 저장되면 새 `saved` 제거를 작성합니다. 경로가 `unsaved`이면 저장된 제거 레코드를 삭제합니다.

 * **매개변수**: `string` `uniqueId`, `string` `path`, `int` `saved`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Cache_Purge`인 오브젝트의 콜렉션
___

## TTL(Time to Live)에 대한 API
### createTimeToLive
새 `TimeToLive` 오브젝트를 작성하고 데이터베이스에 이를 삽입합니다. 

 * **매개변수**: `string` `uniqueId`, `string` `path`, `int` `ttl`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`인 오브젝트
___
### updateTtl
기존 `TimeToLive` 오브젝트를 업데이트합니다. _oldTtl_ 및 _newTtl_ 입력이 동일하면 조기에 종료됩니다. 

 * **매개변수**: `string` `uniqueId`, `string` `oldPath`, `string` `newPath`, `int` `oldTtl`, `int` `newTtl`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`인 오브젝트
___
### deleteTtl
데이터베이스에서 기존 `TimeToLive` 오브젝트를 삭제합니다. 

 * **매개변수**: `string` `uniqueId`, `string` `pathName`
 * **리턴**: 삭제 상태의 문자열
___
### listTtl
CDN의 `uniqueId`를 기반으로 한 기존 `TimeToLive` 오브젝트를 나열합니다.

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Network_CdnMarketplace_Configuration_Cache_TimeToLive`인 오브젝트의 배열
___

## 원본에 대한 API
### createOriginPath
기존 CDN과 특정 고객을 위해 원본 경로를 작성합니다. 원본 경로는 호스트 서버 또는 오브젝트 스토리지를 기반으로 할 수 있습니다. 원본 경로를 작성하려면 도메인 맵핑의 상태는 _RUNNING_ 또는 _CNAME\_CONFIGURATION_ 중 하나여야 합니다.   

 * **매개변수**: `domainName`, `vendorName`, `path`, `originType` 및 `origin`의 특성이 설정되도록 예상하는 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 오브젝트. 원본 유형이 서버인 경우 `httpPort` 및/또는 `httpsPort`도 설정해야 합니다. 원본 유형이 오브젝트 스토리지인 경우 `fileExtension`(선택적)과 함께 `bucketName`도 제공해야 합니다.   
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`인 오브젝트의 콜렉션

___ 
### updateOrigin
기존 맵핑과 특정 고객을 위해 기존 원본 경로를 업데이트합니다. `originType`을 변경할 수 없습니다. `path`, `origin`, `httpPort` 및 `httpsPort`의 특성만 변경할 수 있습니다. 업데이트하려면 도메인 맵핑의 상태는 _RUNNING_ 또는 _CNAME\_CONFIGURATION_ 중 하나여야 합니다. 

 * **매개변수**: `domainName`, `vendorName`, `path`, `originType` 및 `origin`의 특성이 설정되도록 예상하는 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Input` 오브젝트. 원본 유형이 서버인 경우 `httpPort` 및/또는 `httpsPort`도 설정해야 합니다. 경로의 원본 유형이 오브젝트 스토리지인 경우 `bucketName`을 제공해야 하고 선택적으로 `fileExtension`을 제공해야 할 수 있습니다.   
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`인 오브젝트의 콜렉션
___ 
### deleteOriginPath
기존 CDN과 특정 고객을 위해 기존 원본 경로를 삭제합니다. 삭제하려면 도메인 맵핑의 상태는 _RUNNING_ 또는 _CNAME\_CONFIGURATION_ 중 하나여야 합니다.   

 * **매개변수**: `string` `uniqueId`, `string` `path`
 * **리턴**: 상태 메시지(삭제된 경우), 예외(삭제되지 않은 경우)

___
### listOriginPath
기존 맵핑과 특정 고객을 위해 원본 경로를 나열합니다. 

 * **매개변수**: `string` `uniqueId`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path`인 오브젝트의 콜렉션
___

## 메트릭에 대한 API
### getCustomerUsageMetrics
지정된 기간 동안 고객 계정의 직접적인 표시(그래프 없음)를 위해 미리 결정된 통계의 총 수를 리턴합니다. 

 * **매개변수**: `string` `vendorName`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___ 
### getMappingUsageMetrics
지정된 맵핑의 직접적인 표시를 위해 미리 결정된 통계의 총 수를 리턴합니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___ 
### getMappingHitsMetrics
지정된 시간 범위 동안 특정 빈도로 도메인 맵핑당 히트의 총 수를 리턴합니다. 빈도는 '일', '주' 및 '월'이 될 수 있습니다. 여기서, 그래프의 각 간격은 하나의 플롯 점입니다. 리턴 데이터는 `startDate`, `endDate` 및 `frequency`에 따라 정렬됩니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingHitsByTypeMetrics
지정된 시간 범위 동안 특정 빈도로 히트의 총 수를 리턴합니다. 빈도는 '시간', '일', '주' 및 '월'이 될 수 있습니다. 여기서, 그래프의 각 간격은 하나의 플롯 점입니다. 리턴 데이터는 `startDate`, `endDate` 및 `frequency`에 따라 정렬되어야 합니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingBandwidthMetrics
개별 CDN에 대해 영역당 에지 히트의 수를 리턴합니다. 영역은 각 벤더별로 다를 수 있습니다. 맵핑당입니다. 

 * **매개변수**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
### getMappingBandwidthByRegionMetrics
지정된 기간 동안 고객 계정의 직접적인 표시(그래프 없음)를 위해 미리 결정된 통계의 총 수를 리턴합니다. 기본적으로 `frequency`의 값은 'aggregate'입니다.

 * **매개변수**: `string` `mappingUniqueId`, `int` `startDate`, `int` `endDate`, `string` `frequency`
 * **리턴**: 유형이 `SoftLayer_Container_Network_CdnMarketplace_Metrics`인 오브젝트의 콜렉션
___
