---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

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
* 每个 CDN 的源总数限制为 75 个。
* 使用 DV SAN 证书的 HTTPS 仅可用于新的 CDN。
* 热链接保护不支持 `refererValues` URL 匹配项，其位于第一个 `.` 字符前边的字符集包含 `://`，例如，`http://www.example.com` 或 `www.example.com http://www.example.com`
