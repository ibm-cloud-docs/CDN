---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-29"

keywords:

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
{: shortdesc}

To set the content caching time using TTL, follow these steps:

1. Open the page of a specified CDN mapping, click **Settings** from the navigation pane.
2. Click **Add path** from the **TTL settings**.
3. Adjust the time using the arrows, or by entering a new time. The time value is specified in seconds. For example, 3600 seconds is equal to 1 hour. The smallest value for `timeToLive` that you can choose is 0 seconds, while the largest is 2147483646 seconds (approximately 24855 days). Select the **Save** button to set the content caching time.

   ![Adding TTL](images/adding-path.png){: caption="Add TTL rule" caption-side="bottom"}

4. After saving, you can **Edit** or **Delete** the TTL setting using the Overflow ![Overflow menu](images/overflow.png) options. (**NOTE**: The path for TTL cannot be changed. If the Mapping path is changed, the TTL path is updated automatically.)

   ![Edit or delete TTL](images/edit-delete-ttl-setting.png){: caption="Edit or delete TTL" caption-side="bottom"}

   * When the content matches multiple rules, the most recently added configuration takes precedence.

   * TTL values can be set only for a specific file name or directory. Regular expressions are not supported because they might create unpredictable behavior.
