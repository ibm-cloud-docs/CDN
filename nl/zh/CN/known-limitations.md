---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 已知限制

以下限制适用于 Akamai 供应商的新 CDN 服务：
* HTTPS 目前仅通过通配符证书进行支持。
* 目前不支持目录级别内容或多个文件的清除。
* 目前给定 {{site.data.keyword.BluSoftlayer_notm}} 帐户限制为 10 个 CDN。
* 每个 CDN 的源和 TTL 条目的总数限制为 75。
* “提供旧内容选项”功能将始终**开启**，即使在创建 CDN 时其为**关闭**也是如此 
* 如果 CDN 是使用**服务器**和 **HTTP 端口**创建的，那么只能使用**服务器**选项添加源。
