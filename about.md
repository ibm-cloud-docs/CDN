---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# About Content Delivery Networks (CDN)

A Content Delivery Network (CDN) is a collection of edge servers that are distributed through various parts of the country or the world. Web content is served from an edge server, which is located in the geographic area closest to the customer who requests the content. This technique lets your end-users receive the content with less delay, and it delivers a better overall experience for your customers.

## How does a CDN work?

A CDN achieves its purpose by caching web content on edge servers around the world. When a user requests web content, the content request is routed to the edge server that is geographically closest to that user. By reducing the distance the content must travel, the CDN offers optimized throughput, minimized latency, and increased performance. Using IBM Cloud Content Delivery Network with Akamai, content providers can realize efficient delivery of requested content from around the globe, with minimal configuration.

The key features of the IBM Cloud Content Delivery Network service include:
* Host Server and Object Storage support 
* HTTPS support with wildcard certificate
* Ability to specify caching behavior at global level and at file level
* Ability to Purge cached content and save purge file path for future re-use
* Graphical view of metrics and total bandwidth usage per geographical region
* Support for multiple origins
