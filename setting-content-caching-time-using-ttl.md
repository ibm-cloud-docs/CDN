---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-29"

keywords: manage, time to live

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
{:DomainName: data-hd-keyref="DomainName"}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Setting content caching time using "Time To Live"
{: #setting-content-caching-time-using-time-to-live}

After your CDN is running, you can set your content caching time using Time To Live (TTL). The Time To Live for a particular file or directory path indicates how long that content should be cached. When you created the CDN Mapping, a default global TTL of 3600 seconds (1 hour) was created.

To set the content caching time using TTL, follow these steps:

1. On the CDN page, select your CDN, which takes you to the **Overview** page.
2. Adjust the time using the arrows, or by entering a new time. The time value is specified in seconds. For example, 3600 seconds is equal to 1 hour. The smallest value for `timeToLive` that can be chosen is 0 seconds, while the largest is 2147483647 seconds (approximately 24855 days). Select the **Save** button to set the content caching time.

  ![Adding ttl](images/adding-path.png)

3. After saving, you can **Edit** or **Delete** the TTL setting using the Overflow ![Overflow menu](images/overflow.png) options. (**NOTE**: The path for TTL cannot be changed. If the Mapping path is changed, the TTL path is updated automatically.)

  ![Edit or delete ttl](images/edit-delete-ttl-setting.png)

  * When the content matches multiple rules, the most recently added configuration takes precedence.

  * TTL values can be set only for a specific file name or directory. Regular expressions are not supported because they might create unpredictable behavior.
