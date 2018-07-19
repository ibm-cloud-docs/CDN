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

# 已知限制

以下限制适用于 Akamai 供应商的新 CDN 服务：
* 目前不支持目录级别内容或多个文件的清除。
* 目前给定 {{site.data.keyword.BluSoftlayer_notm}} 帐户限制为 10 个 CDN。
* 每个 CDN 的源和 TTL 条目的总数限制为 75。
* “提供旧内容选项”功能将始终**开启**。
* 创建映射后，无法更改 HTTPS 证书类型，例如从通配符证书更改为 DV SAN 证书。
* 使用 DV SAN 证书的 HTTPS 仅可用于新的 CDN。
* 无法删除使用 DV SAN 证书创建的 CDN，除非它处于 RUNNING、CNAME_Configuration、CREATE_ERROR 或 DELETE_ERROR 状态。
