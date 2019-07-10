---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# 规则和命名约定
{: #rules-and-naming-conventions}

## CDN 主机名的规则是什么？
{: #what-are-the-rules-for-the-cdn-hostname}

CDN `主机名`输入字符串**必须**符合以下规则：
  * 由字母数字字符组成
  * 少于 254 个字符
  * 以有效的顶级域名结尾
  * **不能**包含 10 个以上标签
  * **不能**以 `cdnedge.bluemix.net` 结尾（此结尾用于 CNAMES，因而被保留）

请参阅 RFC 1035 第 2.3.4 部分以获取更多详细信息。 

此外，我们强烈建议使用标准域名作为 CDN 主机名。请选择格式为 `www.example.com` 的名称，而不是格式为 `example.com` 的根域名（也称为“区域顶点”或“裸域”）。您需要为使用的 CDN 主机名创建 CNAME 记录，并且 DNS RFC 1033 要求该根域记录作为 A 记录，而不是 CNAME。RFC 2181 的第 10.1 节中提供了进一步的阐述。

## 定制 CNAME 的命名约定是什么？
{: #what-are-the-custom-cname-naming-conventions}

`CNAME` 输入字符串必须符合以下规则：
  * **必须**唯一（不能由其他任何 IBM Cloud CDN 使用）
  * 少于 64 个字母数字字符
  * **不**允许使用特殊字符 `! @ # $ % ^ & *`
  * **不**允许使用下划线 `_`
  * **不**允许使用句点 `.`
  * 允许使用连字符 `-`，但 CNAME 不能以连字符开头或结尾

## 存储区名称的规则是什么？
{: #what-are-the-rules-for-bucket-names}

存储区名称：
  * **必须**至少为 1 个字符
  * 必须少于 200 个字符
  * 可以包含字母（允许使用大写和小写字母）、数字和连字符
  * 格式**不能**设置为 IP 地址（例如 127.0.0.1）
  * **不能**以句点 (.) 开头
  * 允许以句点 (.) 结尾
  * 标签之间只允许使用一个句点 (.)（例如，不允许使用 my..ibmcloud.bucket）。

虽然大写字母和句点可以通过验证，但我们建议您始终使用符合 DNS 标准的存储区名称。
{: note}

## 源的路径字符串的规则是什么？
{: #what-are-the-rules-for-the-path-string-for-the-origin}

创建 CDN 时，路径是可选的。但是，如果提供了路径，那么路径**必须**符合以下规则：
  * 长度少于 1000 个字符
  * 以 `/` 开头

## 清除的路径字符串的规则是什么？
{: #what-are-the-rules-for-the-path-string-for-purge}

清除路径：
  * 长度必须少于 1000 个字符
  * 必须以 `/` 开头
  * 不能以 `/` 结尾
  * 不能以句点 (.) 结尾
  * 不能包含 `*`

仅允许对单个文件执行清除。目前不支持目录级别的清除。
{; note}

## 对于**添加源**命令，Path 字符串有要遵循的任何规则吗？
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

对于**添加源**，路径是**必需**的。此外，如果 CDN 是使用路径创建的，那么**添加源**的路径开头必须以 CDN 路径作为前缀。例如，如果 CDN 路径指定为 `/storage`，要调用路径为 `/examplePath` 的**添加源**，所提供的路径应该为 `/storage/examplePath`。

## 提供文件扩展名的规则是什么？
{: #what-are-the-rules-for-providing-file-extensions}

创建使用 Object Storage 的源时，文件扩展名应该以逗号分隔。例如，`txt, jpg, xml` 是有效的列表。任何一个特定扩展名的长度不能超过 10 个字符。
