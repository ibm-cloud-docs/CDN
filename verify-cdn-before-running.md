---

copyright:
  years: 2019, 2020
lastupdated: "2020-10-12"

keywords: traffic, hosts file, dns, cname, record, CNAME configuration required, wildcard

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:DomainName: data-hd-keyref="DomainName"}
{:note: .note}
{:tip: .tip}
{:important: .important}
{:deprecated: .deprecated}
{:generic: data-hd-programlang="generic"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:download: .download}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Verifying that your CDN is working before pointing to IBM CNAME
{: #verify-cdn-before-pointing-domain-to-ibm-cname}
{: help}
{: support}

For **HTTP only** and **SAN HTTPS** CDN mappings only, you must verify that your CDN is working before you switch the domain to use the CDN.
{: shortdesc}

All the commands, tools, and files that are used in this example are based on the `Ubuntu 16.04.5 LTS` system. Your examples might vary if you are using another OS or different Ubuntu versions.
{: note}

This example uses the following CDN mapping:

![CDN list of ibm-test.cdn-demo.com](images/ibm-test.cdn-demo.com.png)

* CDN hostname: `ibm-test.cdn-demo.com`
* CDN CNAME: `cdnakaotn3exg6px.cdn.appdomain.cloud.`
* Origin path: `/*`
* Origin port: `80` & `443`
* Origin IP address: `119.xx.xx.xx`
* Origin host header: `ibm-test.cdn-demo.com`
* Certificate type: `SAN HTTPS`
* The current CDN status: `CNAME configuration required`

The domain `ibm-test.cdn-demo.com` is pointing to the Origin server:

```shell
dig +short ibm-test.cdn-demo.com
119.xx.xx.xx
```
{: pre}

and a service is running on it:

```shell
curl -s -o origin-index.html -D - https://ibm-test.cdn-demo.com/index.html
HTTP/1.1 200 OK
Server: nginx/1.14.0
Date: Tue, 18 Feb 2020 05:40:45 GMT
Content-Type: text/html
...
```
{: pre}

If services are running on your domain, it is recommended to verify CDN functionality before you migrate your domain to CDN. You can use the following methods to verify CDN traffic.  

* Changing local hosts to verify CDN traffic
* Using wildcard CDN to verify CDN traffic

## Changing local hosts to verify CDN traffic
{: #changing-local-hosts-to-verify-cdn-traffic}

When CDN-mapping status is in `CNAME configuration required`, all the configurations in IBM and on the Akamai side (for example, DNS, policies, certificates) are ready for you to point the hostname to the IBM CNAME.

In almost all operating systems, there is a local hosts file that maps hostnames to IP addresses. Changes to the hosts file only affect your own computer, not how the domain is resolved worldwide. It's safe to point the domain to IBM CNAME by changing the local hosts file. To do so, follow these steps:

1. Get the Akamai Edge server's IP address.

   If the CDN certificate type is SAN HTTPS, then you can get the Edge server IP address by performing name resolution of [IBM CNAME](/docs/CDN?topic=CDN-getting-to-running-status#ibm-cname). If the CDN type is HTTP only, then you can use the fixed domain `wildcard.cdnedge.bluemix.net.edgekey.net` to get the IP address. You can use the `dig` command to perform the resolution.

   In this example, the CDN is a SAN HTTPS mapping, so its IBM CNAME is used to do the resolution. The `dig` result is similar to the following:

   ```bash
   dig +short cdnakaotn3exg6px.cdn.appdomain.cloud.
   cdnakaotn3exg6px.akamai.cdn.appdomain.cloud.
   cert-00033-cdnedge-bluemix.akamaized.net.edgekey.net.
   e24455.dsce16.akamaiedge.net.
   104.84.150.124
   104.84.150.67
   ```
   {: pre}

   The DNS chain and the IP addresses resolved on your machine might be different.
   {: tip}

2. Make a backup copy of your original local hosts file. To do so, use an editor to open your local hosts file `/etc/hosts`, and add the IP-hostname entry into it. You can choose any of the IP addresses resolved in the prior step.

   For example:

   ```
   104.84.150.124 ibm-test.cdn-demo.com
   ```
   {: screen}

3. Verify the hosts change. You can use the `ping` command to resolve and test the IP address.

   ```bash
   ping ibm-test.cdn-demo.com
   PING ibm-test.cdn-demo.com (104.84.150.124) 56(84) bytes of data.
   64 bytes from ibm-test.cdn-demo.com (104.84.150.124): icmp_seq=1 ttl=59 time=1.66 ms
   64 bytes from ibm-test.cdn-demo.com (104.84.150.124): icmp_seq=2 ttl=59 time=1.42 ms
   64 bytes from ibm-test.cdn-demo.com (104.84.150.124): icmp_seq=3 ttl=59 time=1.52 ms
   64 bytes from ibm-test.cdn-demo.com (104.84.150.124): icmp_seq=4 ttl=59 time=1.42 ms
   ^C
   --- ibm-test.cdn-demo.com ping statistics ---
   4 packets transmitted, 4 received, 0% packet loss, time 3005ms
   rtt min/avg/max/mdev = 1.420/1.507/1.662/0.099 ms
   ```
   {: pre}

   This result shows that the IP address of `ibm-test.cdn-demo.com` was resolved to Akamai Edge server's IP address (`104.84.150.124`)and that the `/etc/hosts` file change took effect.

4. Verify the CDN traffic. You can add the [Akamai debug headers](/docs/CDN?topic=CDN-troubleshoot-cdn-working) to request the domain.

   ```bash
   curl -s -o cdn-test-index.html -D - \
       -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no, akamai-x-get-request-id,akamai-x-get-nonces,akamai-x-get-client-ip,akamai-x-feo-trace" \
       https://ibm-test.cdn-demo.com/index.html
   HTTP/1.1 200 OK
   Server: nginx/1.14.0
   Content-Type: text/html
   ...
   Date: Tue, 18 Feb 2020 07:16:56 GMT
   Content-Length: 1524
   ...
   ...
   X-Check-Cacheable: YES
   ```
   {: pre}

   If Akamai debug headers are returned, and the response code and content are the same as before, then the CDN traffic works. See [Troubleshooting](/docs/CDN?topic=CDN-troubleshoot-cdn-working) for further debugging tips.

   Along with the basic function verification, it is recommended that you verify more functions that you plan to use; for example, caching, multiple origins and purge.
   {: important}

6. Restore your original local hosts file by using the backup copy that was created in Step 2.   

## Using wildcard CDN to verify CDN traffic
{: using-wildcard-cdn-to-verify-cdn-traffic}

The wildcard CDN does not need the DNS record changed to point the domain to the IBM CNAME. The IBM CNAME can be used to access the service, which makes it fast and easy to verify CDN traffic.

To verify CDN traffic using a wildcard certificate, follow these steps:

1. Create a wildcard CDN mapping with a new temporary domain (for example `ibm-test-wildcard.cdn-demo.com`). This wildcard CDN is only for testing, and it must have the same configuration as the wanted CDN `ibm-test.cdn-demo.com`. The following configurations, at a minimum, must be the same:

   * Origin path
   * Origin port type and number
   * Origin hostname
   * Origin hostheader

  The wildcard CDN shows a final list similar to the following image.

   ![CDN list of ibm-test.cdn-demo.com](images/ibm-test.cdn-demo.com-wildcard-test.png)

2. Verify the wildcard traffic. You can add the [Akamai debug headers](/docs/CDN?topic=CDN-troubleshoot-cdn-working) to request the wildcard CDN's IBM CNAME.

   ```bash
   curl -s -o cdn-test-wildcard-index.html -D - \
       -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no, akamai-x-get-request-id,akamai-x-get-nonces,akamai-x-get-client-ip,akamai-x-feo-trace" \
       https://ibm-test-wildcard.cdn.appdomain.cloud/index.html
   HTTP/1.1 200 OK
   Server: nginx/1.14.0
   Content-Type: text/html
   ...
   Date: Tue, 18 Feb 2020 07:16:56 GMT
   Content-Length: 1524
   ...
   ...
   X-Check-Cacheable: YES
   ```
   {: pre}

   If Akamai debug headers are returned, and the response code and content match the content that is returned from the wanted domain, the wildcard CDN traffic works. See the Troubleshooting section for further debugging tips.

     Along with the basic function verification, it is recommended that you verify more functions that you plan to use; for example, caching, multiple origins, purge, and so on.
     {: important}
