---

copyright:
  years: 2024
lastupdated: "2024-06-27"

keywords: object storage, bucket

subcollection: CDN

---

{{site.data.keyword.attribute-definition-list}}

# Migration options
{: #migration-options}

CDN is scheduled for End of Marketing and eventually End of Service. In preparation for this transition, it is recommended that you migrate your CDN mapping to {{site.data.keyword.cis_full}} ({{site.data.keyword.cis_short_notm}}) or create an Akamai account if you prefer to continue with Akamai CDN.
{: shortdesc}

## Migrating CDN to IBM Cloud Internet Services
{: #migrating-cdn-to-cis}

The following sections describe the methods of migrating your CDN instances to {{site.data.keyword.cis_short_notm}}.

### Migrating Wildcard CDN to {{site.data.keyword.cis_short_notm}}
{: #migrating-wildcard}

In this example, an existing Wildcard CDN instance with the following configurations is about to be moved from CDN to {{site.data.keyword.cis_short_notm}}:

* **Hostname**: `cdn-demo.slcdnservice.net`
* **Current IBM CNAME**: `cdn-demo.cdn.appdomain.cloud`

With the following two different types of origins, part of the configuration steps will be different. 

* A [static website](/docs/cloud-object-storage?topic=cloud-object-storage-static-website-tutorial&interface=ui) hosted on IBM Cloud Object Storage (ICOS) and has a format like `http://<bucketname>.s3-web.<endpoint>/`. 
* An origin that has a public IP. 

After migration, the hostname `cdn-demo.slcdnservice.net` is moved to {{site.data.keyword.cis_short_notm}} and can be used to access the website.

This instance is being moved in a non-disruptive way. The IBM provided CDN CNAME `cdn-demo.cdn.appdomain.cloud` continues working until migration is complete; the parent domain `slcdnservice.net` is not moved to {{site.data.keyword.cis_short_notm}}.
{: tip}

#### Before you begin
{: #prerequisites-migration}

1. Create a {{site.data.keyword.cis_short_notm}} instance on IBM Cloud Catalog. See [Getting Started with {{site.data.keyword.cis_short_notm}}](/docs/cis?topic=cis-getting-started) for more information.
   
   To make sure you can utilize the {{site.data.keyword.cis_short_notm}} caching capabilities, choose an Enterprise plan. 
   {: note}
   
1. Prepare [IBM Cloud CLI](/docs/cli?topic=cli-getting-started#getting-started).
1. Log in to IBM Cloud CLI and install {{site.data.keyword.cis_short_notm}} CLI. See [{{site.data.keyword.cis_short_notm}} CLI reference](/docs/cis?topic=cis-cis-cli) for more details about how to log in and access {{site.data.keyword.cis_short_notm}}. 


#### Set up DNS zone CNAME
{: #create-a-partial-zone}

1. Set the instance context in CLI.

   ```sh
   ibmcloud cis instance-set "<your-instance-name>"
   ```
   {: pre}
  
1. Create a partial type of CNAME zone for your domain (in this example, `cdn-demo.slcdnservice.net`). For more information about adding a partial zone, see [Add a domain](/docs/cis?topic=cis-cis-cli#add-domain) and [DNS zone CNAME partial setup](/docs/cis?topic=cis-cname-setup).

   ```sh
   ibmcloud cis domain-add "cdn-demo.slcdnservice.net" --type partial
   ```
   {: pre}
   
1. Get the TXT record `verification_key` and `cname_suffix` from the response:
   
    ```text
    {
    "result": {
        "id": "1df93abfb59849abd3e34fde156a4c21",
        "name": "cdn-demo.slcdnservice.net",
        "status": "pending",
        "paused": false,
        "verification_key": "476754457-428595283",
         "cname_suffix": "cdn.cloudflare.net",
        "original_name_servers": [
            "ns1.softlayer.com",
            "ns2.softlayer.com"
        ],
        "original_registrar": "everyones internet, ltd. dba s (id: 925)",
        "original_dnshost": null,
        "modified_on": "2021-05-07T06:46:19.326826Z",
        "created_on": "2021-05-07T01:57:53.163247Z",
        "account": {
            "id": "b0c53e3f037b8cdc62b5cb373b8c55e6",
            "name": "57aea3aa-a38e-4760-ada5-a698bca56171"
        }
    },
    "success": true,
    "errors": [],
    "messages": []
    }
    ```
    {: screen}

1. With the authoritative DNS provider of your domain, add a TXT record `cloudflare-verify` to the DNS zone (in this example, `cdn-demo.slcdnservice.net`) pointing to the verification key (in this example,`476754457-428595283`). Wait for a few hours for the change to take effect.

   ```sh
   txt cloudflare-verify.cdn-demo.slcdnservice.net  476754457-428595283
   ```
   {: pre}
   
#### Set up the DNS records
{: #verify-partial-zone}

1. With {{site.data.keyword.cis_short_notm}}, get your domain ID from the output of the following command.

   ```sh
   ibmcloud cis domains
   ```
   {: pre}
   
1. Depending on your origin type, add a CNAME record or an A record for `cdn-demo.slcdnservice.net` and enable proxy. 
  
   * If your origin is a static website hosted on ICOS: Add A CNAME record to associate the domain `cdn-demo.slcdnservice.net`.
 
   ```sh
   ibmcloud cis dns-record-create <your-domain-ID> --type CNAME --name cdn-demo.slcdnservice.net --content <bucketname>.s3-web.<endpoint> --proxied true
   ```
   {: pre}
   
   * If your origin is a website or application with a public IP, add an A record. 
   
    ```sh
   ibmcloud cis dns-record-create <your-domain-ID> --type A --name cdn-demo.slcdnservice.net --content <your-origin-public-IP> --proxied true
   ```
   {: pre}
   
1. Before moving the domain officially to {{site.data.keyword.cis_short_notm}}, verify the subdomain changes on your local server. A typical way of doing this is to add a row to the */etc/hosts* file on your local server.
   
   ```sh
   <CIS-proxy-IP> cdn-demo.slcdnservice.net 
   ```
   {: pre}
   
   Where `<CIS-proxy-IP>` can be retrieved by the following command: 
   
   ```sh
   dig cdn-demo.slcdnservice.net.cdn.cloudflare.net a +short
   ```
   {: pre}
   
   You can also use the **curl** command to verify the setting on your local server. 
   
   ```sh
   curl --resolve cdn-demo.slcdnservice.net:443:<CIS-proxy-IP> https://cdn-demo.slcdnservice.net
   ```
   {: pre}

1. Add a CNAME record with the `cdn.cloudflare.net` suffix in the authoritative DNS so that the domain can be moved to {{site.data.keyword.cis_short_notm}}.

   ```sh
   cdn-demo.slcdnservice.net CNAME cdn-demo.slcdnservice.net.cdn.cloudflare.net
   ```
   {: pre}
   
1. Verify the CNAME settings.

   ```sh
   dig cdn-demo.slcdnservice.net a
   ```
   {: pre}
   
   In the resulting screen, if the domain is migrated successfully, you can see something similar to the following example in the **ANSWER SECTION**:
   
   ``` ...
       ;; ANSWER SECTION:
    cdn-demo.slcdnservice.net. 900	IN	CNAME	cdn-demo.slcdnservice.net.cdn.cloudflare.net.
    cdn-demo.slcdnservice.net.cdn.cloudflare.net. 300 IN A 104.18.3.72
    cdn-demo.slcdnservice.net.cdn.cloudflare.net. 300 IN A 104.18.2.72
       ...
   ```
   {: screen}
   
   You can also verify whether the status of the domain has been changed to *active*.
   
   ```sh
   ibmcloud cis domain <your-domain-ID> -i <your-instance-name>
   ```
   {: pre}
   
   After this status is verified, if you have changed the */etc/hosts* file in the previous steps for verification purpose, remove the added line. 
   {: note}
   
### Additional steps for an ICOS static website origin
{: #additional-steps-for-icos}

If you are configuring an ICOS static website origin, additional steps are required to finish migrating your CDN instance to {{site.data.keyword.cis_short_notm}}.

#### Add another CNAME record in {{site.data.keyword.cis_short_notm}}
{: #add-another-cname-record-in-cis}

Within {{site.data.keyword.cis_short_notm}}, add another CNAME record for `cos.cdn-demo.slcdnservice.net` and enable proxy. The CNAME configuration for `cos.cdn-demo.slcdnservice.net` together with the upcoming page rule settings will ensure the domain `cdn-demo.slcdnservice.net` points to the origin website successfully.

   ```sh
   ibmcloud cis dns-record-create <domain_ID> --type CNAME --name cos.cdn-demo.slcdnservice.net --content <bucketname>.s3-web.<endpoint> --proxied true
   ```
   {: pre}
     
#### Configure page rules with {{site.data.keyword.cis_short_notm}}
{: #configure-rules-partial-zone}

1. Within {{site.data.keyword.cis_short_notm}}, navigate to **Performance** and the **Page Rules** tab. 
1. Create two rules for URLs that match `cdn-demo.slcdnservice.net/*`. 
1. Enable these two rules.
   
| **Behavior** | **Setting** |
|--------------|-------------|
| Resolve Override | `cos.cdn-demo.slcdnservice.net` |
| Host header override | `<bucketname>.s3-web.<endpoint>`|
{: caption="Table 1. Page rule configurations" caption-side="bottom"}
   
### Verify the configuration and delete the CDN instance
{: #verify-configuration-delete-cdn}

1. Verify that the domain `cdn-demo.slcdnservice.net` is accessible and points to the static website hosted on IBM Cloud Object Storage.
1. Delete the existing CDN instance as described in [deleting a CDN](/docs/CDN?topic=CDN-deleting-a-cdn).

### Migrating DV SAN CDN to {{site.data.keyword.cis_short_notm}}
{: #migrating-dv-san-cnd-cis}

In this example, an existing DV SAN CDN instance with the following configurations is about to be moved from CDN to {{site.data.keyword.cis_short_notm}}:

* **Hostname**: `site.slcdnservice.net`
* **Current IBM CNAME**: `cdn-trial.cdn.appdomain.cloud`

With a DV SAN CDN, all the migration steps are the same as a [Wildcard CDN](#migrating-wildcard). After migration, the hostname `site.slcdnservice.net` is moved to {{site.data.keyword.cis_short_notm}}. 

* During migration, the domain `site.slcdnservice.net` will experience a CNAME change from an Akamai CNAME to a Cloudflare CNAME. 
* After migration, a new dedicated certificate issued by {{site.data.keyword.cis_short_notm}} will be associated with your domain replacing the previous shared certificate.

## Migrating CDN to Akamai
{: #migrating-cdn-to-akamai}

Alternatively, if you prefer to use Akamai CDN after the End of Marketing date, you can migrate to Akamai by creating an Akamai account. You can either inquire on the [Akamai website](https://www.akamai.com/why-akamai/contact-us/contact-sales){: external} to be contacted by sales, or can contact our designated account manager (jomorton@akamai.com) for this migration to be redirected to your assigned account team.

## Migrating CDN to third-party CDN services on {{site.data.keyword.cloud_notm}}
{: #migrating-cdn-third-party-services}

In addition, if you are looking for more CDN options, navigate to the [IBM Cloud catalog](/catalog) and choose from the third-party [CDN services](/catalog?search=CDN#search_results) for a rich collection of configurable features, for example, [Cloudsway CDN](/catalog/services/cloudsway-cdn).
