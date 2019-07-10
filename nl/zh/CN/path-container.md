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

# 路径（源）容器
{: #path-origin-container}

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` 集合包含（源）路径 API 利用的属性。每一个路径 API 都会返回此类型的集合。

`SoftLayer_Container_Network_CdnMarketplace_Configuration_Mapping_Path` **类**  

* `mappingUniqueId`：此源路径所属的映射的唯一标识  
* `path`：可用于访问此源的域的相对路径。  
* `originType`：源主机的类型，当前为“HOST\_SERVER”或“OBJECT\_STORAGE”。  
* `httpPort`：用于 HTTP 协议的端口号。  
* `httpsPort`：用于 HTTPS 协议的端口号。  
* `status`：（源）路径的状态。有效状态为：_CREATING_、_STARTING_、_RUNNING_、_UPDATING_、_STOPPING_ 和 _DELETING_。
* `fileExtension`：允许进行高速缓存的文件扩展名。  
* `header`：指定源服务器使用的主机头信息。
* `cacheKeyQueryRule`：以下选项可用于配置高速缓存键行为：
  * `include-all`：包含所有查询自变量（**缺省值**）
  * `ignore-all`：忽略所有查询自变量
  * `ignore: 空格分隔的查询自变量`：忽略这些特定查询自变量。例如，“ignore: query1 query2”
  * `include: 空格分隔的查询自变量`：包含这些特定查询自变量。例如，“include: query1 query2”
