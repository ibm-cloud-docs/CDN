---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-08-06"

keywords: manage, time to live, origin path, cache key, server, object storage, bucket, configuration, details, updating

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

# Manage your CDN
{: #manage-your-cdn}

This document describes common tasks for managing your CDN.

## Setting content caching time using "Time To Live"
{: #setting-content-caching-time-using-time-to-live}

After your CDN is running, you can set your content caching time using Time To Live (TTL). The Time To Live for a particular file or directory path indicates how long that content should be cached. When you created the CDN Mapping, a default global TTL of 3600 seconds (1 hour) was created.

**Step 1:**  

On the CDN page, select your CDN, which takes you to the **Overview** page.

**Step 2:**  

You can adjust the time using the arrows or by entering a new time. The time value is specified in seconds. For example, 3600 seconds is equal to 1 hour. The smallest value for `timeToLive` that can be chosen is 0 seconds, while the largest is 2147483647 seconds (approximately 24855 days). Select the **Save** button to set the content caching time.

  ![Adding ttl](images/adding-path.png)

**Step 3:**

After saving, you can **Edit** or **Delete** the TTL setting using the overflow menu options. (**NOTE**: The Path for TTL cannot be changed. If the Mapping path is changed, the TTL path is updated automatically.)

  ![Edit or delete ttl](images/edit-delete-ttl-setting.png)  

  * When the content matches multiple rules, the most recently added configuration takes precedence.

  * TTL values can be set only for a specific file name or directory. Regular expressions are not supported, because they may create unpredictable behavior.

## Adding Origin Path details
{: #adding-origin-path-details}

When your CDN is in *CNAME_Configuration* or *Running* status, you can add Origin Path details. You can choose to provide content from multiple Origin Servers. For example, photos can be delivered from a different server than videos. The Origin can be based upon a Host Server or Object Storage.

The CDN makes a URL transformation for the origin server. For example, if origin `xyz.example.com` is added with path `/example/*` when a user opens the URL `www.example.com/example/*`, the CDN edge server retrieves the content from `xyz.example.com/*`.
{: note}

**Step 1:**

On the CDN page, select your CDN, which takes you to the **Overview** page.  

**Step 2:**

Select the **Origins** tab, then select the **Add Origin** button. This step opens a new dialog window, where you can configure your Origin.  

   ![Origins add origin](images/add-origins.png)

**Step 3:**

You *must* provide a path. You may optionally provide a host header.  

   ![Origins add origin](images/add-origin-path.png)

**Step 4:**

Select either **Server** or **Object Storage**.

  * If you selected **Server**, enter the Origin server address as IPv4 address or the _hostname_. It is recommended to provide the hostname and provide a Fully Qualified Domain Name (FQDN). Depending on which protocol you selected during CDN creation, also provide an HTTP port, an HTTPS port, or both. If you use an HTTPS port, the Origin server address **must** be a _hostname_ and not an IP address.

       ![Add origin server](images/add-origin-server-default.png)

  * If you selected **Object Storage**, provide the Endpoint, Bucket name, and HTTPS port. Optionally, specify the file extensions that can be used in the CDN service. If nothing is specified, all file extensions are allowed.

       ![Add origin object storage](images/add-origin-object-storage.png)

  * **Optimization** and **Cache Key** options are the same for the Server and the Object Storage configurations.

    * Choose **Optimization** options from the drop-down menu. **General web delivery** is the default option, or you can choose **Large file** or **Video on demand** optimizations. **General web delivery** allows the CDN to serve content up to 1.8GB, while **Large file** optimization allows downloads of files from 1.8GB to 320GB. **Video on demand** optimizes your CDN for delivery of segmented streaming formats. The Feature descriptions for [Large file optimization](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#large-file-optimization) and [Video on Demand](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#video-on-demand) provide further information.

        ![Performance configuration options](images/performance-config-options.png)

    * Choose **Cache Key** options from the drop-down menu. The default option is **Include-all**. If you select **Include specified** or **Ignore specified**, you **must** enter query strings to be included or ignored, separated by a space. For example, enter `uuid=123456` for a single query string, or `uuid=123456 issue=important` for two query strings.  You can find out more about [Cache Key Query Args](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#cache-key-query-args) in the feature description.

        ![Cache key options](images/cache-key-options.png)

The Protocol and Port options shown by the UI will match what was selected when you ordered the CDN. For example, if **HTTP port** was selected as part of ordering a CDN, only the **HTTP port** option is shown as part of Add Origin.
{: note}

**Step 5:**

Select the **Add** button to add your Origin Path.

When you provide file extensions for an Object Storage origin path, the TTL setting with the same URL as the origin path is scoped to include all files that have those specified file extensions. For example, if you create an origin path of `/example` and you specify file extensions of "jpg png gif", the TTL value of the TTL path `/example` will have a scope that includes all JPG/PNG/GIF files under the `/example` directory and its sub-directories.
{: note}

**Step 6:**

After adding, you can  **Edit** or **Delete** the Origin using the overflow menu options.

  ![Edit or delete Origin](images/edit-delete-origin.png)

## Purging Cached Content
{: #purging-cached-content}

After your CDN is running, you can purge cached content from the Vendor's server.

**Step 1:**

On the CDN page, select your CDN, which takes you to the **Overview** page.

**Step 2:**

Select the **Purge** tab.

   ![Purge page](images/purge_tab.png)

**Step 3:**

Enter standard Unix path syntax to indicate which file you would like to purge, then select the **Purge** button. Purge is allowed only for a single file at a time. Please see the [Rules and Naming Conventions](/docs/infrastructure/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge-) page for more details on what syntax is allowed for the Purge path.

**Step 4:**

After purging, the activity is listed under **Purge Activity**. You can **Redo purge** or **Favorite** the path using the overflow menu options.

   ![Purge activity](images/purge-activity.png)

If there are more than 15 purges, Purge Activity is trimmed every 15 days automatically.
{: note}

## Updating CDN Configuration details
{: #updating-cdn-configuration-details}

After your CDN is running, you can update CDN configuration details.

**Step 1:**

On the CDN page, select your CDN, which takes you to the **Overview** page.

**Step 2:**

Select the **Settings** tab. Your CDN configuration details are displayed.

   ![Settings tab](images/settings-tab.png)  

You will only see SSL Certificate if your CDN was configured with HTTPS.
{: note}

For **Server**, the following fields can be changed:
  * Host header
  * Origin server address
  * HTTP/HTTPS Port
  * Serve Stale Content
  * Respect Headers
  * Optimization options
  * Cache-query    

For **Object Storage**, the following fields can be changed:
  * Host header
  * Endpoint
  * Bucket name
  * HTTPS Port
  * Allowed file extensions
  * Serve Stale Content
  * Respect Headers
  * Optimization options
  * Cache-query

**Step 3:**

Update the **Origin** or **Other Options** details if needed, then click the **Save** button in the bottom right corner to update your CDN configuration details.

   ![Save button](images/save-button.png)
