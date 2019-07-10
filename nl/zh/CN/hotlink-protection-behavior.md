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

# 热链接保护类
{: #hotlink-protection-class}

`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` 类包含热链接保护 API 利用的属性。此对象用于通过调用 API 来设置 CDN 的热链接保护行为。在成功调用 API 之后，热链接保护 API 也会返回该值。

**类** `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`：

* **不**支持 URL 的第一个标签中的 `://`。
   * **有效**：`http*www.example.com`
   * **无效**：`http://www.example.com`

* `protectionType`：指定当 HTTP 请求具有与 `refererValues` 中的某一项匹配的 Referer 头值时，允许或拒绝对内容的访问。当没有匹配项时，会发生相反的操作。
  * protectionType 的可能值：
    * `ALLOW`
    * `DENY`
* `refererValues`：是 referer URL 的单空格分隔列表，也可能有字符串形式的通配符匹配。
  * `refererValues` 的**有效**字符串的一些示例：
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * `refererValues` 的**无效**字符串的一些示例：
   
      |**无效示例的描述**| 示例
      |-------|-----|
      | 总共包含超过 2100 个字符|`www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |包含其字符数超过 255 个的至少一个 URL 匹配项| `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |列表中包含重复项|`domain1.example.com domain1.example.com`|
      |refererValues 为空| ` `|
      |使用 [RFC-3986 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://tools.ietf.org/html/rfc3986#section-2) 中未指定的字符|`domain1.exa}mple.com domain1.example.com`|
      |不支持 `&` 字符|`www.example.com/path&`|
      |不支持这样的 `refererValues` 字符串：带有至少一个 URL 匹配项，在第一个 `.` 字符前边的字符集中包含 `://`|`www.example.org http://www.example.com`|


