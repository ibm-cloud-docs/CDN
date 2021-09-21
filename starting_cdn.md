---

copyright:
  years: 2020
lastupdated: "2020-03-29"

keywords:

subcollection: CDN

---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Starting a CDN
{: #starting-cdn}

Starting your CDN informs the DNS to direct traffic from your origin to the Akamai Edge server. After the mapping is started, the DNS cache can still direct traffic to the origin so the functionality might not be seen by the domain immediately after the mapping is started. The time it takes to update depends on how often the DNS cache is refreshed, and varies depending on your DNS provider.
{: shortdesc}

You can start a CDN only when it is in `Stopped` status.
{: note}

1. Click **Start CDN** from the Overflow ![Overflow menu](images/overflow.png) menu.

    You are prompted if you want to start the service.

2. Select **Confirm** to proceed.

    If the action was successful, a message appears in the upper right corner of the page, letting you know that it was successful.

    This step changes the status to `CNAME Configuration`.

3. Click **Get status** from Overflow menu. The status shows `Running`.
