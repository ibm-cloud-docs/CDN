---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-26"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Getting to Running status
{: next-steps-after-ordering}

After you've ordered a CDN, it appears on your CDN dashboard. To get your CDN to `Running` status, complete the steps for your particular CDN configuration.
{:shortdesc}

You might need to click the **Get status** button a few times before your CDN shows `Running` status.
{: note}

## For a Wildcard HTTPS CDN
{: for-a-wildcard-https-cdn}

From the Overflow ![Overflow menu](images/overflow.png) menu, click **Get status** until your CDN shows `RUNNING` status. You don't need to configure any other settings or verify that CDN traffic works as expected. From here, you can review HOW TO topics (see contents on left) to configure and manage your CDN.

## For an HTTPS-only CDN
{: for-a-https-only-cdn}

Follow these steps to get your CDN up & running:

1. Verify whether CDN traffic works as expected. For instructions, see [Verifying that your CDN is working before pointing to IBM CNAME](/docs/CDN?topic=CDN-verify-cdn-before-pointing-domain-to-ibm-cname).
2. After you verify that CDN traffic is working, you must change your DNS record to point your domain to IBM CNAME. Most DNS providers can give you instructions on setting up or changing the CNAME.
3. Configure the IBM CNAME with your DNS provider. Until you configure the IBM CNAME, your CDN's status shows as `CNAME configuration required`.

   Check with your DNS provider to find out when the changes become active. Then, add a CNAME record for your CDN domain in DNS. To do so, on the DNS configuration page for your CDN domain, create a CNAME record with your CDN domain name as the **Host** and the IBM CNAME you used to configure the CDN as the **CNAME**. The IBM CNAME ends with `cdn.appdomain.cloud.`.

   A typical CNAME record looks similar to the following:

   ![CNAME record](images/cname.png)

4. When the IBM CNAME chaining is complete, highlight the table row of the CDN and click **Get status** from the Overflow ![Overflow menu](images/overflow.png) menu until your CDN shows `RUNNING` status. Alternatively, if you are on the CDN's details page, click **Actions > Get status**.

Your CDN is now running. From here, you can review HOW-TO topics to further configure and manage your CDN.

## For a SAN HTTPS CDN
{: for-a-san-https-cdn}

Follow these steps to get your CDN up & running:

1. Address the domain validation challenge. For instructions, see [Completing Domain Control Validation for HTTPS with DV SAN](/docs/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san).
2. Verify whether CDN traffic works as expected. For instructions, see [Verifying that your CDN is working before pointing to IBM CNAME](/docs/CDN?topic=CDN-verify-cdn-before-pointing-domain-to-ibm-cname).
3. After you verify that CDN traffic is working, you must change your DNS record to point your domain to the IBM CNAME. Most DNS providers can give you instructions on setting up or changing the CNAME.
4. Configure the IBM CNAME with your DNS provider. Until you configure the IBM CNAME, your CDN's status shows as `CNAME configuration required`.

   Check with your DNS provider to find out when the changes become active. Then, add a CNAME record for your CDN domain in DNS. To do so, on the DNS configuration page for your CDN domain, create a CNAME record with your CDN domain name as the **Host** and the IBM CNAME you used to configure the CDN as the **CNAME**. The IBM CNAME ends with `cdn.appdomain.cloud.`.

   A typical CNAME record looks similar to the following:

   ![CNAME record](images/cname.png)

5. When the CNAME chaining is complete, highlight the table row of the CDN and click **Get status** from the Overflow ![Overflow menu](images/overflow.png) menu until your CDN shows `RUNNING` status. Alternatively, if you are on the CDN's details page, click **Actions > Get status**.

Your CDN is now running. From here, you can review HOW TO topics (see contents on left) to further configure and manage your CDN.
