---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-11"

keywords: IBM, Content Delivery Network, CDN, Audit log, activity tracker, LogDNA, event

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

# {{site.data.keyword.cloudaccesstrailshort}} events for CDN
{: #at_events}

Use the Activity Tracker service to track how users and applications interact with the CDN service in {{site.data.keyword.cloud}}.
{: shortdesc}

{{site.data.keyword.cloudaccesstrailshort}} records user-initiated activities that change the state of {{site.data.keyword.cloud_notm}} Content Delivery Network (CDN) service. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.cloudaccesstrailshort}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started).

## List of Domain Mapping events
{: #events_domain_mapping}

The following table lists the actions taken related to Domain Mappings. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.domain-mapping.create | Create a domain mapping.     | 
| cdn-powered-by-akamai.domain-mapping.update | Update a domain mapping.     | 
| cdn-powered-by-akamai.domain-mapping.delete | Delete a domain mapping.     | 
| cdn-powered-by-akamai.domain-mapping.stop | Stop a domain mapping.     | 
| cdn-powered-by-akamai.domain-mapping.start | Start a domain mapping.     | 
| cdn-powered-by-akamai.domain-mapping.status | Domain mapping status change.     | 

## List of Origin Path Action events
{: #events_origin_path}

The following table lists the actions taken related to Origin Path Actions. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.origin-path.create | Create an origin path.     | 
| cdn-powered-by-akamai.origin-path.update | Update an origin path.     | 
| cdn-powered-by-akamai.origin-path.delete | Delete an origin path.     | 

## List of Time To Live events
{: #events_ttl}

The following table lists the actions taken related to TTL. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.ttl.create | Create TTL.     | 
| cdn-powered-by-akamai.ttl.update | Update TTL.     | 
| cdn-powered-by-akamai.ttl.delete | Delete TTL.     | 

## List of Purge events
{: #events_purge}

The following table lists the actions taken related to purge. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.purge.create | Create purge.     | 
| cdn-powered-by-akamai.purge.save | Save purge.     | 
| cdn-powered-by-akamai.purge.unsave | Unsave purge.     | 

## List of CDN Account events
{: #events_account}

The following table lists the actions taken related to CDN account. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.account.create | Create CDN account.     | 
| cdn-powered-by-akamai.account.cancel | Cancel CDN account.     | 
| cdn-powered-by-akamai.account.delete | Delete CDN account.     | 

## List of Geolocation events
{: #events_geo}

The following table lists the actions taken related to geolocation. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.geo.create | Create Geo location.     | 
| cdn-powered-by-akamai.geo.update | Update Geo location.     | 
| cdn-powered-by-akamai.geo.delete | Delete Geo location.     | 

## List of Hotlink events
{: #events_hotlink}

The following table lists the actions taken related to hotlink. With each of these actions, an event has been generated:

| Action                                              | Description                                            | 
|:-----------------------------------------------|:----------------------------------------------------|
| cdn-powered-by-akamai.hotlink.create | Create hotlink.     | 
| cdn-powered-by-akamai.hotlink.update | Update hotlink.     | 
| cdn-powered-by-akamai.hotlink.delete | Delete hotlink.     | 


## Viewing events
{: #ui}

{{site.data.keyword.cloudaccesstrailshort}} events that are generated by an instance of the CDN service are automatically forwarded to the {{site.data.keyword.cloudaccesstrailshort}} service instance that is available in the same location. Currently, the {{site.data.keyword.cloudaccesstrailshort}} Events for {{site.data.keyword.cloud}} CDN service are available only on IBM Cloud - EU-DE(Frankfurt ) Region.

[Provision an {{site.data.keyword.cloudaccesstrailshort}} instance](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-provision) if you don't have one. {{site.data.keyword.cloudaccesstrailshort}} can have only one instance per location.

To view events, access the web UI of the {{site.data.keyword.cloudaccesstrailshort}} service in the same location where your service instance is available. For more information, see [Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-launch#launch_step2).

You can apply search and filtering criteria to define the events that are displayed through a [custom view](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-views).
