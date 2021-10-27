---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-12"

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

# Purging cached content
{: #purging-cached-content}

When your CDN in [these statuses](/docs/CDN?topic=CDN-faqs#what-status-is-cdn-allowed-for-multiple-file-purge), you can purge the cached contents in multiple paths from the vendor's server.
{: shortdesc}

1. On the CDN page, select your CDN, which takes you to the **Overview** page.
2. Select the **Purge** tab.

   ![Purge page](images/purge-tab.png){: caption="Purge tab" caption-side="top"}

3. Click the **Create purge** button.
4. Enter standard UNIX path syntax or upload a local purge file to indicate which paths you want to purge, then click the **Purge** button. You can purge multiple paths at a time. See [FAQs for rules and naming conventions](/docs/CDN?topic=CDN-rules-and-naming-conventions#what-are-the-rules-for-the-path-string-for-purge) for more details on what syntax or a local file is allowed for the Purge paths.

   ![Purge page](images/purge-create-dialog.png){: caption="Purge button" caption-side="top"}

5. After purging, the group is listed under **Purge history**. If the process succeeds, the cached contents of the path list in the Edge server are cleared. If you want to save a group for future use without performing a purge operation, click the **Add paths** button instead.

   ![Purge page](images/purge-history-list.png){: caption="Purge history" caption-side="top"}

6. With a specified group you can **Redo purge**, **View purge paths**, or **Add to favorites** using the Overflow ![Overflow menu](images/overflow.png) menu options.

   ![Purge page](images/purge-history-options.png){: caption="Purge history options" caption-side="top}

   By default, the purge groups in the **Purge history** are not saved to favorites and trimmed every 15 days automatically. To save purge groups for future use, you can save them as favorites by choosing the **Add to favorites** option. When a purge group is saved, it is not be automatically trimmed.

   The name and path list for a purge group that is listed in **Favorite paths** and **Purge history** cannot be updated at present.
   {: note}
