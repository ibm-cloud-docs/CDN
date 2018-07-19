---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 限制和最大值

## 生存时间有最大值吗？有最小值吗？

生存时间的最大值为 2,147,483,647 秒，大概等于 68 年！最小值为 30 秒。

## 源和 TTL 条目的数目有限制吗？

有，组合限制为每个 CDN 75 个条目。

## 我可以拥有的 CDN 的数目有限制吗？

有，每个帐户的 CDN 数限制为 10。

## 可以同时处于活动状态的清除请求数目有限制吗？
有。一次只能有 5 个活动的清除请求。

## 可以通过 Akamai CDN 传递的最大文件大小是多少？

对于缺省性能配置，尝试检索或传递大于 1.8 GB 的文件时，会收到 `403 访问被拒绝`响应。如果启用了“大型文件优化”，那么可以下载最大 320 GB 的文件。
